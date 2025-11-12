# Registry Integration Guide

## 🎯 Overview

The TFGrid App Registry serves as the central catalog for all available applications, providing metadata, versioning, and Git-based cache integration. This guide covers how the registry works with the enhanced cache management system.

## 📋 Registry Structure

### Core Components

```
app-registry/
├── registry/
│   ├── apps.yaml              # Main registry file
│   └── metadata.yaml          # Registry metadata
└── docs/
    ├── integration-guide.md   # This file
    └── app-guidelines.md      # App submission guidelines
```

### Schema Evolution

- **v0.1.0**: Basic app listing with cache_version
- **v0.2.0**: Enhanced with Git-based cache metadata
- **v1.0.0**: (Planned) Full API integration and community features

## 🏗️ App Registry Schema v0.2.0

### App Entry Structure

```yaml
- name: app-name
  repo: github.com/organization/app-repo
  description: "Human-readable description"
  pattern: single-vm|gateway|k3s
  status: production|beta|experimental
  version: v1.0.0
  cache_version: "2.0.0"  # Git-based cache version
  maintainer: organization
  commands:                # Available app commands
    create: "Description of create command"
    run: "Description of run command"
    publish: "Description of publish command"
  cache_metadata:          # Enhanced cache integration
    git_tracking: true     # Uses Git commit tracking
    auto_refresh: true     # Auto-refresh on updates
    health_check: true     # Cache health validation
    migration_ready: true  # Supports cache migration
  tags:
    - ai
    - development
    - enhanced-cache
  requirements:
    cpu: 4
    memory: 8GB
    disk: 100GB
  links:
    docs: https://docs.example.com/app
    repo: https://github.com/org/app
    workflow: https://docs.example.com/workflow
```

### Metadata Categories

#### 1. **Official Apps** (`apps.official`)
- Maintained by tfgrid-studio team
- Production quality and full support
- Includes enhanced cache metadata
- Git-based cache tracking enabled
- Migration support for cache updates

#### 2. **Community Apps** (`apps.community`)
- Submitted and maintained by community
- Reviewed and approved by tfgrid-studio team
- Quality checked but supported by original author
- May include basic cache metadata

## 🔄 Cache Integration

### Git-Based Tracking

The enhanced cache system uses registry metadata to enable:

1. **Commit Hash Tracking**: Each app tracks the Git commit hash
2. **Auto-Invalidation**: Cache refreshes when source commits change
3. **Health Validation**: Registry provides health check specifications
4. **Migration Support**: Registry tracks migration capabilities

### Cache Metadata Support

```yaml
cache_metadata:
  git_tracking: true        # Enable Git commit tracking
  auto_refresh: true        # Auto-refresh stale cache
  health_check: true        # Validate cache integrity
  last_commit_check: "2025-11-12"  # Last metadata sync
  migration_ready: true     # Support cache version migration
  backup_cache: false       # Keep backup before migration
  validation_rules:         # Cache validation rules
    - "yaml_syntax"
    - "deployment_hooks"
    - "command_scripts"
```

### Enhanced Commands Integration

Registry now tracks available commands:

```yaml
commands:
  create:    # t create command
    description: "Create new AI coding project (interactive)"
    tty_required: true      # Needs interactive terminal
    cache_aware: true       # Works with enhanced cache
  run:       # t run command  
    description: "Start agent loop for project"
    tty_required: true
    background: false       # Foreground process
  publish:   # t publish command
    description: "Publish project for web hosting"
    tty_required: false     # Can run non-interactively
    background: true        # Background operation
```

## 📡 Registry Integration Process

### 1. App Discovery

```bash
# Discover available apps
t search

# Search with query
t search ai

# Search by tag
t search --tag enhanced-cache

# Get detailed app info
t inspect tfgrid-ai-stack
```

### 2. Cache Integration

```bash
# Cache app with Git tracking
t up tfgrid-ai-stack

# Check cache health
t cache list

# Validate cache integrity
t cache validate tfgrid-ai-stack

# View cache metadata
t cache info tfgrid-ai-stack
```

### 3. Auto-Refresh Workflow

```
Registry Lookup → Git Commit Check → Cache Validation → Auto-Refresh
       ↓              ↓              ↓               ↓
   App Info      Compare Hashes   Validate Files   Update Cache
       ↓              ↓              ↓               ↓
  App Commands   Update Needed?   Health Status    Fresh Cache
```

## 🔧 Development Integration

### For App Maintainers

#### 1. Adding Cache Metadata

```yaml
cache_metadata:
  git_tracking: true
  auto_refresh: true
  health_check: true
  last_commit_check: "2025-11-12"
  migration_ready: true
  validation_rules:
    - "yaml_syntax"
    - "deployment_hooks"
    - "command_scripts"
    - "template_files"
```

#### 2. Command Documentation

```yaml
commands:
  create:
    description: "Interactive project creation with AI assistance"
    usage: "t create [project-name]"
    examples:
      - "t create my-website"
      - "t create --template=react my-app"
    requires_tty: true
    notes: "Opens interactive terminal for project setup"
```

#### 3. Cache Version Migration

When updating cache_version from "1.0.0" to "2.0.0":

```yaml
cache_metadata:
  migration_ready: true
  migration_notes: |
    Migrates from cache_version to Git-based tracking
    Old metadata is preserved and converted automatically
    Cache will refresh on next deployment
```

### For Registry Maintainers

#### 1. Adding New Apps

