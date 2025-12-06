# Quick Start Guide

Deploy your first application on ThreeFold Grid in **5 minutes**.

:::tip
New in v0.13.4
Complete workflow from installation to deployment with registry integration!
:::

---

## Prerequisites

Before starting, ensure you have:

- ✅ Basic command line knowledge
- ✅ Internet connection
- ✅ ThreeFold Grid account (get one at [threefold.io](https://threefold.io))

---

## 🚀 Complete Quick Start

**Deploy an AI coding assistant in 5 minutes:**

### Step 1: Install TFGrid Compose (30 seconds)

```bash
# One-command installation
curl -sSL install.tfgrid.studio/install.sh | sh
```

This installs `tfgrid-compose` and creates a default `tfgrid` shortcut.

### Step 2: Login & Configure (1 minute)

```bash
# Interactive setup (configures mnemonic, git, optional tokens)
tfgrid-compose login
```

**What it configures:**

- ThreeFold mnemonic (for deployments)
- Git identity (for AI-generated commits)
- Optional: GitHub/Gitea tokens

### Step 3: Set Up Shortcuts (30 seconds)

```bash
# Create convenient shortcuts
tfgrid-compose shortcut tfc  # Creates 'tfc' command

# Or use default shortcut 'tfgrid'
# tfgrid-compose shortcut --default  # Creates 'tfgrid' command
```

### Step 4: Configure Node Preferences (Optional but Recommended) ⭐

**Set your preferred nodes/farms once, use everywhere:**

```bash
# Interactive setup (recommended)
tfgrid-compose whitelist

# Or direct setup
tfgrid-compose whitelist nodes 920,891  # Your favorite nodes
tfgrid-compose whitelist farms FastFarm  # Your preferred farm
tfgrid-compose blacklist nodes 617      # Avoid problematic nodes

# View current preferences
tfgrid-compose preferences --status
```

**This saves you from typing whitelist/blacklist flags on every deployment!**

### Step 5: Deploy Your First App
### Step 3: Set Up Shortcuts (30 seconds)

```bash
# Create convenient shortcuts
tfgrid-compose shortcut tfc  # Creates 'tfc' command
```

### Step 4: Explore & Deploy (2 minutes)

```bash
# Browse available apps
tfc search

# Deploy AI agent by name
tfc up tfgrid-ai-agent

# Watch deployment:
# ✅ Validating prerequisites...
# ✅ Downloading from registry...
# ✅ Provisioning ThreeFold Grid VM...
# ✅ Configuring networking...
# ✅ Installing AI agent...
# ✅ Deployment complete!
```

### Step 5: Use Your AI Agent (1 minute)

```bash
# Create your first AI project
tfc create my-website

# Monitor AI coding in real-time
tfc monitor my-website

# Check all projects
tfc projects

# SSH to see generated code
tfc ssh
cd /home/developer/code/my-website
ls -la
```

**That's it!** 🎉 Your AI agent is now autonomously coding on ThreeFold Grid.

---

## 📦 Alternative: Manual Setup

If you prefer working with local repositories or need custom configurations:

### Manual Installation

```bash
# Create workspace
mkdir -p ~/code/github.com/tfgrid-studio
cd ~/code/github.com/tfgrid-studio

# Clone repositories
git clone https://github.com/tfgrid-studio/tfgrid-compose
git clone https://github.com/tfgrid-studio/tfgrid-ai-agent

# Install CLI
cd tfgrid-compose
make install

# Configure mnemonic
mkdir -p ~/.config/threefold
echo "your twelve word mnemonic" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)

# Create context file
echo "app: ../tfgrid-ai-agent" > .tfgrid-compose.yaml

# Deploy
tfgrid-compose up
```

See [Complete Setup Guide](setup.md) for detailed manual instructions.

---

## Step 6: Verify & Use

### Check Deployment Status

```bash
# Check deployment status
tfc status

# Output:
# App: tfgrid-ai-agent
# Status: Running
# Pattern: single-vm
# Network: main
# IP Address: 10.1.3.2 (WireGuard)
# Deployed: 2025-10-21 15:52:00
```

### Use Your AI Agent

```bash
# Create a project
tfc create my-first-app
# Interactive prompts guide you through setup

# Monitor AI coding in real-time
tfc monitor my-first-app

# List all projects
tfc projects

# SSH to inspect generated code
tfc ssh
cd /home/developer/code/my-first-app
ls -la
cat README.md

# Stop when done
tfc stop my-first-app
```

### View Logs

```bash
# View application logs
tfgrid-compose logs
```

---

## Step 7: Clean Up

When you're done:

```bash
# Destroy the deployment
tfgrid-compose down

# Confirm: yes

# All resources are now deleted:
# ✅ VM destroyed
# ✅ Network cleaned up
# ✅ State removed
```

---

## Complete Workflow Summary

```bash
# 1. Install & Setup (one-time)
curl -sSL install.tfgrid.studio/install.sh | sh
tfgrid-compose login
tfgrid-compose shortcut tfc

# 2. Explore & Deploy
tfc search
tfc up tfgrid-ai-agent

# 3. Use AI Agent
tfc create my-project
tfc monitor my-project
tfc projects

# 4. Access & Inspect
tfc ssh
cd /home/developer/code/my-project
ls -la

# 5. Clean up
tfc stop my-project
tfc down
```

---

## Available Commands

### Deployment

```bash
tfgrid-compose up [app]       # Deploy application
tfgrid-compose down [app]     # Destroy deployment
tfgrid-compose status [app]   # Show status
```

### Access

```bash
tfgrid-compose ssh [app]      # SSH to VM
tfgrid-compose logs [app]     # View logs
tfgrid-compose exec [app] <cmd>  # Execute command
```

### AI Agent (when using tfgrid-ai-agent)

```bash
tfgrid-compose projects            # List projects
tfgrid-compose create              # Create project (interactive)
tfgrid-compose run <project>       # Start AI agent
tfgrid-compose monitor <project>   # Monitor progress
tfgrid-compose stop <project>      # Stop agent
```

### Patterns

```bash
tfgrid-compose patterns       # List available patterns
```

---

## Without Context File

If you don't use a context file, specify the app path:

```bash
# Deploy
tfgrid-compose up ../tfgrid-ai-agent

# Status
tfgrid-compose status ../tfgrid-ai-agent

# SSH
tfgrid-compose ssh ../tfgrid-ai-agent

# Destroy
tfgrid-compose down ../tfgrid-ai-agent
```

---

## Multiple Applications

You can deploy multiple apps:

```bash
# Deploy app 1
tfgrid-compose up ../tfgrid-ai-agent

# Deploy app 2
tfgrid-compose up ../my-other-app

# Each app gets its own:
# - VM
# - Network
# - State
# - Resources
```

---

## Troubleshooting

### Deployment Fails

```bash
# Check prerequisites
tfgrid-compose --help
tofu version
ansible --version
wg --version

# Check mnemonic
echo $TF_VAR_mnemonic

# Try again
tfgrid-compose down
tfgrid-compose up
```

### Can't Connect to VM

```bash
# Check WireGuard
sudo wg show

# Reconnect
cd ~/code/github.com/tfgrid-studio/tfgrid-compose
make wg APP=../tfgrid-ai-agent

# Test SSH
tfgrid-compose ssh
```

### Command Not Found

```bash
# Re-install CLI
cd ~/code/github.com/tfgrid-studio/tfgrid-compose
make install

# Or add to PATH manually
export PATH="$PATH:$(pwd)/cli"
```

### App Not Found

```bash
# Check context file
cat .tfgrid-compose.yaml

# Should show:
# app: ../tfgrid-ai-agent

# Or specify full path
tfgrid-compose up ../tfgrid-ai-agent
```

See [Troubleshooting Guide](../guides/troubleshooting.md) for more solutions.

---

## Next Steps

Now that you've deployed your first app:

### Learn More
- **[Core Concepts](concepts.md)** - Understand patterns, apps, and manifests
- **[Pattern Documentation](../patterns/overview.md)** - Learn about deployment patterns
- **[CLI Reference](../reference/cli.md)** - Complete command reference

### Deploy More Apps
- **[AI Agent Guide](../guides/ai-agent.md)** - Complete AI agent workflows
- **[Gitea Guide](../guides/gitea.md)** - Self-hosted Git service
- **[App Registry](../guides/app-registry.md)** - Discover more applications

### Advanced Topics
- **[Advanced Deployment](../guides/deployment.md)** - Production strategies
- **[Networking Guide](../guides/networking.md)** - WireGuard and Mycelium
- **[Security Best Practices](../guides/security.md)** - Secure your deployments

---

## Quick Reference Card

```bash
# Setup (one-time)
make install
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)
echo "app: ../my-app" > .tfgrid-compose.yaml

# Daily workflow
tfgrid-compose up        # Deploy
tfgrid-compose status    # Check status
tfgrid-compose ssh       # Connect
tfgrid-compose logs      # View logs
tfgrid-compose down      # Destroy
```

---

**Congratulations!** You've successfully deployed on ThreeFold Grid. 🚀

Need help? [Join our community](https://github.com/orgs/tfgrid-compose/discussions) or [open an issue](https://github.com/tfgrid-studio/tfgrid-compose/issues).
