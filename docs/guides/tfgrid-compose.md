# TFGrid Compose - Complete User Guide

**Version**: Git commit-based (0a7195b)
**Semantic Version**: v0.14.1
**Last Updated**: 2025-12-02
**Status**: Production Ready with Git Commit Versioning & Grid-Authoritative Architecture

TFGrid Compose is the universal deployment orchestrator for the ThreeFold Grid, providing intelligent node selection, app registry integration, comprehensive farm browser, enhanced filtering capabilities, and **grid-authoritative deployment management**.

## 🚀 Quick Start

### Prerequisites

**Required Dependencies:**

- **tfcmd**: Essential for grid contract management and validation
  ```bash
  curl -fsSL https://raw.githubusercontent.com/threefoldtech/tfcmd/main/install.sh | bash
  ```

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
t signin

# 3. Search available apps
t search

# 4. Deploy an app
t up tfgrid-ai-stack
```

## 🌐 Local Dashboard (Built-in UI)

TFGrid Compose includes a **local web dashboard** feature for managing apps and deployments visually.

- Launch the dashboard in the foreground:

  ```bash
  t dashboard
  ```

- Run it as a local service:

  ```bash
  t dashboard start
  t dashboard status
  t dashboard logs
  t dashboard stop
  ```

For a full walkthrough of the dashboard UI and lifecycle commands, see the dedicated guide:

- [TFGrid Dashboard](tfgrid-dashboard.md)

## 🎯 Git Commit-Based Versioning

TFGrid Compose now uses **Git commit hashes as the primary version identifier** across the entire ecosystem, providing precise code traceability and automatic version management.

### Enhanced Version Display

**Tool Version:**
```bash
$ t --version
TFGrid Compose cef6d4d
Semantic version: v0.14.1
```

**Application Deployment:**
```bash
$ t up tfgrid-ai-stack
✅ Application loaded: tfgrid-ai-stack 24c9148
ℹ Git commit: 24c9148
ℹ Last updated: 2025-11-11 22:47:49
ℹ Branch: main
ℹ Repository: https://github.com/tfgrid-studio/tfgrid-ai-stack.git
```

**Cache Status:**
```bash
$ t cache list
✅ tfgrid-ai-stack (24c9148)
    Last updated: 2025-11-11 22:47:49
⚠️ tfgrid-ai-agent (0e91178) - [needs update]
    Last updated: 2025-11-02 08:23:48
```

### Benefits

- ✅ **Precise Versioning**: Every deployment has a unique, immutable version identifier
- ✅ **Automatic Management**: No manual version bumping required - Git handles it automatically
- ✅ **Instant Traceability**: Know exactly which code is running in each deployment
- ✅ **Enhanced Debugging**: Can track exactly what changed between deployments
- ✅ **Consistent Approach**: All TFGrid components use the same Git commit versioning
- ✅ **Smart Cache**: Cache invalidation based on Git commit changes

### Enhanced Error Reporting

**Before:**
```
Application loaded: tfgrid-ai-stack v0.12.0-dev
```

**After:**
```
✅ Application loaded: tfgrid-ai-stack 24c9148
ℹ Git commit: 24c9148
ℹ Last updated: 2025-11-11 22:47:49
ℹ Branch: main
ℹ Repository: https://github.com/tfgrid-studio/tfgrid-ai-stack.git
```

## 📋 Command Structure

### Core Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t help` | Show help message | `t help` |
| `t shortcut <name>` | Create command alias | `t shortcut tf` |
| `t search [query]` | Search apps in registry | `t search ai` |
| `t ps` | List deployments with timestamps & ages | `t ps` |
| `t ps --all` | Show all deployments with active contracts (including incomplete) | `t ps --all` |
| `t ps --outside` | Show grid contracts not tracked in local registry (SOURCE=outside) | `t ps --outside` |
| `t select [app]` | Select active app (supports partial IDs) | `t select tfgrid-ai-stack` |
| `t select --force [id]` | Force select incomplete/failed deployments | `t select --force e0c` |
| `t commands` | Show app commands | `t commands` |
| `t dashboard [start\|stop\|status\|logs]` | Local web dashboard for apps and deployments | `t dashboard` |

