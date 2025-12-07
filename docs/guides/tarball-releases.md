# Versioned Tarball Releases

**Last Updated**: 2025-12-06  
**Status**: Production Ready

This guide explains how to implement versioned release workflows for TFGrid Compose applications. This pattern enables atomic deployments, instant rollbacks, and in-place updates without recreating VMs.

---

## When to Use This Pattern

Use versioned tarball releases when you need:

- **Long-lived VMs** - Update code without destroying infrastructure
- **Atomic deployments** - All-or-nothing updates that can't leave partial state
- **Instant rollback** - Revert to any previous version in seconds
- **Compiled binaries** - Ship pre-built artifacts instead of building on the VM
- **Production stability** - Tested releases promoted through environments (dev → staging → prod)
- **Audit trail** - Know exactly which version is running at any time

---

## Workflow Overview

```
Build → Package → Ship → Deploy → Verify → (Rollback if needed)
```

| Phase | Description | Where |
|-------|-------------|-------|
| **Build** | Compile binaries, bundle assets | Ops machine / CI |
| **Package** | Create versioned tarball | Ops machine / CI |
| **Ship** | Transfer tarball to VM | rsync / HTTP download |
| **Deploy** | Extract, update symlinks, restart | On the VM |
| **Verify** | Health checks | On the VM |
| **Rollback** | Switch to previous version | On the VM |

---

## Directory Structure

### On the VM

```
/opt/my-app/
├── releases/
│   ├── v1.0.0/              # Previous release
│   │   ├── bin/
│   │   ├── assets/
│   │   └── VERSION
│   ├── v1.1.0/              # Current release
│   │   ├── bin/
│   │   ├── assets/
│   │   └── VERSION
│   └── current -> v1.1.0    # Symlink to active release
├── config/
│   └── app.env              # Persistent configuration
├── data/                    # Persistent data (not in releases)
├── uploads/                 # Incoming tarballs
├── commands/                # Release management scripts
│   ├── deploy-release.sh
│   ├── rollback-release.sh
│   └── list-releases.sh
└── logs/
```

### Key Principles

1. **Releases are immutable** - Never modify files in `/opt/my-app/releases/<version>/`
2. **Current is a symlink** - Atomic switch between versions
3. **Config is separate** - Persistent configuration outside releases
4. **Data is persistent** - Database, uploads, etc. survive releases

---

## Building Release Tarballs

### Tarball Contents

A release tarball contains everything needed to run a specific version:

```
my-app-v1.1.0.tar.gz
├── bin/                     # Compiled binaries
│   ├── server
│   └── worker
├── assets/                  # Static files, frontend builds
│   └── dist/
├── commands/                # Updated command scripts
│   ├── deploy-release.sh
│   └── rollback-release.sh
├── scripts/                 # Helper scripts
│   └── migrate.sh
└── VERSION                  # Version marker file
```

### Builder Container Pattern

Use a Docker container for reproducible builds:

**Dockerfile**

```dockerfile
FROM ubuntu:24.04

# Install build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    git \
    nodejs \
    npm \
    && rm -rf /var/lib/apt/lists/*

# Add any language-specific toolchains
# RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

WORKDIR /workspace
```

**build-artifacts.sh**

```bash
#!/usr/bin/env bash
set -euo pipefail

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
REPO_ROOT="$(cd "$SCRIPT_DIR/.." && pwd)"
IMAGE_NAME="my-app-builder:local"

echo "[builder] Building builder image..."
docker build -t "$IMAGE_NAME" "$SCRIPT_DIR"

echo "[builder] Running build inside container..."

# Pass VERSION from environment or derive from git
DOCKER_ENV=()
if [ -n "${VERSION:-}" ]; then
    DOCKER_ENV+=(-e VERSION="$VERSION")
fi

docker run --rm \
    "${DOCKER_ENV[@]}" \
    -v "$REPO_ROOT":/workspace \
    -w /workspace \
    "$IMAGE_NAME" \
    bash -lc "./builder/inside-build.sh"

echo "[builder] Build complete"
```

**inside-build.sh**

