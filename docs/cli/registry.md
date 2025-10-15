# Registry Integration

The TFGrid Compose registry allows you to discover and deploy apps by name, without manually cloning repositories.

:::info Version
Registry integration was added in **v0.10.0** (October 15, 2025)
:::

---

## Quick Start

```bash
# Search available apps
tfgrid-compose search

# Deploy an app by name
tfgrid-compose up wordpress

# List deployed apps
tfgrid-compose list

# Switch between apps
tfgrid-compose switch ai-agent
```

---

## Searching Apps

### Browse All Apps

```bash
tfgrid-compose search
```

**Output:**
```
Available apps:

ai-agent             AI-powered deployment assistant
wordpress            WordPress CMS platform
nextcloud            File sharing and collaboration
ghost                Modern blogging platform
discourse            Community discussion forum
...
```

### Search by Name

```bash
tfgrid-compose search wordpress
```

**Output:**
```
wordpress            WordPress CMS platform
```

### Search by Keyword

```bash
tfgrid-compose search blog
```

**Output:**
```
wordpress            WordPress CMS platform
ghost                Modern blogging platform
```

---

## Deploying Apps

### Deploy by Name

Instead of cloning repositories manually, deploy directly by name:

```bash
tfgrid-compose up wordpress
```

**What happens:**
1. ✅ Fetches app info from registry
2. ✅ Downloads app repository to cache
3. ✅ Deploys the app
4. ✅ Sets it as active context

### Deploy from Local Path

You can still deploy from local paths:

```bash
tfgrid-compose up ./my-custom-app
```

**Both methods work identically** - the CLI automatically detects whether you're providing a name or path.

---

## Managing Multiple Apps

### List Deployed Apps

See all your deployed apps:

```bash
tfgrid-compose list
```

**Output:**
```
  * wordpress (active)
    ai-agent
    nextcloud
```

The asterisk (`*`) indicates the currently active app.

### Switch Active App

Change which app commands operate on:

```bash
tfgrid-compose switch ai-agent
```

**After switching:**
- `tfgrid-compose logs` → shows ai-agent logs
- `tfgrid-compose status` → shows ai-agent status
- `tfgrid-compose exec` → runs commands on ai-agent

### Context-Aware Commands

Once you switch, all commands operate on the active app:

```bash
# Switch to WordPress
tfgrid-compose switch wordpress

# These all operate on WordPress now
tfgrid-compose logs
tfgrid-compose status
tfgrid-compose address

# Switch to AI agent
tfgrid-compose switch ai-agent

# Now these operate on ai-agent
tfgrid-compose exec login
tfgrid-compose logs
```

---

## Complete Workflow Example

### Deploy Multiple Apps

```bash
# Search for apps
tfgrid-compose search

# Deploy WordPress blog
tfgrid-compose up wordpress

# Deploy AI agent for development
tfgrid-compose up ai-agent

# Deploy Nextcloud for file storage
tfgrid-compose up nextcloud

# List all deployed apps
tfgrid-compose list
```

**Output:**
```
  * nextcloud (active)
    ai-agent
    wordpress
```

### Work with Different Apps

```bash
# Work with AI agent
tfgrid-compose switch ai-agent
tfgrid-compose exec create my-project
tfgrid-compose exec run my-project
tfgrid-compose logs

# Check WordPress status
tfgrid-compose switch wordpress
tfgrid-compose address
tfgrid-compose logs

# Manage Nextcloud
tfgrid-compose switch nextcloud
tfgrid-compose status
```

---

## App Caching

Apps are automatically cached locally for fast deployment.

### Cache Location

```
~/.config/tfgrid-compose/
├── registry/
│   └── apps.yaml           # Registry cache (1hr TTL)
├── apps/
│   ├── wordpress/          # Cached app repos
│   ├── ai-agent/
│   └── nextcloud/
├── state/
│   ├── wordpress/          # Per-app deployment state
│   ├── ai-agent/
│   └── nextcloud/
└── current-app             # Active app pointer
```

### Cache Behavior