### Deployment Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t init <app>` | Initialize app configuration | `t init tfgrid-ai-stack` |
| `t up <app>` | Deploy application | `t up tfgrid-ai-stack` |
| `t down [app]` | Destroy deployment | `t down` |
| `t status [app]` | Check application status | `t status` |
| `t logs [app]` | Show application logs | `t logs` |
| `t ssh [app]` | SSH into deployment | `t ssh` |
| `t ssh --direct [id]` | Direct SSH (bypass validation, for failed deployments) | `t ssh --direct e0c` |
| `t ssh --ipv4` | SSH forcing IPv4 network | `t ssh --ipv4` |
| `t ssh --mycelium` | SSH forcing Mycelium network | `t ssh --mycelium` |

### Management Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t nodes --farm <name>` | Show all nodes from specific farm | `t nodes --farm qualiafarm` |
| `t exec <cmd>` | Execute command on active app | `t exec ls -la` |
| `t address [app]` | Show deployment addresses | `t address` |
| `t clean` | Clean up local state | `t clean` |
| `t update [subcommand]` | Enhanced update system | `t update registry` |
| `t cache [subcommand]` | Cache management system | `t cache status` |

### Contract & State Cleanup Commands
| Command | Description | Example |
|---------|-------------|---------|
| `t contracts list` | List all contracts | `t contracts list` |
| `t contracts delete <id...>` | Delete contract(s) by ID | `t contracts delete 12345 67890` |
| `t contracts delete -c <id>` | Delete by container ID (short ID) | `t contracts delete -c f13` |
| `t contracts orphans` | Find orphaned contracts | `t contracts orphans` |
| `t contracts orphans --delete` | Delete orphaned contracts | `t contracts orphans --delete` |
| `t contracts clean -i` | Interactive cleanup | `t contracts clean -i` |
| `t state list` | List state directories | `t state list` |
| `t state clean` | Remove orphaned state dirs and registry entries | `t state clean` |

### Network Management Commands

| Command | Description | Example |
|---------|-------------|---------|
| `t network provision <networks>` | Set networks to provision on VM | `t network provision ipv4,mycelium` |
| `t network prefer <networks>` | Set connection preference order | `t network prefer mycelium,ipv4` |
| `t network get` | Show current network settings | `t network get` |
| `t network list` | List available networks | `t network list` |
| `t network test` | Test connectivity to all networks | `t network test` |

**Available Networks:**
- `ipv4` - Public IPv4 address (direct internet access)
- `ipv6` - Public IPv6 address (direct internet access)
- `mycelium` - Mycelium overlay network (global IPv6, encrypted)
- `wireguard` - WireGuard VPN (private network)

**Provisioning Examples:**
```bash
t network provision ipv4,mycelium    # Provision both IPv4 and Mycelium
t network provision mycelium         # Mycelium only (minimal)
t network provision all              # Provision all available networks
```

**Connection Preference:**
The prefer command sets the order in which networks are tried when connecting:
```bash
t network prefer mycelium,ipv4       # Try mycelium first, then IPv4
t network prefer ipv4                # Only use IPv4
```

**SSH Network Override:**
Force a specific network for a single SSH connection:
```bash
t ssh --ipv4       # or -4
t ssh --ipv6
t ssh --mycelium   # or -m  
t ssh --wireguard  # or -w
```

## Update and Cache Commands

### Update Commands

| Command | Description | Example |
|---------|-------------|---------|
| `t update` | Update tfgrid-compose binary | `t update` |
| `t update registry` | Update registry and all cached apps | `t update registry` |
| `t update <app-name>` | Update specific app | `t update tfgrid-ai-stack` |
| `t update --all-apps` | Update all cached apps | `t update --all-apps` |

