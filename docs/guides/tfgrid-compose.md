# TFGrid Compose - Complete User Guide

**Version**: 0.13.5
**Last Updated**: 2025-11-02
**Status**: Production Ready with Farm Browser

TFGrid Compose is the universal deployment orchestrator for the ThreeFold Grid, providing intelligent node selection, app registry integration, comprehensive farm browser, and enhanced filtering capabilities.

## 🚀 Quick Start

### Installation
```bash
cd tfgrid-studio/tfgrid-compose
make install
```

### Basic Workflow
```bash
# 1. Create shortcut (optional)
t shortcut tf

# 2. Login to ThreeFold
t login

# 3. Search available apps
t search

# 4. Deploy an app
t up tfgrid-ai-stack
```

## 📋 Command Structure

### Core Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t help` | Show help message | `t help` |
| `t shortcut <name>` | Create command alias | `t shortcut tf` |
| `t search [query]` | Search apps in registry | `t search ai` |
| `t list` | List deployed apps (Docker-style) | `t list` |
| `t ps` | Docker-style deployment inspection | `t ps` |
| `t select [app]` | Select active app | `t select tfgrid-ai-stack` |
| `t commands` | Show app commands | `t commands` |

### Deployment Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t init <app>` | Initialize app configuration | `t init tfgrid-ai-stack` |
| `t up <app>` | Deploy application | `t up tfgrid-ai-stack` |
| `t down [app]` | Destroy deployment | `t down` |
| `t status [app]` | Check application status | `t status` |
| `t logs [app]` | Show application logs | `t logs` |
| `t ssh [app]` | SSH into deployment | `t ssh` |

### Management Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t nodes --farm <name>` | Show all nodes from specific farm | `t nodes --farm qualiafarm` |
| `t exec <cmd>` | Execute command on active app | `t exec ls -la` |
| `t address [app]` | Show deployment addresses | `t address` |
| `t clean` | Clean up local state | `t clean` |

## 🐳 Docker-Style Deployment System

TFGrid Compose uses a **Docker-style deployment ID system** that provides unique identifiers for each deployment, similar to Docker container IDs.

### What Are Deployment IDs?

Each deployment gets a unique 16-character hexadecimal ID (e.g., `mj4y7bu4a1c2d3e4f`). This system:

- **Ensures uniqueness**: No deployment name conflicts
- **Supports multiple instances**: Deploy the same app multiple times
- **Enables advanced management**: Better tracking and inspection
- **Provides familiar UX**: Docker-style command experience

### How It Works

#### 1. Deployment Creation
```bash
$ t up tfgrid-ai-stack
✅ Generated deployment ID: mj4y7bu4a1c2d3e4f
🎉 Deployment complete!

App: tfgrid-ai-stack v0.12.0-dev
Pattern: single-vm v1.0.0

ℹ 🌐 Access your application:
  • Launch in browser: tfgrid-compose launch tfgrid-ai-stack
```

#### 2. Docker-Style Inspection
```bash
$ t ps
ℹ TFGrid Compose v0.13.4 - Docker-style Deployment List

Deployments (Docker-style):

CONTAINER ID    APP NAME           STATUS          IP ADDRESS
─────────────────────────────────────────────────────────────────
mj4y7bu4a1c2d3e4f  tfgrid-ai-stack    active          10.1.3.2

$ t list
    tfgrid-ai-stack (10.1.3.2) ← Using smart context (only app deployed)
```

#### 3. App Selection
```bash
$ t select
📱 Select an app:

  1) mj4y7bu4a1c2d3e4f tfgrid-ai-stack (10.1.3.2) [active] ← (currently selected)

Enter number [1-1] or 'q' to quit: 1
✅ Selected tfgrid-ai-stack
```

#### 4. Command Execution
```bash
$ t login
ℹ Looking up tfgrid-ai-stack in registry...
✅ Connected to deployment mj4y7bu4a1c2d3e4f (10.1.3.2)
[Login proceeds normally...]

$ t create "math website"
🚀 Creating project: math website
✅ Project created successfully!
📁 Repository: 10.1.3.2/git/mik-tf/mathweb
🌐 Website: 10.1.3.2/web/mik-tf/mathweb
```

### Benefits

#### Better Multi-Deployment Support
```bash
# Deploy multiple instances
$ t up tfgrid-ai-stack --name dev
$ t up tfgrid-ai-stack --name prod

$ t ps
CONTAINER ID    APP NAME           STATUS          IP ADDRESS
─────────────────────────────────────────────────────────────────
a1b2c3d4e5f6g7h8  tfgrid-ai-stack-dev  active          10.1.3.2
i9j0k1l2m3n4o5p6  tfgrid-ai-stack-prod active          10.1.3.3

$ t select tfgrid-ai-stack-dev
```