```bash
#!/usr/bin/env bash
set -euo pipefail

REPO_ROOT="/workspace"
ARTIFACTS_DIR="$REPO_ROOT/artifacts"

# Determine version
if [ -n "${VERSION:-}" ]; then
    :
elif git -C "$REPO_ROOT" rev-parse --is-inside-work-tree >/dev/null 2>&1; then
    VERSION="$(git -C "$REPO_ROOT" rev-parse --short HEAD)"
else
    VERSION="$(date +%Y%m%d%H%M%S)"
fi

echo "[builder] Building version: $VERSION"

RELEASE_DIR="$ARTIFACTS_DIR/build-$VERSION"
rm -rf "$RELEASE_DIR"
mkdir -p "$RELEASE_DIR"/{bin,assets,commands,scripts}

# Build your application
echo "[builder] Compiling binaries..."
# Example: cargo build --release && cp target/release/server "$RELEASE_DIR/bin/"
# Example: npm run build && cp -r dist "$RELEASE_DIR/assets/"

# Copy command scripts
cp "$REPO_ROOT/deployment/commands"/*.sh "$RELEASE_DIR/commands/" 2>/dev/null || true
chmod +x "$RELEASE_DIR/commands"/*.sh 2>/dev/null || true

# Version marker
echo "$VERSION" > "$RELEASE_DIR/VERSION"

# Package tarball
cd "$RELEASE_DIR"
TARBALL="$ARTIFACTS_DIR/my-app-$VERSION.tar.gz"
tar czf "$TARBALL" .

# Also create latest.tar.gz for convenience
cp "$TARBALL" "$ARTIFACTS_DIR/latest.tar.gz"

echo "[builder] Created: $TARBALL"
```

---

## Makefile Targets

A Makefile provides a consistent interface for the release workflow:

```makefile
SHELL := /usr/bin/env bash

# Deployment names (override as needed)
DEV_DEPLOYMENT ?= my-app-dev
PROD_DEPLOYMENT ?= my-app-prod

# Artifact paths
ARTIFACTS_DIR ?= ./artifacts
APP_PATH ?= .

.PHONY: build-release ship-release-dev ship-release-prod ship-release-dev-local ship-release-prod-local

# Build a versioned release tarball
# Usage: make build-release VERSION=v1.1.0
build-release:
	@echo "[release] Building release (VERSION=$(VERSION))..."
	@VERSION=$(VERSION) ./builder/build-artifacts.sh

# Ship release to dev via URL (CI/registry path)
# Usage: make ship-release-dev VERSION=v1.1.0 ARTIFACT_URL=https://...
ship-release-dev:
	@: $${VERSION:?VERSION is required}
	@: $${ARTIFACT_URL:?ARTIFACT_URL is required}
	@echo "[release] Shipping $(VERSION) to dev..."
	@tfgrid-compose select $(DEV_DEPLOYMENT)
	@tfgrid-compose deploy-release --version "$$VERSION" --artifact-url "$$ARTIFACT_URL"

# Ship release to prod via URL
ship-release-prod:
	@: $${VERSION:?VERSION is required}
	@: $${ARTIFACT_URL:?ARTIFACT_URL is required}
	@echo "[release] Shipping $(VERSION) to prod..."
	@tfgrid-compose select $(PROD_DEPLOYMENT)
	@tfgrid-compose deploy-release --version "$$VERSION" --artifact-url "$$ARTIFACT_URL"

# Ship release to dev using local tarball (rsync, no registry)
# Usage: make ship-release-dev-local VERSION=v1.1.0
ship-release-dev-local:
	@: $${VERSION:?VERSION is required}
	@echo "[release] Shipping local $(VERSION) to dev..."
	@TARBALL="$(ARTIFACTS_DIR)/my-app-$$VERSION.tar.gz"; \
	 if [ ! -f "$$TARBALL" ]; then \
	   echo "[release] Tarball not found, building..."; \
	   VERSION=$$VERSION ./builder/build-artifacts.sh; \
	 fi; \
	 tfgrid-compose select $(DEV_DEPLOYMENT); \
	 VM_IP="$$(tfgrid-compose address 2>&1 | grep -oP 'Primary IP.*:\s*\K[\d.]+' | head -1)"; \
	 echo "[release] Uploading to $$VM_IP..."; \
	 ssh root@$$VM_IP "mkdir -p /opt/my-app/uploads"; \
	 rsync -avzP "$$TARBALL" root@$$VM_IP:/opt/my-app/uploads/; \
	 tfgrid-compose deploy-release --version "$$VERSION" \
	   --artifact-path "/opt/my-app/uploads/my-app-$$VERSION.tar.gz"

# Ship release to prod using local tarball
ship-release-prod-local:
	@: $${VERSION:?VERSION is required}
	@echo "[release] Shipping local $(VERSION) to prod..."
	@TARBALL="$(ARTIFACTS_DIR)/my-app-$$VERSION.tar.gz"; \
	 if [ ! -f "$$TARBALL" ]; then \
	   echo "[release] Tarball not found, building..."; \
	   VERSION=$$VERSION ./builder/build-artifacts.sh; \
	 fi; \
	 tfgrid-compose select $(PROD_DEPLOYMENT); \
	 VM_IP="$$(tfgrid-compose address 2>&1 | grep -oP 'Primary IP.*:\s*\K[\d.]+' | head -1)"; \
	 echo "[release] Uploading to $$VM_IP..."; \
	 ssh root@$$VM_IP "mkdir -p /opt/my-app/uploads"; \
	 rsync -avzP "$$TARBALL" root@$$VM_IP:/opt/my-app/uploads/; \
	 tfgrid-compose deploy-release --version "$$VERSION" \
	   --artifact-path "/opt/my-app/uploads/my-app-$$VERSION.tar.gz"
```