### Cache Commands

| Command | Description | Example |
|---------|-------------|---------|
| `t cache status` | Show cache health overview | `t cache status` |
| `t cache list` | List all cached apps with status | `t cache list` |
| `t cache outdated` | Show apps needing updates | `t cache outdated` |
| `t cache refresh` | Auto-refresh stale apps | `t cache refresh` |
| `t cache validate` | Validate cached apps integrity | `t cache validate` |
| `t cache clear [app\|--all]` | Clear cache (specific app or all) | `t cache clear tfgrid-ai-stack` |
| `t cache info` | Show cache statistics | `t cache info` |

### Update System Overview

#### Registry Updates
The `t update registry` command performs two operations:
1. Fetches the latest app registry metadata from `tfgrid-studio/tfgrid-registry`
2. Updates all cached app repositories to their latest versions

#### App Cache System
- **Location**: Apps are cached in `~/.config/tfgrid-compose/apps/`
- **Version Tracking**: Uses Git commit hashes to track app versions
- **Smart Updates**: Existing git repositories are updated with `git pull`
- **Health Monitoring**: Cache status shows if apps are healthy, stale, invalid, or not cached

#### Cache Structure
```
~/.config/tfgrid-compose/
├── registry/
│   └── apps.yaml              # Registry metadata
├── apps/
│   ├── tfgrid-ai-stack/       # Cached app repositories
│   ├── tfgrid-ai-agent/
│   └── ...
└── cache-metadata/
    ├── tfgrid-ai-stack.json   # Version tracking data
    └── ...
```

### Common Usage Patterns

#### Initial Setup
```bash
# Update registry and download all apps
t update registry

# Check cache status
t cache status

# Search available apps
t search

# Deploy an app
t up tfgrid-ai-stack
```

#### Regular Maintenance
```bash
# Update registry and apps
t update registry

# Check for outdated apps
t cache outdated

# Refresh stale cache
t cache refresh

# Deploy with fresh cache
t up tfgrid-ai-stack --refresh
```

#### Troubleshooting
```bash
# Check cache health
t cache status

# Validate specific app
t cache validate tfgrid-ai-stack

# Clear problematic cache
t cache clear tfgrid-ai-stack

# Download fresh copy
t cache refresh tfgrid-ai-stack
```

### Rate Limiting Protection

The system includes GitHub API rate limiting:
- 5-second delays between requests
- 3 retry attempts per app
- Clear error messages for rate limiting issues

```bash
# Example output when rate limited
⚠️  GitHub rate limiting detected for tfgrid-ai-stack
     This usually means too many requests were made to GitHub
     Wait a few minutes and try again
```

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
ℹ TFGrid Compose v0a7195b - Docker-Style Deployment Listing

Deployments (Docker-style):

CONTAINER ID    APP NAME           STATUS    IP ADDRESS    CONTRACT  SOURCE    AGE
──────────────────────────────────────────────────────────────────────────────────
mj4y7bu4a1c2d3e4f tfgrid-ai-stack   active    10.1.3.2      1631275   registry  10h ago

$ t list
tfgrid-ai-stack (10.1.3.2) ← Using smart context (only app deployed)

# Show contracts that exist on the grid but are not tracked locally
$ t ps --outside
ℹ TFGrid Compose v0a7195b - Docker-Style Deployment Listing

Deployments (Docker-style):

CONTRACT ID     APP NAME           STATUS    SOURCE
────────────────────────────────────────────────────
1727260         ubunvm             active    outside
```

#### 3. App Selection
```bash
$ t select
📱 Select an app:

  1) mj4y7bu4a1c2d3e4f tfgrid-ai-stack (10.1.3.2) [active] ← (currently selected)

