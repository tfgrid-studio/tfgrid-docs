# Creating Custom TFGrid Compose Apps

**Last Updated**: 2025-12-06  
**Status**: Production Ready

This guide explains how to create your own custom TFGrid Compose applications. Custom apps allow you to package and deploy any software stack on ThreeFold Grid with a consistent, repeatable workflow.

---

## When to Create a Custom App

Create a custom app when you need to:

- **Deploy proprietary software** not available in the public registry
- **Bundle multiple services** into a single deployable unit
- **Customize deployment logic** beyond what registry apps offer
- **Implement organization-specific workflows** (CI/CD, release management)
- **Control the full lifecycle** of your application on TFGrid

---

## App Structure

A TFGrid Compose app follows this directory structure:

```
my-custom-app/
├── tfgrid-compose.yaml      # App manifest (required)
├── .env.example             # Configuration template
├── .env                     # Local configuration (gitignored)
├── .gitignore               # Ignore .env, artifacts, etc.
├── README.md                # Documentation
├── deployment/              # Lifecycle hooks
│   ├── setup.sh             # Install dependencies
│   ├── configure.sh         # Configure and start services
│   └── healthcheck.sh       # Verify deployment health
└── artifacts/               # Build outputs (optional)
```

### Minimal Example

The simplest custom app needs only:

```
my-simple-app/
├── tfgrid-compose.yaml
└── deployment/
    ├── setup.sh
    ├── configure.sh
    └── healthcheck.sh
```

---

## The App Manifest

The `tfgrid-compose.yaml` file defines your application's metadata, requirements, and behavior.

### Minimal Manifest

```yaml
name: my-app
version: "1.0.0"
description: My custom application on TFGrid

patterns:
  recommended: single-vm
  supported:
    - single-vm

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

### Complete Manifest Reference

```yaml
name: my-app
version: "1.0.0"
description: A complete custom application example

# Pattern compatibility
patterns:
  recommended: single-vm
  supported:
    - single-vm
    - gateway

# Network configuration
network:
  main: wireguard           # How Ansible connects: public, wireguard, mycelium
  inter_node: wireguard     # For multi-VM patterns
  mode: both                # User access: wireguard-only, mycelium-only, both
  public_ipv4: false        # Request public IPv4 (costs extra)

# Deployment hooks executed on the VM
hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh

# User-configurable variables
variables:
  app_port:
    type: number
    description: "Port for the application server"
    default: 8080
    required: false

  app_domain:
    type: string
    description: "Domain name for the application"
    required: true

# Environment variables for hooks
environment:
  - name: APP_SECRET
    required: true
    description: "Secret key for the application"

# Resource requirements
resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096        # MB
    recommended: 8192
  disk:
    min: 50          # GB
    recommended: 100
  ports:
    - 22:ssh
    - 80:http
    - 443:https
    - 8080:app

# Custom commands (see "Custom Commands" section)
commands:
  restart:
    script: /opt/my-app/commands/restart.sh
    description: "Restart all application services"

  logs:
    script: /opt/my-app/commands/logs.sh
    description: "Show application logs"
    args: "[--follow] [--lines N]"

# Logging configuration
logging:
  method: systemd
  location: /var/log/my-app/

# Status check
status:
  method: script
  endpoint: deployment/healthcheck.sh

# Documentation
documentation:
  readme: README.md

# Metadata for discovery
tags:
  - custom
  - example

metadata:
  category: development
  maturity: stable
  support_level: community
```

---

## Deployment Hooks

Hooks are shell scripts executed on the VM during deployment. They run in order: `setup` → `configure` → `healthcheck`.

### setup.sh

Installs system dependencies and prepares the environment. Runs once during initial deployment.

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "[my-app] Installing dependencies..."

# Update package lists
apt-get update -qq

# Install required packages
apt-get install -y -qq \
  nginx \
  postgresql \
  redis-server \
  curl \
  jq

# Create application directories
mkdir -p /opt/my-app/{bin,config,data,logs}
mkdir -p /var/www/my-app

# Create application user
useradd --system --home /opt/my-app --shell /bin/false myapp || true

echo "[my-app] Setup complete"
```

### configure.sh

Configures services and starts the application. May run multiple times (initial deploy + updates).

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "[my-app] Configuring application..."

# Load environment variables
APP_ROOT="/tmp/app-source"
if [ -f "$APP_ROOT/.env" ]; then
  set -a
  source "$APP_ROOT/.env"
  set +a