### Usage Examples

```bash
# Build a release from current git commit
export VERSION=$(git rev-parse --short HEAD)
make build-release VERSION=$VERSION

# Ship to dev via rsync (no registry needed)
make ship-release-dev-local VERSION=$VERSION

# After testing, ship to prod
make ship-release-prod-local VERSION=$VERSION

# Or ship via URL if using a registry/CDN
make ship-release-prod VERSION=$VERSION \
  ARTIFACT_URL=https://releases.example.com/my-app-$VERSION.tar.gz
```

---

## Deploy-Release Command

The `deploy-release.sh` script runs on the VM to activate a new version.

### tfgrid-compose.yaml Integration

```yaml
commands:
  deploy-release:
    script: /opt/my-app/commands/deploy-release.sh
    description: "Deploy a new release"
    args: "--version <v> (--artifact-url <url> | --artifact-path <path>)"

  list-releases:
    script: /opt/my-app/commands/list-releases.sh
    description: "List all available releases"

  rollback-release:
    script: /opt/my-app/commands/rollback-release.sh
    description: "Roll back to a previous release"
    args: "--version <v>"
```

### deploy-release.sh

```bash
#!/usr/bin/env bash
set -euo pipefail

VERSION=""
ARTIFACT_URL=""
ARTIFACT_PATH=""

usage() {
    echo "Usage: $0 --version <v> (--artifact-url <url> | --artifact-path <path>)" >&2
}

while [ "$#" -gt 0 ]; do
    case "$1" in
        --version) VERSION="${2-}"; shift 2 ;;
        --artifact-url) ARTIFACT_URL="${2-}"; shift 2 ;;
        --artifact-path) ARTIFACT_PATH="${2-}"; shift 2 ;;
        -h|--help) usage; exit 0 ;;
        *) echo "Unknown argument: $1" >&2; usage; exit 1 ;;
    esac
done

if [ -z "$VERSION" ]; then
    echo "ERROR: --version is required" >&2
    usage
    exit 1
fi

if [ -z "$ARTIFACT_URL" ] && [ -z "$ARTIFACT_PATH" ]; then
    echo "ERROR: One of --artifact-url or --artifact-path is required" >&2
    usage
    exit 1
fi

# Configuration
APP_NAME="my-app"
RELEASES_BASE="/opt/$APP_NAME/releases"
TMP_TARBALL="/tmp/$APP_NAME-$VERSION.tar.gz"

echo "[deploy] Deploying $APP_NAME version $VERSION..."

mkdir -p "$RELEASES_BASE"

# Get the tarball
if [ -n "$ARTIFACT_PATH" ]; then
    if [ ! -f "$ARTIFACT_PATH" ]; then
        echo "ERROR: Artifact not found: $ARTIFACT_PATH" >&2
        exit 1
    fi
    cp "$ARTIFACT_PATH" "$TMP_TARBALL"
else
    echo "[deploy] Downloading from $ARTIFACT_URL..."
    if ! curl -fsSL "$ARTIFACT_URL" -o "$TMP_TARBALL"; then
        echo "ERROR: Failed to download artifact" >&2
        exit 1
    fi
fi

# Extract to versioned directory
RELEASE_DIR="$RELEASES_BASE/$VERSION"
rm -rf "$RELEASE_DIR"
mkdir -p "$RELEASE_DIR"

echo "[deploy] Extracting to $RELEASE_DIR..."
if ! tar -xzf "$TMP_TARBALL" -C "$RELEASE_DIR"; then
    echo "ERROR: Failed to extract tarball" >&2
    exit 1
fi

# Update current symlink (atomic operation)
echo "[deploy] Activating version $VERSION..."
ln -sfn "$RELEASE_DIR" "$RELEASES_BASE/current"

# Update commands from release
COMMANDS_SRC="$RELEASE_DIR/commands"
COMMANDS_DST="/opt/$APP_NAME/commands"
if [ -d "$COMMANDS_SRC" ]; then
    mkdir -p "$COMMANDS_DST"
    cp "$COMMANDS_SRC"/*.sh "$COMMANDS_DST"/ 2>/dev/null || true
    chmod +x "$COMMANDS_DST"/*.sh 2>/dev/null || true
fi

# Restart services
echo "[deploy] Restarting services..."
systemctl restart $APP_NAME-server || true
systemctl restart $APP_NAME-worker || true

# Cleanup
rm -f "$TMP_TARBALL"

echo "[deploy] ✅ Deployed $APP_NAME version $VERSION"
```

