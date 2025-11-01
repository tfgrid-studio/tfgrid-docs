# Node Selection & Filtering

Advanced node selection controls for precise deployment targeting on the ThreeFold Grid.

:::info Version
Advanced node filtering is available in **v0.14.0+**
:::

---

## Overview

TFGrid Compose provides multiple ways to control which nodes your deployments use:

- **Automatic Selection**: Smart defaults based on resource requirements
- **Manual Selection**: Specify exact node IDs
- **Persistent Preferences**: ⭐ Set whitelist/blacklist once, apply to all deployments
- **Advanced Filtering**: Health-based filtering and one-time CLI flags
- **Interactive Setup**: Easy guided configuration with visual feedback

---

## Quick Start

**🚀 NEW: Persistent Preferences (Recommended)**

Set your preferences once, use everywhere:

```bash
# Interactive setup (easiest)
tfgrid-compose whitelist

# Direct setup
tfgrid-compose whitelist nodes 920,891
tfgrid-compose blacklist nodes 617

# All deployments now auto-use your preferences
tfgrid-compose up myapp
```

**Classic Method (Still Supported)**

```bash
# Auto-select best node (default)
tfgrid-compose up myapp

# Use specific node
tfgrid-compose up myapp --node=574

# Filter by health
tfgrid-compose up myapp --max-cpu-usage=70 --min-uptime-days=30

# One-time blacklist (deprecated - use persistent preferences above)
tfgrid-compose up myapp --blacklist-node=617
```

---

## Node Selection Methods

### 1. Automatic Selection (Default)

TFGrid Compose automatically selects the best available node:

```bash
tfgrid-compose up tfgrid-ai-agent
```

**Selection Criteria:**
- ✅ Healthy nodes only
- ✅ Non-dedicated nodes (available for rent)
- ✅ Sufficient resources (CPU, RAM, disk)
- ✅ Highest uptime (most stable)

### 2. Manual Node Selection

Specify exact node ID for precise control:

```bash
# Deploy to specific node
tfgrid-compose up myapp --node=574

# Verify node exists first
tfgrid-compose up myapp --node=574 --dry-run
```

### 3. Advanced Filtering

Combine multiple filters for precise targeting:

```bash
# Health-based filtering
tfgrid-compose up myapp --max-cpu-usage=70 --max-disk-usage=80

# Farm preferences
tfgrid-compose up myapp --whitelist-farm=Freefarm
tfgrid-compose up myapp --blacklist-farm=BadFarm

# Node exclusions
tfgrid-compose up myapp --blacklist-node=617,892

# Combined filters
tfgrid-compose up myapp \
  --whitelist-farm=Freefarm \
  --max-cpu-usage=70 \
  --min-uptime-days=30
```

---

## Configuration System

### Global Configuration File

Set persistent preferences in `~/.config/tfgrid-compose/config.yaml`:

```yaml
# Node filtering (comma-separated lists)
blacklist_nodes: "617,892"
blacklist_farms: "BadFarm,ProblemFarm"
whitelist_farms: "Freefarm,TrustedFarm"

# Health thresholds (percentage usage)
max_cpu_usage: 70
max_disk_usage: 80

# Minimum uptime requirements (days)
min_uptime_days: 30
```
### Persistent Preferences System

**Set your whitelist and blacklist preferences once, apply to all deployments!**

The persistent preferences system allows you to configure node and farm filtering that automatically applies to all your deployments, eliminating the need to specify the same flags repeatedly.

```bash
# Interactive setup (recommended)
tfgrid-compose whitelist
# → Guided menu to set whitelist and blacklist preferences

# Set whitelist nodes
tfgrid-compose whitelist nodes 920,891

# Set whitelist farms
tfgrid-compose whitelist farms FastFarm,ReliableFarm

# Set blacklist nodes
tfgrid-compose blacklist nodes 615,888

# Set blacklist farms
tfgrid-compose blacklist farms SlowFarm,ProblematicFarm

# View current preferences
tfgrid-compose preferences --status
```

**Benefits:**

