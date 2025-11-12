# Enhanced TFGrid Compose Workflow

## 🎯 Overview

The Enhanced TFGrid Compose workflow provides a streamlined development experience with Git-based cache management and intelligent app orchestration. This guide covers the complete workflow from setup to deployment.

### Key Benefits

- **Git-based cache management** - Automatic cache invalidation when source code changes
- **Smart caching** - Intelligent cache refresh without manual intervention  
- **Shortened commands** - Use 't' instead of 'tfgrid-compose'
- **Enhanced error handling** - Clear cache health monitoring and validation
- **Seamless updates** - Automatic cache refresh during deployments

## 🚀 Quick Start

### 1. Install and Setup

```bash
# Install tfgrid-compose (if not already installed)
make install

# Create the 't' shortcut
t shortcut t

# Verify setup
t version
```

### 2. Complete Workflow Example

```bash
# Step 1: Check available apps
t search

# Step 2: Deploy tfgrid-ai-stack  
t up tfgrid-ai-stack

# Step 3: Use app commands directly
t create          # Create new AI coding project
t run            # Start agent loop for project
t publish        # Publish project for web hosting

# Step 4: Access your projects
# Git repositories: http://DEPLOYMENT_IP/git/username/reponame
# Published sites: http://DEPLOYMENT_IP/web/username/reponame
```

## 📋 Detailed Workflow

### Phase 1: Initial Setup

#### 1.1 Create Command Shortcut

```bash
# Create 't' shortcut for tfgrid-compose
t shortcut t

# List existing shortcuts
t shortcut --list

# Remove shortcuts if needed
t shortcut --remove t
```

#### 1.2 Verify Cache System

```bash
# Check cache status
t cache status

# List all cached apps with health
t cache list

# Show cache information
t cache info
```

### Phase 2: App Deployment

#### 2.1 Discover Available Apps

```bash
# Search all available apps
t search

# Search with query
t search tfgrid

# Search by tag
t search --tag ai
```

#### 2.2 Deploy Applications

```bash
# Basic deployment
t up tfgrid-ai-stack

# Deploy with custom naming
t up tfgrid-ai-stack --name=my-ai-stack

# Force deployment (replaces existing)
t up tfgrid-ai-stack --force

# Deploy with auto-cache refresh
t up tfgrid-ai-stack --refresh
```

#### 2.3 Monitor Deployment

```bash
# List all deployments
t ps

# Select active deployment
t select

# Check deployment status
t status

# View deployment logs
t logs
```

### Phase 3: App Command Execution

#### 3.1 App-Specific Commands

Once an app is deployed, you can run its specific commands:

```bash
# For tfgrid-ai-stack:
t create           # Interactive AI project creation
t run             # Start AI agent loop
t publish         # Publish project to web
t monitor         # Monitor project progress
t projects        # Show all projects status
t login           # Authenticate with AI service
t status          # Show system status
t backup          # Trigger manual backup
```

#### 3.2 Command Execution Flow

```bash
# If multiple deployments exist, specify which to use:
t select tfgrid-ai-stack-abc123
t create

# Or run commands directly with deployment context:
t create --deployment=tfgrid-ai-stack-abc123
```

### Phase 4: Advanced Cache Management

#### 4.1 Cache Health Monitoring

```bash
# Check overall cache health
t cache status

# List all apps with health indicators:
# ✅ = Healthy cache
# 🔄 = Stale cache (needs update)
# ⚠️ = Invalid cache (has issues)
# ❌ = Not cached
t cache list

# Validate specific app cache
t cache validate tfgrid-ai-stack

# Validate all cached apps
t cache validate
```

#### 4.2 Cache Updates

```bash
# Auto-refresh all outdated apps
t cache refresh

# List apps that need updates
t cache outdated

# Update specific app
t update tfgrid-ai-stack

# Update all apps
t update --all-apps
```

#### 4.3 Manual Cache Management

```bash
# Clear specific app cache
t cache clear tfgrid-ai-stack

# Clear all cache (with confirmation)
t cache clear

# Force clear all cache
t cache clear --all

# Clean cache during deployment
t up tfgrid-ai-stack --refresh
```

## 🛠️ Cache Management System

### Git-Based Cache Invalidation

The enhanced cache system uses Git commit hashes for precise version tracking:

- **Automatic invalidation** - Cache refreshes when source commits change
- **Migration support** - Upgrades from old cache_version format
- **Health validation** - Checks for corrupted or incomplete caches
- **Background refresh** - Silent updates during normal operations

### Cache Architecture

```
~/.config/tfgrid-compose/
├── apps/                    # Cached app repositories
│   ├── tfgrid-ai-stack/     # App source code
│   └── tfgrid-gitea/
└── cache-metadata/          # Cache version tracking
    ├── tfgrid-ai-stack.json # Git commit hashes, versions
    └── tfgrid-gitea.json
```

### Cache Metadata Format

```json
{
  "app_name": "tfgrid-ai-stack",
  "commit_hash": "abc123def456789",
  "manifest_version": "1.2.3",
  "cached_at": 1640995200,
  "last_updated": 1640995200
}
```

## 🔄 Workflow Integration

### Automatic Cache Refresh

The cache system automatically handles updates during normal operations:

1. **During deployment** - Apps are checked for updates before use
2. **Cache validation** - Failed deployments trigger cache refresh
3. **Manual commands** - Cache commands work regardless of deployment state
4. **Background updates** - Stale caches are updated transparently

### Smart Context Detection

The system automatically detects the correct context:

- **Single deployment** - Commands work without specifying app
- **Multiple deployments** - Shows selection menu or requires specification
- **App-specific commands** - Takes precedence over built-in commands
- **Context persistence** - Remembers selections between commands

## 🎯 Real-World Example: AI Development Workflow

### Complete AI Development Session

```bash
# 1. Setup (one-time)
t shortcut t

# 2. Deploy AI stack
t up tfgrid-ai-stack --name=my-ai-env

# 3. Create new project
t create
# [Interactive: Select project type, naming, etc.]

# 4. Start development loop
t run
# [AI agent runs, creates code, commits to git]

# 5. Publish when ready
t publish
# [Website deployed to http://DEPLOYMENT_IP/web/username/projectname]

# 6. Access your work
# Git repo: http://DEPLOYMENT_IP/git/username/projectname
# Live site: http://DEPLOYMENT_IP/web/username/projectname

# 7. Continue development
t create        # Create another project
t run          # Work on existing project
t projects     # Check all projects status
t backup       # Backup everything
```

### Development Workflow Benefits

- **Seamless Git integration** - All work automatically versioned
- **One-command deployments** - No complex deployment processes
- **Web hosting included** - Instant public access to projects  
- **AI-powered development** - Intelligent code generation and management
- **Cache efficiency** - Always uses latest code without manual refreshes

## 🔧 Configuration and Customization

### App Manifest Customization

Apps can be customized by modifying their manifests:

```yaml
# tfgrid-compose.yaml
name: my-custom-ai-stack
version: 1.0.0
commands:
  create:
    description: "Create new AI project"
    script: "src/scripts/create-project.sh"
    allocate_tty: true
  deploy:
    description: "Deploy to production"
    script: "src/scripts/deploy-prod.sh"
    allocate_tty: false
```

### Cache Configuration

Cache behavior can be customized:

```bash
# Set custom cache directories
export TFGRID_CACHE_DIR="/custom/cache/path"
export TFGRID_METADATA_DIR="/custom/metadata/path"

# Disable automatic cache updates (not recommended)
export TFGRID_CACHE_AUTO_REFRESH=false
```

## 📊 Monitoring and Debugging

### Cache Health Diagnostics

```bash
# Detailed cache health check
t cache list

# Validate cache integrity
t cache validate

# Show cache statistics
t cache info

# Check for outdated apps
t cache outdated
```

### Deployment Monitoring

```bash
# Deployment status overview
t ps

# Detailed deployment inspection
t inspect DEPLOYMENT_ID

# Health check
t status health DEPLOYMENT_ID

# View logs
t logs DEPLOYMENT_ID

# Retry failed deployment
t status retry DEPLOYMENT_ID
```

### Troubleshooting Commands

```bash
# Check tfgrid-compose version
t version

# Show current configuration
t config show

# View available patterns
t patterns

# Get help for specific command
t help cache
t help up
t help shortcut
```

## 🚨 Troubleshooting

### Common Issues

#### 1. Cache Stale Issues

**Problem**: Using old version of app despite recent updates

**Solution**: 
```bash
# Auto-refresh outdated apps
t cache refresh

# Or force refresh specific app
t update tfgrid-ai-stack
```

#### 2. Deployment Failures

**Problem**: Deployment fails with cache-related errors

**Solution**:
```bash
# Clear cache and redeploy
t up tfgrid-ai-stack --refresh --force
```

#### 3. Command Not Found

**Problem**: App-specific commands not working

**Solution**:
```bash
# Check if app is selected
t ps

# Select correct deployment
t select DEPLOYMENT_ID

# Verify app commands
t commands
```

#### 4. Multiple Deployments

**Problem**: Ambiguous deployment selection

**Solution**:
```bash
# List all deployments with details
t ps

# Select specific deployment
t select DEPLOYMENT_ID

# Or specify deployment in command
t create --deployment=SPECIFIC_DEPLOYMENT_ID
```

### Cache Recovery

If cache becomes corrupted:

```bash
# 1. Clear all cache
t cache clear --all

# 2. Update registry
t update registry

# 3. Redeploy apps
t up tfgrid-ai-stack
```

### Performance Optimization

```bash
# Check cache health regularly
t cache status

# Clear unused cache
t cache clear

# Monitor cache size
t cache info

# Validate cache periodically
t cache validate
```

## 🎯 Best Practices

### Development Workflow

1. **Use meaningful deployment names**: `t up tfgrid-ai-stack --name=production`
2. **Regular cache maintenance**: `t cache refresh` weekly
3. **Monitor deployment health**: `t status health` after deployments
4. **Backup before major changes**: `t backup` before updates

### Cache Management

1. **Let auto-refresh work**: Don't manually clear cache unless necessary
2. **Monitor cache health**: Use `t cache list` to check status
3. **Validate after updates**: `t cache validate` after system updates
4. **Clean unused apps**: Remove cache for apps you no longer use

### Security Considerations

1. **Secure deployments**: Use proper network configurations
2. **Regular backups**: Use app backup commands before changes
3. **Monitor access**: Check git and web access logs
4. **Update regularly**: Keep tfgrid-compose and apps updated

## 🔗 Related Documentation

- [Cache Management Guide](./cache-management.md)
- [Registry Integration](./registry-integration.md)
- [Deployment Patterns](../patterns/)
- [CLI Reference](../cli/)
- [Architecture Overview](../architecture/)

---

**Next Steps**: 
1. Follow the [Quick Start](#-quick-start) to get running
2. Explore [Advanced Cache Management](#-cache-management-system) for optimization  
3. Try the [Real-World Example](#-real-world-example-ai-development-workflow) for practical experience