---

## Rollback Command

The `rollback-release.sh` script switches to a previous version.

### rollback-release.sh

```bash
#!/usr/bin/env bash
set -euo pipefail

VERSION=""

usage() {
    echo "Usage: $0 --version <v>" >&2
}

while [ "$#" -gt 0 ]; do
    case "$1" in
        --version) VERSION="${2-}"; shift 2 ;;
        -h|--help) usage; exit 0 ;;
        *) echo "Unknown argument: $1" >&2; usage; exit 1 ;;
    esac
done

if [ -z "$VERSION" ]; then
    echo "ERROR: --version is required" >&2
    usage
    exit 1
fi

APP_NAME="my-app"
RELEASES_BASE="/opt/$APP_NAME/releases"
RELEASE_DIR="$RELEASES_BASE/$VERSION"

if [ ! -d "$RELEASE_DIR" ]; then
    echo "ERROR: Release not found: $RELEASE_DIR" >&2
    echo "Available releases:"
    ls -1 "$RELEASES_BASE" | grep -v current
    exit 1
fi

echo "[rollback] Rolling back to version $VERSION..."

# Update symlink
ln -sfn "$RELEASE_DIR" "$RELEASES_BASE/current"

# Restart services
echo "[rollback] Restarting services..."
systemctl restart $APP_NAME-server || true
systemctl restart $APP_NAME-worker || true

echo "[rollback] ✅ Rolled back to version $VERSION"
```

---

## List-Releases Command

The `list-releases.sh` script shows available versions.

### list-releases.sh

```bash
#!/usr/bin/env bash
set -euo pipefail

APP_NAME="my-app"
RELEASES_BASE="/opt/$APP_NAME/releases"

if [ ! -d "$RELEASES_BASE" ]; then
    echo "No releases found"
    exit 0
fi

# Get current version
CURRENT=""
if [ -L "$RELEASES_BASE/current" ]; then
    CURRENT=$(basename "$(readlink "$RELEASES_BASE/current")")
fi

echo "Available releases:"
echo ""

for dir in "$RELEASES_BASE"/*/; do
    [ -d "$dir" ] || continue
    VERSION=$(basename "$dir")
    [ "$VERSION" = "current" ] && continue
    
    # Get modification time
    MTIME=$(stat -c %y "$dir" 2>/dev/null | cut -d' ' -f1)
    
    if [ "$VERSION" = "$CURRENT" ]; then
        echo "  * $VERSION  ($MTIME)  ← current"
    else
        echo "    $VERSION  ($MTIME)"
    fi
done

echo ""
echo "Total: $(ls -1d "$RELEASES_BASE"/*/ 2>/dev/null | grep -v current | wc -l) releases"
```

---

## Using Release Commands