- ✅ **One-time setup** - Configure once, use everywhere
- ✅ **Interactive mode** - Easy guided configuration
- ✅ **Visual status** - Clear view of current preferences
- ✅ **CLI overrides** - Can still override for special cases
- ✅ **Team sharing** - YAML config file is shareable

**Configuration file:** `~/.config/tfgrid-compose/preferences.yaml`

### Legacy Configuration Commands

The old `config` system is deprecated in favor of the new preference commands above:

### Configuration Commands

```bash
# Interactive configuration (recommended)
tfgrid-compose config
# → Shows menu to set/get values interactively

# Initialize configuration
tfgrid-compose config init

# View current settings
tfgrid-compose config show

# Set individual values
tfgrid-compose config set blacklist_nodes "617,892"
tfgrid-compose config set max_cpu_usage 70

# Get specific values
tfgrid-compose config get blacklist_nodes
```

### CLI Override Precedence

CLI flags override configuration file settings:

```bash
# Config file has max_cpu_usage: 70
# CLI flag overrides it
tfgrid-compose up myapp --max-cpu-usage=50
```

---

## Filtering Options

### Node Blacklisting

Exclude specific nodes that have issues:

---

## Persistent Preferences Guide

### Setting Up Persistent Preferences

**Step 1: Interactive Setup (Recommended)**

The easiest way to set up your preferences:

```bash
tfgrid-compose whitelist
```

This opens an interactive menu where you can:
1. Add whitelist nodes
2. Add whitelist farms  
3. Add blacklist nodes
4. Add blacklist farms
5. Set deployment preferences (CPU, disk, uptime)
6. View current preferences
7. Clear preferences
8. Save and exit

**Step 2: Direct Commands**

Or use direct commands for quick setup:

```bash
# Whitelist preferred nodes and farms
tfgrid-compose whitelist nodes 920,891
tfgrid-compose whitelist farms FastFarm,ReliableFarm

# Blacklist problematic nodes and farms
tfgrid-compose blacklist nodes 615,888
tfgrid-compose blacklist farms SlowFarm,ProblematicFarm

# Set deployment preferences
tfgrid-compose whitelist  # Interactive mode for preferences
```

**Step 3: View Current Settings**

```bash
# View all preferences
tfgrid-compose preferences --status

# Shows:
# ✅ Whitelist (preferred nodes/farms):
#    Nodes: 920, 891
#    Farms: FastFarm, ReliableFarm
# ❌ Blacklist (avoided nodes/farms):
#    Nodes: 615, 888
#    Farms: SlowFarm, ProblematicFarm
# ⚙️  Deployment Preferences:
#    Max CPU Usage: 80%
#    Max Disk Usage: 60%
#    Min Uptime: 3 days
```

### Using Persistent Preferences

Once configured, all deployments automatically use your preferences:

```bash
# These commands automatically apply your persistent preferences
tfgrid-compose up tfgrid-ai-stack
tfgrid-compose up tfgrid-gitea
tfgrid-compose up tfgrid-ai-agent

# Each deployment shows:
# ✅ Applying whitelist: nodes=920,891 farms=FastFarm,ReliableFarm
# ✅ Applying blacklist: nodes=615 farms=SlowFarm
```

### Overriding Persistent Preferences

You can still override preferences for specific deployments:

```bash
# Use persistent whitelist but override with specific node
tfgrid-compose up tfgrid-ai-stack --node=920

# Override whitelist for this deployment only
tfgrid-compose up tfgrid-ai-stack --whitelist-node=920,891 --blacklist-node=615

# Override with different whitelist farm
tfgrid-compose up tfgrid-ai-stack --whitelist-farm=PremiumFarm
```

**Precedence Order:**
1. CLI flags (highest precedence)
2. Persistent preferences
3. Default behavior (lowest precedence)

### Managing Preferences

**Clear specific preferences:**
```bash
tfgrid-compose whitelist --clear      # Clear all whitelist
tfgrid-compose blacklist --clear      # Clear all blacklist
```

**Update preferences:**
```bash
# Add more nodes to whitelist
tfgrid-compose whitelist nodes 920,891,1234

# Update blacklist farms
tfgrid-compose blacklist farms SlowFarm,ProblematicFarm,BadFarm
```

**Check what will be applied:**
```bash
tfgrid-compose preferences --status
```