Enter number [1-1] or 'q' to quit: 1
✅ Selected tfgrid-ai-stack
```

#### 4. Grid-Authoritative Architecture
```bash
# Contract validation ensures logical consistency
$ t contracts list
ℹ Fetching contracts via tfcmd...
[Sentry] 2025/11/03 00:26:49 Using release from debug info: 171e8b2b4ddbdfcb2623c09311d317990a7e4de6
12:26AM INF starting peer session=tf-90662 twin=6905

Node contracts:
ID        Node ID    Type    Name                    Project Name
1629826   8          VM      tfgrid-ai-stack-mj4y7b  mj4y7bu4a1c2d3e4f
1629827   8          VM      tfgrid-ai-stack-a94dc2  a94dc2809ed701b8

Name contracts:
ID    Name
```

#### 5. Partial ID Resolution
```bash
# Unique partial matches work automatically
$ t login mj4
✅ Logged in to mj4y7bu4a1c2d3e4f tfgrid-ai-stack

# Ambiguous matches show selection menu
$ t select tfgrid-ai-stack
📱 Select deployment:

  1) mj4y7bu4a1c2d3e4f tfgrid-ai-stack (10.1.3.2) - 2h ago
  2) a94dc2809ed701b8 tfgrid-ai-stack (10.1.3.2) - 1h ago

Enter number [1-2] or 'q' to quit: 1
✅ Selected mj4y7bu4a1c2d3e4f tfgrid-ai-stack
```

## 🎯 Dual Identification System (Contract ID + Container ID)

TFGrid Compose uses a **dual identification system** where each deployment gets two types of identifiers for different purposes.

### Two ID Types Per Deployment

Every deployment receives both:

| ID Type | Purpose | Example | Usage |
|---------|---------|---------|-------|
| **Container ID** | User operations (Docker-style UX) | `u4lavu3x` | `t login u4`, `t inspect u4` |
| **Contract ID** | Grid validation (authoritative) | `1631275` | `t list` validation, `t inspect 1631275` |

### How It Works

#### Registry Storage
```yaml
# Each deployment stores both IDs:
deployments:
  u4lavu3x:                           # Container ID (16-char hex)
    app_name: tfgrid-ai-stack
    contract_id: "1631275"            # Grid contract ID (authoritative)
    vm_ip: 10.1.3.2
    created_at: "2025-11-03T04:19:19Z"
    status: active
```

#### User Operations
```bash
# Container ID (familiar Docker-style)
t login u4                           # Resolves to: u4lavu3x
t inspect u4lavu3x                   # Direct container ID access

# Contract ID (grid-authoritative)  
t inspect 1631275                    # Direct contract ID access
t list                               # Validates contract exists on grid

# Smart resolution
t select tfgrid-ai-stack             # Shows menu with BOTH IDs
```

#### Enhanced Display
```bash
CONTAINER ID    APP NAME         CONTRACT    STATUS    IP ADDRESS    AGE
─────────────────────────────────────────────────────────────────────────
u4lavu3x        tfgrid-ai-stack   1631274    active    10.1.3.2     10h ago
934ab460dd5663f2 tfgrid-ai-stack   1631275    active    10.1.3.2     10h ago
```

### Benefits

#### ✅ Best of Both Worlds
- **Container ID**: Intuitive Docker-style user experience
- **Contract ID**: Authoritative grid validation

#### ✅ Flexible Operations
- Users can operate with either identifier
- System automatically validates against grid contracts  
- Clear separation: UX layer vs. validation layer

#### ✅ Robust Cross-Validation
- Registry entries always link container_id ↔ contract_id
- Grid contracts provide definitive source of truth
- "Ghost deployments" automatically filtered out

### Cross-Validation Example

```bash
# Check grid contracts (reality)
$ t contracts list
Node contracts:
ID        Node ID    Type    Name                    Project Name
1631275   8          VM      tfgrid-ai-stack         u4lavu3x

