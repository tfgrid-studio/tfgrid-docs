# Quick Start Guide

Deploy your first application on ThreeFold Grid in **5 minutes**.

:::tip New in v0.10.0
You can now deploy apps directly by name! No need to clone repositories manually.
:::

---

## Prerequisites

Before starting, ensure you have:
- ✅ [TFGrid Compose installed](installation.md)
- ✅ ThreeFold mnemonic configured
- ✅ Terraform/OpenTofu, Ansible, WireGuard installed

---

## 🚀 Quick Start (Registry Method)

**Fastest way to deploy** - uses the app registry (v0.10.0+):

### Step 1: Configure ThreeFold (1 minute)

```bash
# Store mnemonic
mkdir -p ~/.config/threefold
echo "your twelve word mnemonic here" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic

# Set environment variable
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)
# Or for Fish: set -x TF_VAR_mnemonic (cat ~/.config/threefold/mnemonic)
```

### Step 2: Search & Deploy (1 minute)

```bash
# Browse available apps
tfgrid-compose search

# Deploy an app by name
tfgrid-compose up ai-agent

# Watch the magic happen! ✨
# ✅ Fetching from registry...
# ✅ Downloading app...
# ✅ Provisioning infrastructure...
# ✅ Deployment complete!
```

### Step 3: Use Your App

```bash
# Check status
tfgrid-compose status

# View logs
tfgrid-compose logs

# SSH into VM
tfgrid-compose ssh

# For AI agent specifically
tfgrid-compose exec login
```

**That's it!** 🎉

See [Registry Guide](../cli/registry.md) for more details.

---

## 📦 Manual Method (Local Path)

If you prefer working with local repositories:

### Step 1: Setup Workspace (2 minutes)

```bash
# Create workspace
mkdir -p ~/code/github.com/tfgrid-studio
cd ~/code/github.com/tfgrid-studio

# Clone deployer
git clone https://github.com/tfgrid-studio/tfgrid-compose
cd tfgrid-compose

# Install CLI
make install

# Clone an app to deploy (AI agent example)
cd ~/code/github.com/tfgrid-studio
git clone https://github.com/tfgrid-studio/tfgrid-ai-agent
```

---

## Step 2: Configure ThreeFold (1 minute)

```bash
# Store mnemonic
mkdir -p ~/.config/threefold
echo "your twelve word mnemonic here" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic

# Set environment variable
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)
# Or for Fish: set -x TF_VAR_mnemonic (cat ~/.config/threefold/mnemonic)
```

---

## Step 3: Create Context File (30 seconds)

Context files make deployment commands simpler:

```bash
cd ~/code/github.com/tfgrid-studio/tfgrid-compose
echo "app: ../tfgrid-ai-agent" > .tfgrid-compose.yaml
```

Now you can run `tfgrid-compose up` without specifying the app path every time!

---

## Step 4: Deploy! (2-3 minutes)

```bash
# Deploy the application
tfgrid-compose up

# Watch the deployment process:
# ✅ Validating prerequisites...
# ✅ Reading app manifest...
# ✅ Provisioning infrastructure (Terraform)...
# ✅ Setting up WireGuard network...
# ✅ Configuring platform (Ansible)...
# ✅ Deploying application...
# ✅ Running health checks...
# ✅ Deployment complete!
```

**That's it!** Your app is now running on ThreeFold Grid. 🎉

---

## Step 5: Verify Deployment

```bash
# Check deployment status
tfgrid-compose status

# Output:
# App: tfgrid-ai-agent
# Status: Running
# Pattern: single-vm
# Network: main
# IP Address: 10.1.3.2 (WireGuard)
# Deployed: 2025-10-09 13:45:23
```

---

## Step 6: Use Your Application

### SSH Access

```bash
# Connect to the VM
tfgrid-compose ssh

# You're now on the deployed VM!
# root@vm:~#
```

### AI Agent Example

If you deployed tfgrid-ai-agent:

```bash
# Login to Qwen AI
tfgrid-compose agent login

# Create a project
tfgrid-compose agent create
# Enter project name: my-website
# Enter duration: 30
# Enter prompt: Create a beautiful portfolio website with React and Tailwind CSS

# Monitor the AI agent working
tfgrid-compose agent monitor my-website

# List all projects
tfgrid-compose agent list

# Stop the agent when done
tfgrid-compose agent stop my-website
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

## Complete Example Workflow

Here's the entire workflow in one go:

```bash
# Setup (one-time)
mkdir -p ~/code/github.com/tfgrid-studio
cd ~/code/github.com/tfgrid-studio
git clone https://github.com/tfgrid-studio/tfgrid-compose
git clone https://github.com/tfgrid-studio/tfgrid-ai-agent
cd tfgrid-compose
make install

# Configure
mkdir -p ~/.config/threefold
echo "your mnemonic" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)

# Create context
echo "app: ../tfgrid-ai-agent" > .tfgrid-compose.yaml

# Deploy
tfgrid-compose up

# Use
tfgrid-compose agent create
tfgrid-compose agent list
tfgrid-compose ssh

# Clean up
tfgrid-compose down
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
tfgrid-compose agent list          # List projects
tfgrid-compose agent create        # Create project (interactive)
tfgrid-compose agent run <project> # Start AI agent
tfgrid-compose agent monitor <project>  # Monitor progress
tfgrid-compose agent stop <project>     # Stop agent
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
- **[Single-VM Pattern](../patterns/single-vm/)** - Deploy databases, services
- **[AI Agent Guide](../applications/tfgrid-ai-agent/)** - Complete AI agent workflows
- **[Create Your Own App](../applications/creating-apps.md)** - Build deployable apps

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
