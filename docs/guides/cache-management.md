# Cache Management Guide

The enhanced cache management system in TFGrid Compose v0.13.4+ provides intelligent caching with version-based invalidation, smart health checks, and automatic refresh capabilities.

## Overview

The cache system speeds up deployment by locally storing app repositories, but the enhanced system prevents the common issue of stale cached apps causing deployment failures.

### Key Features

- **Git-Based Version Tracking**: Uses Git commit hashes for precise version detection
- **Smart Health Checks**: Validates cached apps for syntax errors and missing files
- **Automatic Refresh**: Detects and updates stale caches automatically
- **Commit Hash Comparison**: Compares cached vs current Git commit to detect changes
- **Clear Status Indicators**: Visual feedback on cache health

## Cache Commands

### Check Cache Status

View the overall health of your cached apps:

```bash
# Show cache overview
tfgrid-compose cache status

# Example output:
# ✅ tfgrid-ai-stack
# ✅ tfgrid-gitea
# 🔄 tfgrid-ai-agent [needs update]
```

### List All Cached Apps

Get detailed information about all cached applications:

```bash
# List all cached apps with status
tfgrid-compose cache list

# Example output:
# ✅ tfgrid-ai-stack
# ✅ tfgrid-gitea
# 🔄 tfgrid-ai-agent [needs update]
# ⚠️ invalid-app [has issues]
```

### Check for Outdated Apps

See which apps need updating:

```bash
# Show apps needing updates
tfgrid-compose cache outdated

# Example output:
# 📦 tfgrid-ai-agent: cache=1.0.0, latest=1.0.1
```

### Auto-Refresh Outdated Apps

Automatically update all stale caches:

```bash
# Refresh all outdated apps
tfgrid-compose cache refresh

# Example output:
# 🔄 Checking for outdated apps...
# 📦 Updating tfgrid-ai-agent...
# ✅ Updated tfgrid-ai-agent
# ✅ Updated 1 apps, skipped 0
```

### Validate Cache Integrity

Validate cached apps for syntax and structure:

```bash
# Validate all cached apps
tfgrid-compose cache validate

# Validate specific app
tfgrid-compose cache validate tfgrid-ai-stack
```

### Clear Cache

Remove cached apps when needed:

```bash
# Clear specific app cache
tfgrid-compose cache clear tfgrid-ai-stack

# Clear all cache (requires confirmation)
tfgrid-compose cache clear --all
```

### View Cache Information

Get statistics and information about your cache:

```bash
# Show cache statistics
tfgrid-compose cache info

# Example output:
# Cache Directory: /home/user/.config/tfgrid-compose/apps
# Cache Statistics:
#   Total apps: 3
#   Healthy: 2
#   Stale: 1
#   Invalid: 0
```

## Update Commands

### Enhanced Update System

The `update` command now supports multiple update strategies:

```bash
# Update tfgrid-compose binary only (fast)
tfgrid-compose update

# Update specific app
tfgrid-compose update tfgrid-ai-stack

# Update registry data only
tfgrid-compose update registry

# Update all cached apps
tfgrid-compose update --all-apps
```

## How It Works

### Git-Based Version Tracking

Since all apps are in Git repositories, the cache system uses Git's native version control:

- **Commit Hash Comparison**: Compares cached commit hash vs current Git commit
- **No Manual Versioning**: Automatically detects when an app has been updated in Git
- **Precise Detection**: Any commit change triggers cache refresh
- **Branch Aware**: Supports both main and master branches

```bash
# Example: Cache knows when tfgrid-ai-stack is updated
# Cached: abc123def456 (from last week)
# Current: 789ghi012jkl (new commit today)
# → Cache refresh triggered automatically
```

### Cache Health States

- **✅ Healthy**: Up-to-date and validated
- **🔄 Stale**: Needs refresh (version mismatch or age)
- **⚠️ Invalid**: Cache has issues (missing files, syntax errors)
- **❌ Not Cached**: App not in cache

### Automatic Refresh Triggers

Caches are considered stale when:
- Git commit hash differs from cached commit hash
- Cache is older than 24 hours (fallback mechanism)
- Validation fails (missing files, syntax errors)
- Repository has new commits since last cache

## Best Practices

### Regular Maintenance

```bash
# Check cache health regularly
tfgrid-compose cache status

# Refresh outdated apps
tfgrid-compose cache refresh
```

### Before Important Deployments

```bash
# Ensure cache is fresh before critical deployments
tfgrid-compose cache validate
tfgrid-compose cache refresh
```

### Development Workflow

```bash
# Development with fresh cache
tfgrid-compose cache clear --all
tfgrid-compose up my-app --refresh

# Or check cache first
tfgrid-compose cache status
```