# Show all registry entries (history + debugging)  
$ t ps
CONTAINER ID    APP NAME         STATUS    IP ADDRESS    CONTRACT  SOURCE    AGE
────────────────────────────────────────────────────────────────────────────────
u4lavu3x        tfgrid-ai-stack  active    10.1.3.2      1631275   registry  10h ago

# Show only grid-valid deployments (authoritative)
$ t list
tfgrid-ai-stack (10.1.3.2) ← Validated against grid contract 1631275
```

### Migration Notes

#### For New Users
- Both ID types work transparently
- Use whichever is more convenient
- System handles validation automatically

#### For Existing Users
- Container IDs remain unchanged
- Contract IDs are automatically added
- Enhanced validation improves accuracy

#### Problem Solved
**Before**: Dual-source truth problem where `t list` showed deployments that didn't exist on the grid
```bash
t ps     → Shows 4 deployments (registry)
t contracts → Shows 0 contracts (grid reality)
❌ LOGICAL INCONSISTENCY
```

**After**: Grid-authoritative approach with contract validation
```bash
t list     → Shows 0 deployments (matches grid)
t contracts → Shows 0 contracts (authoritative)
✅ LOGICAL CONSISTENCY
```

## 🛠️ Installation & Dependencies

### Auto-Installation
The installation script automatically checks for dependencies:

#### 1. tfcmd Dependency
```bash
# make install will check for tfcmd and offer auto-install
cd tfgrid-studio/tfgrid-compose
make install

# Installation process:
🔍 Checking tfcmd dependency...
⚠️  tfcmd not found - Required for ThreeFold Grid operations
Install tfcmd now? (y/N): y
🚀 Installing tfcmd...
✅ tfcmd installed successfully
```

#### What's Included
- **Required**: tfcmd is now essential for basic functionality
- **Auto-install**: `make install` offers to install tfcmd automatically  
- **Validation**: Installation script checks for tfcmd presence
- **Grid operations**: Contract management, status validation, deployment linkage

#### 2. Contract ID Linkage
```yaml
# Registry stores bidirectional mapping
deployments:
  abc123:
    app_name: tfgrid-ai-stack
    vm_ip: 10.1.3.2
    contract_id: "1629826"  # Links to grid contract
    created_at: "2025-11-03T12:37:27Z"
    status: "active"
```

#### 3. Auto-Cleanup System
- **Orphan removal**: Registry entries without corresponding grid contracts
- **Legacy cleanup**: Old entries without contract IDs
- **Transparent logging**: All cleanup actions are logged
- **Grid precedence**: Grid reality always takes priority

#### 4. Enhanced Docker-Style Display
```bash
$ t ps
CONTAINER ID    APP NAME           STATUS    IP ADDRESS    CONTRACT  SOURCE    AGE
──────────────────────────────────────────────────────────────────────────────────
abc123def456   tfgrid-ai-stack     active    10.1.3.2     1629826   registry  2h ago
```

#### 5. Smart Contract Management
```bash
# Bi-directional operations
t down abc123  # Finds contract 1629826 → Cancels it → Removes registry entry

# Cross-validation
t list         # Only shows deployments that exist in both registry AND grid

# Contract Management CLI commands
# List all contracts
t contracts list

# Delete a single contract
t contracts delete <contract-id>

# Delete multiple contracts
t contracts delete 12345 67890 11111

# Delete by container ID (supports short IDs like Docker)
t contracts delete --container f13
t contracts delete -c 66b982

# Find orphaned contracts (exist on grid but not in local state)
t contracts orphans

# Delete orphaned contracts
t contracts orphans --delete

# Interactive cleanup (shows numbered list, select which to delete)
t contracts clean --interactive
t contracts clean -i

# Delete all contracts (dangerous)
t contracts delete --all
```

#### 6. State Directory Management
```bash
# List all state directories
t state list

# Clean orphaned state directories (no active contracts)
t state clean