fi

# Configure nginx
cat > /etc/nginx/sites-available/my-app <<EOF
server {
    listen 80;
    server_name ${APP_DOMAIN:-localhost};
    
    location / {
        proxy_pass http://127.0.0.1:${APP_PORT:-8080};
        proxy_set_header Host \$host;
        proxy_set_header X-Real-IP \$remote_addr;
    }
}
EOF

ln -sf /etc/nginx/sites-available/my-app /etc/nginx/sites-enabled/
rm -f /etc/nginx/sites-enabled/default

# Restart services
systemctl restart nginx
systemctl enable nginx

echo "[my-app] Configuration complete"
```

### healthcheck.sh

Verifies the deployment is working correctly. Should exit 0 on success, non-zero on failure.

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "[my-app] Running health checks..."

ERRORS=0

# Check nginx is running
if ! systemctl is-active --quiet nginx; then
  echo "❌ nginx is not running"
  ERRORS=$((ERRORS + 1))
else
  echo "✅ nginx is running"
fi

# Check application responds
if ! curl -sf http://localhost:${APP_PORT:-8080}/health > /dev/null 2>&1; then
  echo "❌ Application health endpoint not responding"
  ERRORS=$((ERRORS + 1))
else
  echo "✅ Application is healthy"
fi

# Check disk space
DISK_USAGE=$(df / | tail -1 | awk '{print $5}' | tr -d '%')
if [ "$DISK_USAGE" -gt 90 ]; then
  echo "⚠️  Disk usage is ${DISK_USAGE}%"
else
  echo "✅ Disk usage is ${DISK_USAGE}%"
fi

if [ "$ERRORS" -gt 0 ]; then
  echo "❌ Health check failed with $ERRORS errors"
  exit 1
fi

echo "✅ All health checks passed"
exit 0
```

---

## Custom Commands

Custom commands extend `tfgrid-compose` with app-specific operations. Define them in the manifest and implement as scripts on the VM.

### Defining Commands

In `tfgrid-compose.yaml`:

```yaml
commands:
  restart:
    script: /opt/my-app/commands/restart.sh
    description: "Restart all application services"

  logs:
    script: /opt/my-app/commands/logs.sh
    description: "Show application logs"
    args: "[--follow] [--lines N]"

  status:
    script: /opt/my-app/commands/status.sh
    description: "Show detailed application status"

  backup:
    script: /opt/my-app/commands/backup.sh
    description: "Create a backup of application data"
    args: "--output <path>"
```

### Implementing Commands

Create the commands directory in your `deployment/setup.sh`:

```bash
mkdir -p /opt/my-app/commands
```

Copy command scripts during deployment or include them in your artifacts.

**Example: /opt/my-app/commands/restart.sh**

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "[my-app] Restarting services..."

systemctl restart my-app-backend
systemctl restart my-app-worker
systemctl restart nginx

echo "[my-app] All services restarted"
```

### Using Commands

After deployment, use commands via tfgrid-compose:

```bash
# Select your app
tfgrid-compose select my-app

# Run custom commands
tfgrid-compose restart
tfgrid-compose logs --follow
tfgrid-compose status
tfgrid-compose backup --output /tmp/backup.tar.gz
```

---

## Environment Configuration

### The .env Pattern

Use `.env` files for configuration that varies between environments.

**`.env.example`** (committed to git):

```bash
# Application Configuration
APP_ENV=development
APP_PORT=8080
APP_DOMAIN=myapp.example.com

# Database
DATABASE_URL=postgres://user:pass@localhost:5432/myapp

# Secrets (replace with real values)
APP_SECRET=change-me-in-production
API_KEY=your-api-key-here

# Optional: External services
# SMTP_HOST=smtp.example.com
# SMTP_USER=
# SMTP_PASS=
```

**`.env`** (gitignored, local only):

```bash
APP_ENV=production
APP_PORT=8080
APP_DOMAIN=myapp.acme-corp.com
DATABASE_URL=postgres://prod:secret@db.internal:5432/myapp
APP_SECRET=super-secret-production-key
API_KEY=real-api-key
```

### Loading Environment in Hooks

```bash
#!/usr/bin/env bash
set -euo pipefail

# Standard location where tfgrid-compose copies app source
APP_ROOT="/tmp/app-source"

