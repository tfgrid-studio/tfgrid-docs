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
| `t list` | List deployed apps | `t list` |
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

## 🎯 Preference Management (Enhanced)

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