#### Enhanced Management
- **Unique identification**: No name conflicts
- **State isolation**: Each deployment has independent state
- **Better debugging**: Clear deployment tracking
- **Registry integration**: Centralized deployment management

#### Smart Context System
```bash
# Single deployment - auto-selected
$ t login  # Works immediately

# Multiple deployments - need explicit selection
$ t select tfgrid-ai-stack-dev
$ t login  # Now works for selected deployment
```

### Migration from Legacy System

If you have deployments created before this feature:

#### Check Current Status
```bash
$ t list
# Shows deployments with both ID and name

$ t ps
# Docker-style view
```

#### Automatic Migration
- **Backward compatible**: Existing deployments work normally
- **Gradual transition**: New features work with old deployments
- **No data loss**: All state and configuration preserved

#### Manual Migration (if needed)
```bash
# Force redeploy for new ID system
$ t up tfgrid-ai-stack --force
# Creates new deployment with Docker-style ID
```

### Commands Reference

| Command | Description | Docker Equivalent |
|---------|-------------|-------------------|
| `t ps` | List deployments with IDs | `docker ps` |
| `t inspect <id>` | Inspect deployment details | `docker inspect <id>` |
| `t logs <id>` | View deployment logs | `docker logs <id>` |
| `t exec <id> <cmd>` | Execute command in deployment | `docker exec <id> <cmd>` |
| `t select <app>` | Select active deployment | `docker-compose ps` (interactive) |

### Integration with Existing Workflows

The Docker-style system seamlessly integrates with all existing features:

#### App Registry
```bash
$ t up tfgrid-ai-stack  # Gets unique ID automatically
```

#### Preference System
```bash
$ t whitelist farms "Freefarm"
$ t up tfgrid-ai-stack  # Uses preferences, gets unique ID
```

#### Smart Context
```bash
$ t select tfgrid-ai-stack  # Works with ID-based system
$ t login                   # Automatically uses correct deployment
```

## 🎯 Preference Management (Enhanced)
## � Preference Management (Enhanced)

TFGrid Compose features a powerful **case-insensitive** preference system with support for both farm names and farm IDs.

### Interactive Whitelist Management
```bash
t whitelist
```

**Interactive Menu Options:**
1. **Add node to whitelist** - Add specific node IDs (e.g., "1,2,3")
2. **Add farm to whitelist** - Add farms by name OR ID (e.g., "Freefarm,1")
3. **Remove node from whitelist** - Remove specific nodes
4. **Remove farm from whitelist** - Remove specific farms
5. **View current whitelist** - See current configuration
6. **Clear whitelist** - Remove all whitelist entries
7. **Exit** - Leave the menu

### Enhanced Farm Support
- **Case-Insensitive**: `freefarm` = `Freefarm` = `FREEFARM` ✅
- **Farm IDs**: Use numeric IDs like `1`, `2`, `3` ✅
- **Farm Names**: Use names like `Freefarm`, `MixNMatch` ✅
- **Mixed Input**: `1,Freefarm,2` works perfectly ✅

### Interactive Blacklist Management
```bash
t blacklist
```

Same menu structure as whitelist, but for excluding nodes/farms.

### Quick Commands
```bash
# View current preferences
t whitelist --status
t blacklist --status
t preferences --status

# Clear all preferences
t whitelist --clear
t blacklist --clear
t preferences --clear

# Set preferences directly
t whitelist nodes 1,2,3
t whitelist farms "Freefarm,MixNMatch"
t blacklist farms "BadFarm"
```

## 🌐 Node Filtering Logic

### Whitelist Logic (OR)
A node is allowed if:
- **No whitelist restrictions** OR
- **Node ID is in whitelist** OR  
- **Farm is in whitelist** (case-insensitive)

### Blacklist Logic (Precedence)
Blacklist **always overrides** whitelist:
- Node excluded if in blacklist
- Farm excluded if in blacklist
- Works case-insensitive

### Case-Insensitive Farm Matching
```bash
# These all work the same:
t whitelist farms Freefarm
t whitelist farms freefarm
t whitelist farms FREEFARM
t whitelist farms 1          # Using farm ID
```

## 🗂️ App Registry Integration