- **Registry**: Refreshed every hour automatically
- **Apps**: Downloaded once, reused for all deployments
- **Updates**: Apps use cached version (pull updates manually if needed)

### Updating Cached Apps

```bash
# Navigate to cached app
cd ~/.config/tfgrid-compose/apps/wordpress

# Pull latest changes
git pull

# Redeploy with updates
tfgrid-compose up wordpress
```

---

## State Management

Each app has isolated state, preventing conflicts.

### Per-App State

```bash
# Deploy two apps
tfgrid-compose up wordpress
tfgrid-compose up nextcloud

# Each has independent state
~/.config/tfgrid-compose/state/
├── wordpress/
│   ├── vm_ip
│   ├── wireguard.conf
│   └── ansible_inventory.yaml
└── nextcloud/
    ├── vm_ip
    ├── wireguard.conf
    └── ansible_inventory.yaml
```

### No Conflicts

You can deploy the same app multiple times by deploying from different paths with different names.

---

## Comparison: Old vs New

### Old Way (Pre-v0.10.0)

```bash
# Manual steps
git clone https://github.com/tfgrid-studio/wordpress-app
cd wordpress-app
tfgrid-compose up .
```

### New Way (v0.10.0+)

```bash
# One command
tfgrid-compose up wordpress
```

---

## Registry Format

The registry is a simple YAML file hosted on GitHub.

### Example Registry Entry

```yaml
wordpress:
  description: WordPress CMS platform
  repo: https://github.com/tfgrid-studio/wordpress-app
  pattern: gateway
  tags:
    - cms
    - blog
    - php

ai-agent:
  description: AI-powered deployment assistant
  repo: https://github.com/tfgrid-studio/tfgrid-ai-agent
  pattern: single-vm
  tags:
    - ai
    - development
    - tools
```

### Registry Location

**Public registry:** https://github.com/tfgrid-studio/registry

---

## Advanced Usage

### Mix Registry and Local Apps

```bash
# Deploy from registry
tfgrid-compose up wordpress

# Deploy from local path
tfgrid-compose up ./my-custom-app

# Both work together
tfgrid-compose list
```

**Output:**
```
  * my-custom-app (active)
    wordpress
```

### Deploy Same App Multiple Times

```bash
# Deploy production WordPress from registry
tfgrid-compose up wordpress

# Deploy staging WordPress from local fork
tfgrid-compose up ./wordpress-staging
```

---

## Troubleshooting

### App Not Found

```bash
tfgrid-compose up myapp
# Error: App 'myapp' not found in registry
```

**Solution:** Search the registry to find the correct name:

```bash
tfgrid-compose search
```

### Cache Issues

If the registry seems outdated, it refreshes automatically after 1 hour. To force refresh:

```bash
# Remove cache
rm -rf ~/.config/tfgrid-compose/registry/

# Next command will fetch fresh registry
tfgrid-compose search
```

### App Already Deployed

```bash
tfgrid-compose up wordpress
# Error: App 'wordpress' is already deployed
```

**Solution:** Destroy first or switch to it:

```bash
# Option 1: Destroy and redeploy
tfgrid-compose down wordpress
tfgrid-compose up wordpress

# Option 2: Switch to it
tfgrid-compose switch wordpress
```

---

## Best Practices

### 1. Search Before Deploying

Always search to find the right app name:

```bash
tfgrid-compose search
```

### 2. List Regularly

Keep track of deployed apps:

```bash
tfgrid-compose list
```

### 3. Switch Explicitly

Always switch before running commands:

```bash
tfgrid-compose switch myapp
tfgrid-compose logs
```

### 4. Clean Up Unused Apps

Destroy apps you're no longer using:

```bash
tfgrid-compose down wordpress
```

---

## Next Steps

- [Deploy Your First App](../getting-started/quickstart.md)
- [CLI Reference](./commands.md)
- [Pattern Guide](../patterns/)
- [Contributing Apps to Registry](../community/contributing.md)

---

## Related

- [Getting Started](../getting-started/)
- [CLI Commands](./commands.md)
- [Deployment Patterns](../patterns/)
