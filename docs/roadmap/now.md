# Now - Active Development

**🔨 What we're building right now**

---

## CLI Integration

**Status:** 🔨 Building

### Overview

Deploy apps by name directly from the registry. No more copying manifests - just use the app name.

### What You'll Be Able To Do

```bash
# Search the registry
tfgrid-compose search
tfgrid-compose search wordpress
tfgrid-compose search --tag=cms

# Deploy by name
tfgrid-compose up wordpress
tfgrid-compose up nextcloud
tfgrid-compose up ghost

# Manage multiple deployments
tfgrid-compose up wordpress --name=prod
tfgrid-compose up wordpress --name=staging
tfgrid-compose list
tfgrid-compose switch prod
```

### Features Included

**Registry Integration:**

- Search apps from CLI
- Fetch app manifests automatically
- Cache apps locally
- Auto-update when available

**Multi-Deployment Management:**

- Deploy same app multiple times
- Name your deployments
- Switch between contexts
- List all active deployments

**Smart Caching:**

- Apps cached in `~/.config/tfgrid-compose/apps/`
- Offline mode for cached apps
- Version checking
- Update notifications

### Technical Details

**App Resolution:**
```
tfgrid-compose up wordpress
  ↓
1. Check local cache
2. Fetch from registry if needed
3. Validate manifest
4. Deploy with context name
5. Track in state
```

**Context Management:**
```
~/.config/tfgrid-compose/
├── apps/              # Cached apps
│   ├── wordpress/
│   ├── nextcloud/
│   └── ghost/
├── contexts/          # Deployment contexts
│   ├── prod/
│   ├── staging/
│   └── dev/
└── config.yaml        # Global config
```

---

## Try Current Features

While CLI integration is in development, you can use all current features:

```bash
# Install
curl -sSL install.tfgrid.studio | sh

# Deploy from local manifest
tfgrid-compose up ./my-app

# Available patterns
tfgrid-compose up app --pattern=single-vm
tfgrid-compose up app --pattern=gateway
tfgrid-compose up app --pattern=k3s

# Browse registry
open https://registry.tfgrid.studio
```

---

## Progress Updates

Follow development:
- [GitHub Project Board](https://github.com/orgs/tfgrid-studio/projects)
- [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- [Changelog](https://github.com/tfgrid-studio/tfgrid-compose/blob/main/CHANGELOG.md)

---

**Next:** [What's Planned →](next.md)