### Official Apps
```bash
## 🔧 Bug Fixes & Updates

### v0.13.5 (Latest) - Docker-Style Deployment Registry Fix

**Critical Bug Fixed**: Deployment registration now works properly when `yq` is not available.

**Previous Issue**: 
- Deployments were created successfully but not registered in the deployment registry
- Commands like `t ps`, `t login`, and `t select` would fail with "No deployment found"
- Deployment state was created but not accessible to CLI commands

**Resolution**: 
- Fixed deployment registry fallback logic in `core/deployment-id.sh`
- Added missing file move operation to properly save deployments to registry
- All Docker-style deployment commands now work correctly

**Fixed Commands**:
- ✅ `t ps` - Shows deployments with proper IDs
- ✅ `t login` - Connects to deployment registry successfully  
- ✅ `t select` - Interactive deployment selection
- ✅ `t exec` - Execute commands on deployed applications
- ✅ App-specific commands (create, run, publish) - Work with deployment context

### Registry Requirements
- **With yq**: Full YAML registry functionality
- **Without yq**: Simple text-based registry (automatically used fallback)

## 🎯 TFGrid AI Stack Workflow
t up tfgrid-ai-stack    # AI-powered development platform
t up tfgrid-ai-agent    # AI coding agent
t up tfgridgrid-gitea   # Self-hosted Git service
```

### Community Apps
```bash
# Deploy any GitHub repo
t up username/repo-name
t up https://github.com/user/app
```

### Search and Discovery
```bash
t search              # List all apps
t search ai           # Search for AI apps
t search git          # Search for Git-related apps
```

## 💻 Deployment Options

### Basic Deployment
```bash
t up tfgrid-ai-stack
```

### Advanced Options
```bash
# Force fresh deployment
t up tfgrid-ai-stack --force

# Interactive node selection
t up tfgrid-ai-stack -i

# Specific node
t up tfgrid-ai-stack --node 123

# With custom filters
t up tfgrid-ai-stack --whitelist-farms "Freefarm,MixNMatch"
t up tfgrid-ai-stack --blacklist-nodes "615,888"

# Resource limits
t up tfgrid-ai-stack --max-cpu-usage 80
t up tfgrid-ai-stack --max-disk-usage 60
t up tfgrid-ai-stack --min-uptime-days 3
```

### Node Browser
```bash
# Interactive node browser
t nodes

# NEW: Farm Browser - Show all nodes from specific farm
t nodes --farm qualiafarm      # Shows 27 nodes (10 online, 17 offline)
t nodes farm freefarm          # Alternative syntax
t nodes --farm=qualiafarm      # Alternative syntax
t nodes --farm QUIAFARM        # Case-insensitive matching

# Node details
t nodes show 123

# Favorites (whitelist integration)
t nodes favorites
```

## 🌾 Farm Browser (New!)

The **Farm Browser** provides comprehensive farm node exploration with real-time data and complete coverage.

### Key Features
- **Complete Coverage**: Shows ALL nodes from specified farm (up to 7000 nodes)
- **Real-time Status**: Live online/offline status (🟢 online, 🔴 offline)
- **Detailed Specs**: CPU, RAM, disk, IPv4, load %, uptime for each node
- **Case-Insensitive**: `qualiafarm` = `QualiaFarm` = `QUALIAFARM`
- **Farm Statistics**: Total, online, offline node counts
- **Comprehensive Query**: Uses size=7000 for maximum node coverage

### Usage Examples
```bash
# Browse QualiaFarm nodes (found 27 nodes)
t nodes --farm qualiafarm

# Browse Freefarm nodes (found 25 nodes)
t nodes farm freefarm

# Case-insensitive matching
t nodes --farm QUIAFARM

# Show specific farm with full details
t nodes --farm=qualiafarm
```

### Output Format
```
🏢 Farm: QualiaFarm
📊 Total Nodes: 27
🟢 Online: 10
🔴 Offline: 17

🔍 ThreeFold Node Browser
ID     Farm                 Location        CPU    RAM    Disk   IPv4   Load     Uptime
────────────────────────────────────────────────────────────────────────────────
7414🟢 QualiaFarm           Canada          4      15.4G   250G   Yes    0%       112d
6499🟢 QualiaFarm           Canada          32     251.9G  500G   Yes    0%       112d
6452🟢 QualiaFarm           Canada          8      31.3G   100G   Yes    0%       112d
... (all 27 nodes shown)

Legend: 🟢 = Online node | 🔴 = Offline node
```

### Integration with System
- **Consistent Coverage**: All node queries use size=7000 for maximum discovery
- **Deployment Integration**: More nodes available for deployment selection
- **Performance Optimized**: Query optimized while maintaining comprehensive data
- **Debug Support**: Use `TFC_DEBUG=1` for troubleshooting

## 🔧 Configuration

### Preferences File
Location: `~/.config/tfgrid-compose/preferences.yaml`