### Configuration File

Preferences are stored in YAML format at:
`~/.config/tfgrid-compose/preferences.yaml`

```yaml
# TFGrid Compose Persistent Preferences
whitelist:
  nodes: [920, 891]
  farms: [FastFarm, ReliableFarm]

blacklist:
  nodes: [615, 888]
  farms: [SlowFarm, ProblematicFarm]

preferences:
  max_cpu_usage: 80
  max_disk_usage: 60
  min_uptime_days: 3

metadata:
  version: "1.0"
  last_updated: "2025-10-31T17:03:00Z"
```

**Benefits of YAML format:**
- ✅ **Human-readable** - Easy to edit manually
- ✅ **Shareable** - Team members can copy preference files
- ✅ **Version control** - Track changes over time
- ✅ **Backup/restore** - Simple file copy

### Common Use Cases

**Development Environment:**
```bash
tfgrid-compose whitelist nodes 920,891  # Fast, reliable nodes
tfgrid-compose whitelist farms FastFarm # Preferred farm
```

**Production Environment:**
```bash
# More restrictive preferences for production
tfgrid-compose blacklist nodes 615,888  # Avoid known problematic nodes
tfgrid-compose blacklist farms SlowFarm # Avoid slow farms
tfgrid-compose whitelist nodes 920,891,1234  # Only use proven nodes
```

**Team Environment:**
```bash
# Share preferences file with team
cp ~/.config/tfgrid-compose/preferences.yaml team-preferences.yaml
# Team members copy to their ~/.config/tfgrid-compose/preferences.yaml
```

**Cost Optimization:**
```bash
# Prefer cheaper farms
tfgrid-compose whitelist farms Freefarm,AffordableFarm
```

**Performance Optimization:**
```bash
# Prefer high-performance nodes
tfgrid-compose whitelist nodes 920,891,1234  # Top-tier nodes
tfgrid-compose blacklist farms SlowFarm      # Avoid slow farms
```

---

## Filtering Options
```bash
# Single node
tfgrid-compose up myapp --blacklist-node=617

# Multiple nodes
tfgrid-compose up myapp --blacklist-node=617,892,1234

# Persistent config
tfgrid-compose config set blacklist_nodes "617,892"
```

### Farm Filtering

Control which farms to use or avoid:

```bash
# Prefer specific farms (whitelist)
tfgrid-compose up myapp --whitelist-farm=Freefarm
tfgrid-compose up myapp --whitelist-farm=Freefarm,DigitalOceanFarm

# Avoid problematic farms (blacklist)
tfgrid-compose up myapp --blacklist-farm=BadFarm
```

### Health-Based Filtering

Filter nodes by resource utilization and stability:

```bash
# CPU usage threshold (percentage)
tfgrid-compose up myapp --max-cpu-usage=70

# Disk usage threshold (percentage)
tfgrid-compose up myapp --max-disk-usage=80

# Minimum uptime requirement (days)
tfgrid-compose up myapp --min-uptime-days=30
```

**Health metrics are calculated as:**
- **CPU Usage**: `(used_resources.cru / total_resources.cru) * 100`
- **Disk Usage**: `(used_resources.sru / total_resources.sru) * 100`
- **Uptime**: `uptime_seconds / 86400` (days)

---

## Use Cases

### High-Performance Workloads

For GPU workloads or performance-critical apps:

```bash
# Prefer farms known for good hardware
tfgrid-compose up gpu-app \
  --whitelist-farm=NVIDIAFarm,GoogleFarm \
  --max-cpu-usage=50 \
  --min-uptime-days=60
```

### Cost Optimization

For cost-sensitive deployments:

```bash
# Use less utilized nodes (lower competition)
tfgrid-compose up myapp \
  --max-cpu-usage=30 \
  --max-disk-usage=50
```

### Reliability First

For production deployments:

```bash
# Prioritize stable, long-running nodes
tfgrid-compose up production-app \
  --min-uptime-days=90 \
  --max-cpu-usage=60
```

### Troubleshooting

When deployments fail on certain nodes:

```bash
# Blacklist problematic nodes
tfgrid-compose up myapp --blacklist-node=617,892

# Add to persistent config
tfgrid-compose config set blacklist_nodes "617,892"
```

---

## Technical Details

### GridProxy API Integration

All filtering uses the GridProxy API (`https://gridproxy.grid.tf`):

```json
{
  "nodeId": 574,
  "farmName": "Freefarm",
  "country": "Germany",
  "healthy": true,
  "dedicated": false,
  "uptime": 1328644,
  "used_resources": {
    "cru": 35,
    "sru": 1597727834112,
    "hru": 0,
    "mru": 95701535130
  },
  "total_resources": {
    "cru": 56,
    "sru": 2000409772032,
    "hru": 144001663500288,
    "mru": 202711719936
  }
}
```

### Filtering Logic

1. **Resource Check**: Basic CPU/RAM/disk availability
2. **Health Filter**: `healthy == true && dedicated == false`
3. **Blacklist Filter**: Exclude specified nodes/farms
4. **Whitelist Filter**: Include only specified farms (if set)
5. **Health Thresholds**: Apply usage percentage limits
6. **Uptime Filter**: Minimum uptime requirement
7. **Scoring**: Sort by uptime (highest first)

### Performance Considerations

- Filters are applied client-side after GridProxy query
- Large result sets (size=50) ensure good selection
- Configuration cached locally for fast access
- API calls respect rate limits

---

## Examples

### Example 1: Production Deployment

```bash
# Configure for production use
tfgrid-compose config set whitelist_farms "Freefarm,TrustedFarm"
tfgrid-compose config set max_cpu_usage 60
tfgrid-compose config set min_uptime_days 30

# Deploy with additional filters
tfgrid-compose up production-app \
  --blacklist-node=617 \
  --max-disk-usage=75
```

### Example 2: Development Environment

```bash
# Relaxed filters for dev
tfgrid-compose up dev-app \
  --max-cpu-usage=90 \
  --min-uptime-days=7
```

### Example 3: GPU Workload

```bash
# Target GPU-capable farms
tfgrid-compose up gpu-app \
  --whitelist-farm=NVIDIAFarm,AMDfarm \
  --max-cpu-usage=50
```

---

## Troubleshooting

### No Nodes Match Filters

```bash
tfgrid-compose up myapp --max-cpu-usage=10
# Error: No nodes found after applying filters
```

**Solutions:**
- Relax filter criteria
- Remove restrictive filters
- Check configuration: `tfgrid-compose config show`

### Configuration Not Working

```bash
# Check config file exists
ls ~/.config/tfgrid-compose/config.yaml

# Reinitialize if corrupted
tfgrid-compose config init
```

### Node Not Available

```bash
tfgrid-compose up myapp --node=99999
# Error: Node 99999 is not available
```

**Solutions:**
- Use auto-selection: `tfgrid-compose up myapp`
- Check node status on dashboard.grid.tf
- Remove `--node` flag

---

## Best Practices

### 1. Start Simple

Begin with automatic selection, add filters as needed:

```bash
# Start basic
tfgrid-compose up myapp

# Add filters incrementally
tfgrid-compose up myapp --max-cpu-usage=70
tfgrid-compose up myapp --whitelist-farm=Freefarm
```

### 2. Use Configuration for Common Settings

Set frequently used filters globally:

```bash
tfgrid-compose config set max_cpu_usage 70
tfgrid-compose config set min_uptime_days 30
```

### 3. Override for Special Cases

Use CLI flags to override config for specific deployments:

```bash
# Config has max_cpu_usage: 70
# Override for this deployment
tfgrid-compose up special-app --max-cpu-usage=30
```

### 4. Monitor and Adjust

Check deployment success rates and adjust filters:

```bash
# If deployments fail often, relax filters
tfgrid-compose config set max_cpu_usage 80

# If getting poor performance, tighten filters
tfgrid-compose config set min_uptime_days 60
```

---

## Related

- [CLI Commands](./commands.md) - Complete command reference
- [Deployment Patterns](../patterns/) - Pattern-specific deployment
- [Troubleshooting](../troubleshooting/) - Common issues and solutions
- [GridProxy API](https://gridproxy.grid.tf) - Node data source