## Troubleshooting

### Cache Validation Fails

If `cache validate` shows issues:

```bash
# Clear problematic cache
tfgrid-compose cache clear <app-name>

# Redeploy to get fresh cache
tfgrid-compose up <app-name>
```

### Stale Cache Issues

If you suspect stale cache:

```bash
# Check what's outdated
tfgrid-compose cache outdated

# Force refresh
tfgrid-compose cache refresh

# Or clear all and start fresh
tfgrid-compose cache clear --all
tfgrid-compose up <app-name>
```

### Performance Issues

If cache operations are slow:

```bash
# Check cache size
du -sh ~/.config/tfgrid-compose/apps

# Clear old/unused apps
tfgrid-compose cache clear <unused-app>
```

## Migration from Old System

The enhanced cache system is backward compatible. Existing cached apps will be automatically migrated and validated on first use.

### Manual Migration

If you want to ensure a clean migration:

```bash
# Clear all old cache
tfgrid-compose cache clear --all

# Redeploy apps with new cache system
tfgrid-compose up tfgrid-ai-stack
```

## Integration with Deployment Commands

### Auto-Refresh on Deploy

```bash
# Deploy automatically checks and refreshes cache if needed
tfgrid-compose up tfgrid-ai-stack

# Force fresh cache
tfgrid-compose up tfgrid-ai-stack --refresh

# Skip cache refresh (for testing)
tfgrid-compose up tfgrid-ai-stack --no-refresh
```

### Cache-Aware Shortcuts

When using shortcuts like `t` instead of `tfgrid-compose`:

```bash
# Check cache with shortcut
t cache status

# Update app with shortcut
t update tfgrid-ai-stack

# Deploy with cache auto-refresh
t up tfgrid-ai-stack
```

## Advanced Usage

### Custom Cache Configuration

Cache locations can be customized:

```bash
# View current cache location
tfgrid-compose cache info

# Default locations:
# Apps: ~/.config/tfgrid-compose/apps
# Metadata: ~/.config/tfgrid-compose/cache-metadata
```

### Batch Operations

Update multiple apps efficiently:

```bash
# Update all apps at once
t update --all-apps

# Or refresh only stale apps
t cache refresh
```

### Health Monitoring

For monitoring scripts or CI/CD:

```bash
# Check cache health (exit code 0 = healthy)
t cache validate

# List apps needing attention
t cache outdated
```

## Examples

### Complete Cache Management Workflow

```bash
# 1. Check current status
$ t cache status
✅ tfgrid-ai-stack
✅ tfgrid-gitea
🔄 tfgrid-ai-agent [needs update]

# 2. See what's outdated (shows commit hashes)
$ t cache outdated
📦 tfgrid-ai-agent: cache=abc123def456, latest=789ghi012jkl

# 3. Update the outdated app
$ t update tfgrid-ai-agent
📦 Updating tfgrid-ai-agent...
✅ Updated tfgrid-ai-agent to commit 789ghi012jkl

# 4. Verify the fix
$ t cache status
✅ tfgrid-ai-stack
✅ tfgrid-gitea
✅ tfgrid-ai-agent

# 5. Deploy with confidence
$ t up tfgrid-ai-stack
[Deploys using fresh, validated cache]
```

### Development Workflow

```bash
# Start development session
$ t cache status
✅ All apps are healthy

# Make changes to app source
# (modify tfgrid-ai-stack source code)

# Test deployment with fresh cache
$ t cache clear tfgrid-ai-stack && t up tfgrid-ai-stack
[Downloads latest version and deploys]

# Or manually update cache
$ t update tfgrid-ai-stack && t up tfgrid-ai-stack
[Updates cache and deploys]
```

## Command Reference

| Command | Description | Example |
|---------|-------------|---------|
| `cache status` | Show cache health overview | `t cache status` |
| `cache list` | List all cached apps with status | `t cache list` |
| `cache outdated` | Show apps needing updates | `t cache outdated` |
| `cache refresh` | Auto-refresh outdated apps | `t cache refresh` |
| `cache validate` | Validate cached apps integrity | `t cache validate` |
| `cache clear [app]` | Clear cache (specific or all) | `t cache clear tfgrid-ai-stack` |
| `cache info` | Show cache statistics | `t cache info` |
| `update [app]` | Update specific app | `t update tfgrid-ai-stack` |
| `update registry` | Update registry data | `t update registry` |
| `update --all-apps` | Update all cached apps | `t update --all-apps` |

## Version History

- **v0.13.4+**: Enhanced cache management with version-based invalidation
- **v0.13.3 and earlier**: Basic caching without version tracking

The enhanced system eliminates the need for manual cache clearing and provides reliable, automatic cache management.