After deploying your app with the release workflow configured:

```bash
# Select your deployment
tfgrid-compose select my-app-prod

# List available releases
tfgrid-compose list-releases

# Output:
# Available releases:
#
#     v1.0.0  (2025-12-01)
#     v1.0.1  (2025-12-03)
#   * v1.1.0  (2025-12-06)  ← current
#
# Total: 3 releases

# Roll back to previous version
tfgrid-compose rollback-release --version v1.0.1

# Deploy a new version
tfgrid-compose deploy-release --version v1.2.0 \
  --artifact-url https://releases.example.com/my-app-v1.2.0.tar.gz
```

---

## Best Practices

### 1. Atomic Deployments

The symlink pattern ensures atomic switches:

```bash
# This is atomic - either the old or new version is active, never partial
ln -sfn "$NEW_RELEASE" "$RELEASES_BASE/current"
```

### 2. Keep N Previous Releases

Add cleanup logic to `deploy-release.sh`:

```bash
# Keep only the last 5 releases
KEEP_RELEASES=5
cd "$RELEASES_BASE"
ls -1t | grep -v current | tail -n +$((KEEP_RELEASES + 1)) | while read old; do
    echo "[deploy] Removing old release: $old"
    rm -rf "$old"
done
```

### 3. Health Checks Before Switching

Validate the new release before activating:

```bash
# Extract to temp location first
TEMP_DIR="/tmp/release-test-$VERSION"
tar -xzf "$TMP_TARBALL" -C "$TEMP_DIR"

# Run validation
if ! "$TEMP_DIR/bin/server" --validate-config; then
    echo "ERROR: Config validation failed" >&2
    rm -rf "$TEMP_DIR"
    exit 1
fi

# Move to final location
mv "$TEMP_DIR" "$RELEASE_DIR"
```

### 4. Graceful Service Restarts

Use systemd's reload when possible:

```bash
# Graceful reload if supported
if systemctl reload $APP_NAME-server 2>/dev/null; then
    echo "[deploy] Gracefully reloaded server"
else
    systemctl restart $APP_NAME-server
fi
```

### 5. Logging and Monitoring

Log all release operations:

```bash
LOG_FILE="/opt/$APP_NAME/logs/releases.log"
log() {
    echo "$(date -Iseconds) $*" | tee -a "$LOG_FILE"
}

log "[deploy] Starting deployment of $VERSION"
# ... deployment steps ...
log "[deploy] Completed deployment of $VERSION"
```

---

## Integration with CI/CD

### GitHub Actions Example

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build release
        run: |
          VERSION=${GITHUB_REF#refs/tags/}
          make build-release VERSION=$VERSION
      
      - name: Upload to release storage
        run: |
          VERSION=${GITHUB_REF#refs/tags/}
          aws s3 cp artifacts/my-app-$VERSION.tar.gz \
            s3://my-releases/my-app-$VERSION.tar.gz
      
      - name: Deploy to staging
        run: |
          VERSION=${GITHUB_REF#refs/tags/}
          make ship-release-dev VERSION=$VERSION \
            ARTIFACT_URL=https://my-releases.s3.amazonaws.com/my-app-$VERSION.tar.gz
```

---

## Troubleshooting

### Release Not Found

```bash
$ tfgrid-compose rollback-release --version v1.0.0
ERROR: Release not found: /opt/my-app/releases/v1.0.0

# Check available releases
tfgrid-compose list-releases
```

### Services Won't Start

```bash
# SSH into the VM
tfgrid-compose ssh

# Check service status
systemctl status my-app-server

# Check logs
journalctl -u my-app-server -n 50

# Verify binary exists
ls -la /opt/my-app/releases/current/bin/
```

### Symlink Issues

```bash
# Verify current symlink
ls -la /opt/my-app/releases/current

# Fix if broken
ln -sfn /opt/my-app/releases/v1.0.0 /opt/my-app/releases/current
```

---

## Next Steps

- **[Custom Apps Guide](custom-apps.md)** - Create the app structure for releases
- **[TFGrid Compose Guide](tfgrid-compose.md)** - Complete CLI reference
- **[App Registry Guide](tfgrid-registry.md)** - Publish your app

---

**Production-ready releases!** You now have a complete workflow for versioned deployments with instant rollback capability.