# Dry run (see what would be removed)
t state clean --dry-run

# Force clean without confirmation
t state clean --force
```

### Migration Guide

#### For Existing Users
1. **Automatic**: Registry entries without contracts are cleaned up automatically
2. **Transparent**: Cleanup actions are logged for visibility
3. **Safe**: No data loss, only synchronization with grid reality

#### For New Installations
1. **Install tfcmd**: Required dependency (auto-installed via `make install`)
2. **Login setup**: `t signin` for ThreeFold Grid access
3. **Contract validation**: All operations now validate against grid state

### Benefits
- **Logical Consistency**: No more ghost deployments
- **Single Source of Truth**: ThreeFold Grid contracts via tfcmd
- **Enhanced Security**: Contract-based validation
- **Better UX**: Clear indication of actual deployment state
- **Automatic Maintenance**: Self-cleaning registry system

## ☸️ K3s Cluster Deployments & Retry Mechanism

TFGrid Compose supports **K3s cluster deployments** for high-availability production workloads. K3s clusters are deployed using Ansible playbooks that provision multi-node Kubernetes clusters with HA etcd, MetalLB load balancing, and Nginx ingress controllers.

### K3s Pattern Overview

K3s deployments use a **9-VM architecture**:
- **1 Management Node**: Ansible orchestration, kubectl access, monitoring tools
- **3 Control Plane Nodes**: K3s masters with HA etcd cluster
- **3 Worker Nodes**: Application pod scheduling
- **2 Ingress Nodes**: Nginx ingress controllers with Keepalived HA

### Deployment Process

```bash
# Deploy K3s cluster using pattern
t up <your-app> --pattern k3s

# This provisions 9 VMs and runs ansible playbooks to:
# 1. Configure common prerequisites on all nodes
# 2. Initialize K3s control plane (HA etcd)
# 3. Join worker nodes to the cluster
# 4. Deploy MetalLB for load balancing
# 5. Deploy Nginx ingress controllers
# 6. Validate cluster health
```

### 🔄 Ansible Retry Mechanism (New Feature!)

If K3s deployment fails partially, you can **retry specific ansible tasks** without redeploying all VMs. This saves significant time and cost by preserving infrastructure while fixing orchestration issues.

#### Key Benefits
- ⚡ **Fast Recovery**: Ansible retries take minutes, not hours
- 💰 **Cost Effective**: No VM reprovisioning costs
- 🎯 **Selective**: Fix only broken components
- 📊 **Transparent**: Clear visibility into what succeeded/failed

#### Retry Commands

```bash
# Check detailed deployment status (selects current/active deployment)
tfgrid-compose status

# Retry specific failing components on current deployment
tfgrid-compose retry control    # Control plane only
tfgrid-compose retry worker     # Worker nodes only
tfgrid-compose retry common     # Common prerequisites
tfgrid-compose retry ingress    # Ingress nodes only
tfgrid-compose retry all        # All components

# Validate cluster health after retry
tfgrid-compose validate
```

#### State Tracking

The retry system uses **state files** on the management node to track component completion:

```bash
# Check state files (run on management node)
ls -la /var/lib/tfgrid-compose/state/
# control_complete     - Control plane setup done
# worker_node1_complete - Worker node1 joined
# worker_node2_complete - Worker node2 joined
# worker_node3_complete - Worker node3 joined
```

#### How It Works

1. **Infrastructure Preserved**: VMs remain deployed, only ansible tasks are retried
2. **Component Isolation**: Each ansible role can be retried independently
3. **State Awareness**: System checks completion markers before running tasks
4. **Smart Validation**: Cluster health checks run after each retry attempt

#### Example Recovery Workflow

```bash
# Initial deployment fails
t up <your-app> --pattern k3s
# ❌ Error: Worker node 2 failed to join cluster

# Check deployment status
t status
# Shows current deployment status and any failed components