# Load .env if present
if [ -f "$APP_ROOT/.env" ]; then
  echo "[my-app] Loading environment from $APP_ROOT/.env"
  set -a
  source "$APP_ROOT/.env"
  set +a
fi

# Use variables with defaults
echo "Running in ${APP_ENV:-development} mode on port ${APP_PORT:-8080}"
```

### Secrets Handling

**Best practices:**

1. **Never commit secrets** - Keep `.env` in `.gitignore`
2. **Use environment variables** - Pass secrets via tfgrid-compose `--var` or environment
3. **Validate required secrets** - Check in setup.sh before proceeding

```bash
# Validate required secrets
for VAR in APP_SECRET DATABASE_URL; do
  if [ -z "${!VAR:-}" ]; then
    echo "[my-app] ERROR: Required variable $VAR is not set" >&2
    exit 1
  fi
done
```

---

## Testing Your App

### Local Validation

Before deploying, validate your app structure:

```bash
# Check manifest syntax
cat tfgrid-compose.yaml | python3 -c "import yaml, sys; yaml.safe_load(sys.stdin)"

# Test hooks are executable
chmod +x deployment/*.sh
bash -n deployment/setup.sh
bash -n deployment/configure.sh
bash -n deployment/healthcheck.sh
```

### Deploy to Test Node

```bash
# Deploy your custom app
tfgrid-compose up ./my-custom-app

# Check status
tfgrid-compose status

# SSH in to debug
tfgrid-compose ssh

# View logs
tfgrid-compose logs

# Tear down when done
tfgrid-compose down
```

### Iterating on Deployment

During development, you can update hooks and redeploy:

```bash
# Make changes to deployment scripts
vim deployment/configure.sh

# Redeploy (destroys and recreates VM)
tfgrid-compose down
tfgrid-compose up ./my-custom-app
```

For faster iteration on long-lived VMs, see the [Tarball Releases Guide](tarball-releases.md).

---

## Publishing to App Registry

Once your app is stable, you can publish it to the TFGrid App Registry for others to use.

### Requirements

1. **Public Git repository** (GitHub, GitLab, etc.)
2. **Complete documentation** (README.md)
3. **Working deployment** tested on TFGrid
4. **Semantic versioning** (v1.0.0, v1.1.0, etc.)

### Registry Entry

Submit a PR to [tfgrid-studio/tfgrid-registry](https://github.com/tfgrid-studio/tfgrid-registry) with:

```yaml
# apps/my-app.yaml
name: my-app
description: Short description of your app
repository: https://github.com/your-org/my-app
version: v1.0.0
tags:
  - category
  - keywords
maintainer: your-email@example.com
```

See the [App Registry Guide](tfgrid-registry.md) for detailed publishing instructions.

---

## Example: Complete Custom App

Here's a complete example of a custom app that deploys a Node.js API with PostgreSQL:

### Directory Structure

```
node-api-app/
├── tfgrid-compose.yaml
├── .env.example
├── .gitignore
├── README.md
├── deployment/
│   ├── setup.sh
│   ├── configure.sh
│   └── healthcheck.sh
└── src/
    ├── package.json
    └── server.js
```

### tfgrid-compose.yaml

```yaml
name: node-api-app
version: "1.0.0"
description: Node.js API with PostgreSQL backend

patterns:
  recommended: single-vm
  supported:
    - single-vm

network:
  main: wireguard
  mode: both
  public_ipv4: true

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh

variables:
  api_port:
    type: number
    default: 3000

resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096
    recommended: 8192
  disk:
    min: 50
    recommended: 100
  ports:
    - 22:ssh
    - 80:http
    - 443:https
    - 3000:api

commands:
  restart:
    script: /opt/node-api/commands/restart.sh
    description: "Restart the API server"

  logs:
    script: /opt/node-api/commands/logs.sh
    description: "Show API logs"

tags:
  - nodejs
  - api
  - postgresql
```

---

## Next Steps

- **[Tarball Releases Guide](tarball-releases.md)** - Implement versioned releases with rollback for long-lived VMs
- **[App Registry Guide](tfgrid-registry.md)** - Publish your app for others to use
- **[TFGrid Compose Guide](tfgrid-compose.md)** - Complete CLI reference
- **[Examples](https://github.com/tfgrid-studio/tfgrid-compose/tree/main/examples)** - More example apps

---

**Ready to build!** You now have everything needed to create custom TFGrid Compose applications.