```yaml
# Add to appropriate category (official/community)
apps:
  official:
    - name: new-app
      # ... full app specification
      cache_metadata:
        git_tracking: true
        auto_refresh: true
        health_check: true
        migration_ready: false  # New app
      commands:
        start: "Start the application"
        stop: "Stop the application"
        status: "Check application status"
```

#### 2. Updating Metadata

```bash
# Update registry version
# 1. Update version in apps.yaml header
# 2. Update metadata.version
# 3. Update last_updated timestamp
# 4. Test with registry parser
```

## 🔍 Registry APIs

### Current Implementation

The registry is currently a static YAML file, parsed by tfgrid-compose:

```bash
# Internal registry parsing
get_registry()           # Returns full registry YAML
get_app_repo()           # Get repo URL for app
get_app_metadata()       # Get full app metadata
search_registry()        # Search apps by name/tag
```

### Future API Enhancements

Planned enhancements for registry API integration:

```bash
# Future registry API calls
registry list            # List all apps
registry info <app>      # Get app details
registry validate        # Validate registry structure
registry update          # Update from remote
registry sync            # Sync cache metadata
```

## 📊 Registry Statistics

### Dynamic App Counts

```bash
# Registry provides dynamic counts (computed by parser)
total_apps = len(apps.official) + len(apps.community)
official_apps = len(apps.official)
community_apps = len(apps.community)
cache_enhanced_apps = apps with cache_metadata.git_tracking == true
```

### Usage Analytics

Track registry usage patterns:

```bash
# Most searched apps
t search --stats

# Popular tags
t search --tag-list

# Cache health statistics
t cache stats
```

## 🛠️ Registry Management

### Validation

```bash
# Validate registry structure
t registry validate

# Check app metadata completeness
t registry validate --metadata

# Validate cache integration
t registry validate --cache
```

### Maintenance

```bash
# Update registry metadata
t registry update

# Sync cache metadata
t registry sync

# Backup registry
t registry backup

# Restore registry
t registry restore
```

## 🔐 Security and Quality

### App Review Process

1. **Code Review**: All code reviewed for security and quality
2. **Metadata Validation**: Registry entries validated for completeness
3. **Cache Integration**: Git-based cache integration tested
4. **Documentation**: Required docs and usage examples
5. **Support**: Clear maintenance and support responsibilities

### Quality Standards

- **Official Apps**: Full tfgrid-studio team support and maintenance
- **Community Apps**: Reviewed quality, maintained by original author
- **Cache Integration**: Git-based tracking for all official apps
- **Documentation**: Comprehensive docs for all apps
- **Testing**: Automated validation for registry entries

## 📈 Future Enhancements

### v0.3.0 Planned Features

```yaml
registry_enhancements:
  api_integration:
    - REST API for registry access
    - Webhook support for auto-updates
    - Rate limiting and authentication
  advanced_search:
    - Full-text search across descriptions
    - Tag-based filtering with AND/OR
    - Version and status filtering
  analytics:
    - Usage statistics per app
    - Popular tags and patterns
    - Deployment success rates
  community_features:
    - App rating and reviews
    - Community-maintained categories
    - User-submitted tags and descriptions
```

### v1.0.0 Vision

- **Dynamic Registry**: Auto-updating from Git repositories
- **Plugin System**: Third-party registry sources
- **Enterprise Features**: Private registry support
- **Advanced Analytics**: Comprehensive usage analytics
- **API Gateway**: Full REST API with authentication

## 🔗 Integration Examples

### Complete App Integration Example

```bash
# 1. App appears in registry
t search
# → Shows: tfgrid-ai-stack with "enhanced-cache" tag

# 2. Deploy with enhanced cache
t up tfgrid-ai-stack --name=production

# 3. Cache automatically tracks Git commits
t cache info tfgrid-ai-stack
# → Shows: commit_hash, last_updated, health_status

# 4. Commands available via registry metadata
t commands
# → Shows: create, run, publish, monitor, etc.

# 5. Auto-refresh on source changes
# (When source commits change, cache refreshes automatically)
```

### Registry-Driven Development

```bash
# Start with registry search
t search ai

# Get detailed app information  
t registry info tfgrid-ai-stack

# Deploy with cache integration
t up tfgrid-ai-stack --pattern=single-vm

# Use registry-documented commands
t create          # Registry shows: "Create new AI coding project"
t run            # Registry shows: "Start agent loop for project"  
t publish        # Registry shows: "Publish project for web hosting"

# Monitor via registry integration
t cache list     # Shows Git tracking status
t cache health   # Shows validation results
```

## 🎯 Best Practices

### For Users

1. **Use enhanced cache apps**: Look for "enhanced-cache" tag
2. **Monitor cache health**: Regular `t cache list` checks
3. **Leverage command metadata**: Use `t commands` for available operations
4. **Follow workflow guides**: Check app-specific documentation links

### For Developers

1. **Include full metadata**: Complete cache_metadata section
2. **Document commands**: Clear descriptions for all commands
3. **Enable Git tracking**: Use git_tracking: true
4. **Test cache integration**: Validate cache operations during development
5. **Maintain documentation**: Keep docs, workflow, and metadata in sync

### For Maintainers

1. **Regular validation**: Use `t registry validate` periodically
2. **Update metadata**: Keep cache metadata current
3. **Monitor usage**: Track popular apps and features
4. **Quality review**: Maintain high standards for official apps
5. **Community support**: Help community app maintainers

---

**Next Steps**:
1. Review [Enhanced Workflow Guide](./enhanced-workflow.md) for complete usage
2. Check [Cache Management Guide](./cache-management.md) for cache details
3. Submit community apps following [App Guidelines](./app-guidelines.md)