# Retry only the failed worker nodes
t retry worker
# ✅ Worker node2 successfully joined

# Validate cluster health
t validate
# ✅ Cluster validation passed: 6 nodes ready
```

### Error Handling Improvements

The retry system includes enhanced error handling:

- **Proper Failures**: Critical errors fail immediately (no more `ignore_errors: yes`)
- **Retry Logic**: Built-in retries with exponential backoff
- **Clear Diagnostics**: Detailed error messages guide recovery
- **Validation Checks**: Health checks between deployment phases

### When to Use Retry vs. Redeploy

| Scenario | Action | Reason |
|----------|--------|--------|
| VM provisioning failed | `t up <app>` (full redeploy) | Infrastructure issue |
| Ansible task timeout | `./scripts/retry-playbook.sh <tag>` | Orchestration issue |
| Network connectivity | `./scripts/retry-playbook.sh <tag>` | Transient network issue |
| Single node failure | `./scripts/retry-playbook.sh worker` | Isolated component failure |
| Control plane issue | `./scripts/retry-playbook.sh control` | Control plane specific |

## 🔧 Troubleshooting Failed Deployments

When a deployment fails mid-way (e.g., during the configure hook), the VM may be running on the grid but the local state is incomplete. TFGrid Compose provides special flags to access these deployments.

### The Problem

When deployment fails after VM creation but before completion:
- VM is running on the grid with an active contract
- IP addresses are registered in the deployment registry
- Local state directory may be missing or incomplete
- Standard commands like `t ssh` or `t select` fail validation

### Recovery Commands

#### Force Select (`--force` / `-f`)

Select a deployment even if it failed validation:

```bash
# Force select by partial ID (Docker-style)
t select --force e0c

# Force select by app name
t select -f my-app

# Output shows deployment info from registry:
# ✅ Selected e0c0418f2a8f4d5f (my-app) [forced]
# ℹ VM IP: 10.1.2.2
# ℹ Mycelium: 496:e335:519a:61c0:ff0f:8f18:11b7:5c1e
# ℹ Status: active
```

After force-selecting, you can use `t address` to see deployment details.

#### Direct SSH (`--direct` / `-d`)

SSH directly using registry data, bypassing all validation:

```bash
# Direct SSH by partial ID
t ssh --direct e0c

# Direct SSH by app name
t ssh -d my-app

# Output:
# ℹ Direct SSH to my-app (e0c0418f2a8f4d5f) via mycelium...
```

### When to Use These Flags

| Scenario | Command | Why |
|----------|---------|-----|
| Configure hook failed | `t ssh --direct <id>` | VM exists, need to debug |
| State directory missing | `t select --force <id>` | Registry has IP, state gone |
| Need to see deployment IP | `t select -f <id>` then `t address` | Quick IP lookup |
| Manual recovery needed | `t ssh -d <id>` | Fix deployment manually |

### Recovery Workflow Example

```bash
# 1. List all deployments (including failed)
t ps --all

# 2. Force select the failed deployment
t select --force e0c
# ✅ Selected e0c0418f2a8f4d5f (my-app) [forced]

# 3. Check the addresses
t address
# Shows VM IP and Mycelium IP

# 4. SSH in to debug/fix
t ssh --direct e0c

# 5. Once fixed, you can retry or clean up
t down e0c0418f2a8f4d5f  # Clean up if needed
```

### Partial ID Support (Docker-Style)

Both `--force` and `--direct` support Docker-style partial ID matching:

```bash
# If you have deployments:
# - e0c0418f2a8f4d5f (my-app)
# - abc123def4567890 (other-app)

t select --force e0c    # Matches e0c0418f2a8f4d5f
t ssh --direct abc      # Matches abc123def4567890

# Ambiguous matches show an error:
# ❌ Ambiguous partial ID: a
# Matches:
#   abc123def4567890 (other-app)
#   a1b2c3d4e5f67890 (another-app)
```
