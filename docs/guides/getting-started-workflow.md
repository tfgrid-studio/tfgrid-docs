# Complete Getting Started Workflow

This guide provides a comprehensive overview of the complete user journey for TFGrid Compose, from first installation to productive AI-assisted development on ThreeFold Grid.

---

## User Journey Overview

```
1. Install → 2. Login → 3. Shortcuts → 4. Registry → 5. Deploy → 6. Use → 7. Scale
```

Each step builds on the previous, creating a smooth onboarding experience.

---

## Phase 1: Installation & Setup

### Step 1: One-Command Installation

```bash
# Install everything needed
curl -sSL install.tfgrid.studio/install.sh | sh
```

**What happens:**
- Downloads TFGrid Compose CLI (v0.13.4)
- Installs to `~/.local/bin/tfgrid-compose`
- Adds to PATH automatically
- Creates default `tfgrid` shortcut
- Sets up configuration directories

**Verification:**
```bash
tfgrid-compose --version  # TFGrid Compose v0.13.4
tfgrid --help            # Works via shortcut
```

### Step 2: Interactive Login

```bash
# Configure all credentials interactively
tfgrid-compose login
```

**Configuration collected:**
- **ThreeFold Mnemonic**: 12/24-word seed phrase (secure input)
- **Git Identity**: Name and email for commits
- **Optional Tokens**: GitHub/Gitea for private repos

**Security features:**
- Hidden input for sensitive data
- Validation of mnemonic format
- Secure file permissions (600)
- Encrypted storage in `~/.config/tfgrid-compose/credentials.yaml`

### Step 3: Command Shortcuts

```bash
# Create convenient shortcuts
tfgrid-compose shortcut tfc
tfgrid-compose shortcut grid
```

**Benefits:**
- Shorter commands (`tfc up` vs `tfgrid-compose up`)
- Custom naming (`grid`, `tfc`, etc.)
- Multiple shortcuts allowed
- Easy management (`--list`, `--remove`)

---

## Phase 2: Discovery & Exploration

### Step 4: App Registry Exploration

```bash
# Discover available applications
tfgrid-compose search        # List all apps
tfgrid-compose search ai     # Search by keyword
tfgrid-compose search --tag development  # Search by tag
```

**Current registry contents:**
- **tfgrid-ai-agent** (v0.3.0): AI coding assistant
- **tfgrid-gitea** (v1.0.0): Self-hosted Git service

**Registry features:**
- Cached locally (1-hour TTL)
- Search by name, description, tags
- Version information and requirements
- Direct deployment from registry

### Step 5: App Information & Selection

```bash
# Get detailed app information
tfgrid-compose info tfgrid-ai-agent
```

**Information provided:**
- Version and status
- Resource requirements (CPU, RAM, disk)
- Supported patterns
- Documentation links
- Repository information

---

## Phase 3: Deployment & Usage

### Step 6: First Deployment

```bash
# Deploy AI agent from registry
tfgrid-compose up tfgrid-ai-agent
```

**Complete deployment flow:**
1. ✅ Validate prerequisites (mnemonic, tools)
2. ✅ Download app from registry/cache
3. ✅ Provision ThreeFold Grid VM (4 CPU, 8GB RAM, 100GB disk)
4. ✅ Configure WireGuard networking
5. ✅ Run Ansible playbooks for setup
6. ✅ Install dependencies (Node.js, Qwen CLI)
7. ✅ Configure systemd services
8. ✅ Health checks and verification

**Time:** 2-3 minutes
**Cost:** Pay-as-you-go on ThreeFold Grid

### Step 7: Basic Usage

```bash
# Verify deployment
tfgrid-compose status

# Access the VM
tfgrid-compose ssh

# View logs
tfgrid-compose logs
```

---

## Phase 4: AI Agent Workflow

### Step 8: Create AI Project

```bash
# Create first AI coding project
tfgrid-compose create my-website
```

**Interactive workflow:**
1. **Project Name**: Auto-suggested or custom
2. **Duration**: 30m, 1h, 2h, indefinite
3. **Git Config**: Auto-detected from login
4. **Prompt Type**: Custom prompt or template
5. **Coding Prompt**: User's specific requirements
6. **Start Now**: Immediate execution option

### Step 9: Monitor AI Development

```bash
# Real-time monitoring
tfgrid-compose monitor my-website

# Project overview
tfgrid-compose projects

# Detailed status
tfgrid-compose summary my-website
```

**AI Agent features:**
- Qwen CLI integration with yolo mode
- Git commits after each successful iteration
- Time management and safety constraints
- Concurrent project execution
- Systemd service management with auto-restart

### Step 10: Access Results

```bash
# SSH to inspect generated code
tfgrid-compose ssh
cd /home/developer/code/my-website
ls -la
cat README.md
git log --oneline
```