```yaml
whitelist:
  nodes: [1, 2, 3]
  farms: ["Freefarm", "MixNMatch"]

blacklist:
  nodes: [615, 888]
  farms: ["BadFarm"]

preferences:
  max_cpu_usage: 80
  max_disk_usage: 60
  min_uptime_days: 3

metadata:
  version: "1.0"
  created: "2025-11-01T00:00:00Z"
  last_updated: "2025-11-01T00:00:00Z"
```

### Config File
Location: `~/.config/tfgrid-compose/config.yaml`

```yaml
# Farm filtering (case-insensitive)
whitelist_farms: "Freefarm,MixNMatch"
blacklist_farms: "BadFarm"

# Node filtering
blacklist_nodes: "615,888"
whitelist_nodes: "1,2,3"

# Resource thresholds
max_cpu_usage: 80
max_disk_usage: 60
min_uptime_days: 3
```

## 🎯 TFGrid AI Stack Workflow

Complete workflow with enhanced preferences:

```bash
# 1. Setup preferences (case-insensitive)
t whitelist farms Freefarm
t blacklist nodes 615

# 2. Deploy tfgrid-ai-stack
t up tfgrid-ai-stack

# 3. Use app commands
t create                    # Create new project
t run                      # Run the project  
t publish                  # Publish to web server
t logs                     # View logs
t status                   # Check status

# 4. Access services
# Gitea (Git): http://10.1.3.2/git/username/reponame
# Web App: http://10.1.3.2/web/username/reponame
```

## 🚨 Troubleshooting

### Common Issues

**No nodes found after applying filters**
```bash
# Check current preferences
t whitelist --status
t blacklist --status

# Clear problematic filters
t whitelist --clear
# or
t blacklist --clear
```

**Farm not found warnings**
```bash
# Farm names are case-insensitive, but must exist
t whitelist farms Freefarm    # ✅ Works
t whitelist farms freefarm    # ✅ Works  
t whitelist farms nonexistent # ⚠️ Will be skipped
```

**Deployment fails**
```bash
# Try without filters
t whitelist --clear
t up tfgrid-ai-stack --force

# Or use interactive mode
t up tfgrid-ai-stack -i
```

### Debug Mode
```bash
# Enable debug logging
export TFC_DEBUG=1
t up tfgrid-ai-stack
```

## 📚 Examples

### Example 1: Basic AI Development
```bash
t shortcut tf                    # Create shortcut
t login                          # Setup credentials
t whitelist farms Freefarm       # Use preferred farm
t up tfgrid-ai-stack             # Deploy AI stack
t create math-calculator         # Create project
t run                           # Run project
t publish                       # Publish to web
```

### Example 2: Multi-Farm Setup
```bash
# Add multiple farms (case-insensitive)
t whitelist farms "Freefarm,mixnmatch,SILVERFARM"

# Add specific nodes
t whitelist nodes "1,2,3"

# Exclude problematic nodes
t blacklist nodes "615,888"
t blacklist farms "BadFarm"

# Deploy
t up tfgrid-ai-stack --force
```

### Example 3: Resource-Aware Deployment
```bash
# Set resource preferences
t preferences
# Choose: Max CPU usage: 70%
# Choose: Max disk usage: 50%  
# Choose: Min uptime days: 5

# Deploy with fresh cache
t up tfgrid-ai-stack --refresh
```

## 🔗 Integration

### With TFGrid AI Stack
```bash
# Preferences automatically apply to deployments
t whitelist farms "Freefarm"
t up tfgrid-ai-stack           # Uses whitelist automatically

# App-specific commands
t commands                     # Show available commands
t select-project              # Select active project
t exec "npm run build"        # Run command in active project
```

### With External Tools
```bash
# Check contracts
t tfcmd-install               # Install tfcmd
t contracts list              # List contracts

# Node verification
t nodes show 123              # Check specific node
t nodes favorites             # View favorite nodes
```

## 📈 Advanced Features

### Farm Cache System
- Automatic farm data caching (1 hour TTL)
- Case-insensitive farm lookup
- Farm ID ↔ Name conversion
- Invalid farm cleanup

### Smart Node Selection
- Health-based filtering
- Resource availability
- Uptime optimization
- Load balancing

### Error Recovery
- Graceful handling of invalid farms
- Warning messages for skipped entries
- Automatic fallback to valid entries
- Continued operation with partial configuration

## 🎉 Summary

TFGrid Compose provides a **production-ready** deployment system with:

✅ **Enhanced Whitelist/Blacklist** with case-insensitive matching  
✅ **Farm ID Support** for both names and numeric IDs  
✅ **Interactive Management** with intuitive menus  
✅ **Smart Filtering** with OR logic and blacklist precedence  
✅ **App Registry Integration** for seamless deployment  
✅ **Error Recovery** with graceful handling  
✅ **Comprehensive Documentation** and examples  

**Ready for Production Use** 🚀