---

## Phase 5: Scaling & Integration

### Step 11: Deploy Additional Services

```bash
# Deploy Gitea for code storage
tfgrid-compose up tfgrid-gitea

# List all deployments
tfgrid-compose list
```

### Step 12: Multi-App Workflows

```bash
# Run multiple AI projects concurrently
tfgrid-compose create frontend-app
tfgrid-compose create backend-api
tfgrid-compose create docs

# Start all projects
tfgrid-compose run frontend-app &
tfgrid-compose run backend-api &
tfgrid-compose run docs &

# Monitor everything
tfgrid-compose projects
```

**Concurrent execution:**
- Each project runs in isolated systemd service
- Dedicated resource limits (2GB memory, 150% CPU quota)
- Automatic failure recovery
- Independent git repositories

### Step 13: Integration Workflows

**AI Agent + Gitea Integration:**
```bash
# Configure git remote in AI projects
tfgrid-compose ssh tfgrid-ai-agent
cd /home/developer/code/my-project
git remote add origin http://gitea-vm-ip:3000/gitadmin/my-project.git
git push -u origin main
```

---

## Phase 6: Management & Cleanup

### Step 14: Resource Management

```bash
# Stop specific projects
tfgrid-compose stop my-website

# Stop all running projects
tfgrid-compose stopall

# Check resource usage
tfgrid-compose status
```

### Step 15: Cleanup

```bash
# Destroy deployments when done
tfgrid-compose down tfgrid-ai-agent
tfgrid-compose down tfgrid-gitea

# Clean local cache (optional)
tfgrid-compose prune
```

---

## Complete Workflow Summary

### Quick Start (5 minutes)
```bash
# 1. Install
curl -sSL install.tfgrid.studio/install.sh | sh

# 2. Configure
tfgrid-compose login
tfgrid-compose shortcut tfc

# 3. Deploy & Use
tfc search
tfc up tfgrid-ai-agent
tfc create my-project
tfc monitor my-project
```

### Full Development Stack (15 minutes)
```bash
# Install & setup
curl -sSL install.tfgrid.studio/install.sh | sh
tfgrid-compose login
tfgrid-compose shortcut tfc

# Deploy services
tfc up tfgrid-ai-agent
tfc up tfgrid-gitea

# Create projects
tfc create frontend
tfc create backend
tfc create docs

# Run concurrently
tfc run frontend &
tfc run backend &
tfc run docs &

# Monitor all
tfc projects
```

---

## Workflow Diagrams

### Basic User Journey
```
Install CLI → Login → Set Shortcuts → Search Registry → Deploy App → Create Project → Monitor → Access Code
```

### AI Development Workflow
```
User Prompt → AI Agent VM → Qwen CLI → Code Generation → Git Commit → User Review → Iterate/Deploy
```

### Multi-App Architecture
```
User Machine
├── tfgrid-compose CLI
└── Multiple ThreeFold VMs
    ├── AI Agent VM (Projects: frontend, backend, docs)
    ├── Gitea VM (Repositories)
    └── Future: Database VM, Web Server VM
```

---

## Troubleshooting Common Issues

### Installation Issues
```bash
# Check prerequisites
curl --version && git --version && make --version

# Reinstall
curl -sSL install.tfgrid.studio/install.sh | sh
```

### Login Problems
```bash
# Validate credentials
tfgrid-compose login --check

# Reconfigure
tfgrid-compose login
```

### Deployment Failures
```bash
# Check logs
tfgrid-compose logs

# Clean and retry
tfgrid-compose down
tfgrid-compose up tfgrid-ai-agent
```

### Network Issues
```bash
# Check WireGuard
sudo wg show

# Restart networking
tfgrid-compose exec sudo systemctl restart wg-quick@wg0
```

---

## Next Steps & Resources

### Continue Learning
- **[AI Agent Guide](ai-agent.md)** - Deep dive into AI development
- **[Gitea Guide](gitea.md)** - Self-hosted Git workflows
- **[App Registry](app-registry.md)** - Discover more applications
- **[CLI Reference](../cli/app-commands.md)** - Complete command reference

### Advanced Topics
- **[Pattern Documentation](../patterns/overview.md)** - Gateway and K3s patterns
- **[Custom Apps](../development/app-manifest.md)** - Build your own deployable apps
- **[Security Best Practices](../guides/security.md)** - Production deployments

### Community & Support
- **GitHub**: [tfgrid-studio](https://github.com/tfgrid-studio)
- **Discussions**: [Community forum](https://github.com/orgs/tfgrid-studio/discussions)
- **Issues**: [Bug reports](https://github.com/tfgrid-studio/tfgrid-compose/issues)

---

**Complete workflow mastered!** 🚀 You're now equipped to deploy, manage, and scale applications on ThreeFold Grid with AI assistance.