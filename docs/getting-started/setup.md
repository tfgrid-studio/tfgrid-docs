# Complete Setup Guide

This guide covers the complete setup process for TFGrid Compose, from installation to productive use. Follow these steps to get started with deploying applications on ThreeFold Grid.

---

## Prerequisites

Before starting, ensure you have:

- ✅ Basic command line knowledge
- ✅ Internet connection
- ✅ ThreeFold Grid account ([threefold.io](https://threefold.io))

---

## Step 1: Install TFGrid Compose

### One-Command Installation

```bash
# Install TFGrid Compose and CLI
curl -sSL install.tfgrid.studio/install.sh | sh
```

**What this does:**
- Downloads and installs `tfgrid-compose` CLI
- Adds it to your PATH automatically
- Creates a default `tfgrid` shortcut
- Sets up configuration directories

### Verify Installation

```bash
# Check installation
tfgrid-compose --version
# Output: TFGrid Compose v0.13.4

# Test basic functionality
tfgrid-compose --help
```

---

## Step 2: Login & Configure Credentials

### Interactive Login

```bash
# Start interactive setup
tfgrid-compose login
```

**Configuration flow:**
1. **ThreeFold Mnemonic**: Enter your 12 or 24-word seed phrase (hidden input for security)
2. **Git Identity**: Configure name and email for AI-generated commits
3. **Optional Tokens**: GitHub/Gitea tokens for private repositories

**Example session:**
```
ThreeFold Mnemonic (required):
  This is your seed phrase from your ThreeFold Chain wallet
  ℹ  Need help? See: tfgrid-compose docs

→ Enter mnemonic (12 or 24 words):
  (input hidden for security)
✅ Validated: 12 words

Git Name (for commits):
→ Git name: John Doe
✅ Git name set: John Doe

Git Email (for commits):
→ Git email: john@example.com
✅ Git email set: john@example.com

✅ Credentials saved securely!
Stored at: /home/user/.config/tfgrid-compose/credentials.yaml
```

### Manual Configuration (Alternative)

If you prefer manual setup:

```bash
# Store mnemonic securely
mkdir -p ~/.config/threefold
echo "your twelve word mnemonic here" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic

# Set environment variable
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)
```

---

## Step 3: Set Up Command Shortcuts

### Create Shortcuts

```bash
# Create convenient shortcuts
tfgrid-compose shortcut tfc    # Creates 'tfc' command
tfgrid-compose shortcut grid   # Creates 'grid' command
```

**Available shortcuts:**
- `tfgrid` (created automatically during install)
- `tfc`, `grid`, or any custom name you choose

### Manage Shortcuts

```bash
# List all shortcuts
tfgrid-compose shortcut --list

# Remove a shortcut
tfgrid-compose shortcut --remove tfc

# Reset to default
tfgrid-compose shortcut --default
```

---

## Step 4: Explore the App Registry

### Browse Available Apps

```bash
# List all available apps
tfgrid-compose search

# Search by keyword
tfgrid-compose search ai
tfgrid-compose search git

# Search by tag
tfgrid-compose search --tag development
tfgrid-compose search --tag ai
```

**Current official apps:**
- `tfgrid-ai-agent` - AI coding assistant (v0.3.0)
- `tfgrid-gitea` - Self-hosted Git service (v1.0.0)

### Get App Information

```bash
# Detailed app information
tfgrid-compose info tfgrid-ai-agent

# Shows: version, resources, patterns, documentation links
```

---

## Step 5: Deploy Your First App

### Deploy AI Agent

```bash
# Deploy the AI coding assistant
tfgrid-compose up tfgrid-ai-agent
```

**Deployment process:**
```
✅ Validating prerequisites...
✅ Downloading from registry...
✅ Provisioning ThreeFold Grid VM (4 CPU, 8GB RAM, 100GB disk)...
✅ Configuring WireGuard networking...
✅ Installing Node.js 20.x, Qwen CLI...
✅ Setting up workspace at /home/developer/code...
✅ Running health checks...
✅ Deployment complete!

ℹ App: tfgrid-ai-agent v0.3.0
ℹ Pattern: single-vm
ℹ Next steps:
  • Check status: tfgrid-compose status tfgrid-ai-agent
  • SSH access: tfgrid-compose ssh tfgrid-ai-agent
```

### Verify Deployment

```bash
# Check deployment status
tfgrid-compose status

# SSH into the VM
tfgrid-compose ssh

# View logs
tfgrid-compose logs
```

---

## Step 6: Use Your AI Agent

### Create First Project

```bash
# Create a new AI coding project
tfgrid-compose create my-website
```

**Interactive prompts:**
1. **Project name**: (auto-filled or enter custom)
2. **Duration**: 30m, 1h, 2h, indefinite
3. **Prompt type**: Custom or template
4. **Your prompt**: Describe what you want the AI to build
5. **Start now?**: y/N

### Monitor Progress

```bash
# Monitor AI coding in real-time
tfgrid-compose monitor my-website

# List all projects
tfgrid-compose projects

# Check project status
tfgrid-compose summary my-website
```

### Access Generated Code

```bash
# SSH to inspect files
tfgrid-compose ssh
cd /home/developer/code/my-website
ls -la
cat README.md
```

---

## Step 7: Deploy Additional Apps

### Deploy Gitea (Git Service)

```bash
# Deploy self-hosted Git service
tfgrid-compose up tfgrid-gitea

# Get access information
tfgrid-compose address tfgrid-gitea
# Output: http://10.1.3.5:3000

# Access at: http://<vm-ip>:3000
# Default credentials: gitadmin / changeme123
```

### Run Multiple Apps Concurrently

```bash
# Deploy both AI agent and Gitea
tfgrid-compose up tfgrid-ai-agent
tfgrid-compose up tfgrid-gitea

# List all deployments
tfgrid-compose list

# Each app runs on its own VM with dedicated resources
```

---

## Configuration & Customization

### Environment Variables

Set these for advanced configuration:

```bash
# Required
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)

# Optional - AI Agent
export QWEN_API_KEY='your-api-key'
export GITHUB_TOKEN='your-token'

# Optional - Gitea
export GITEA_ADMIN_USER='admin'
export GITEA_ADMIN_PASSWORD='secure-password'
```

### Custom Resources

Deploy with custom resource allocation:

```bash
# AI Agent with more resources
tfgrid-compose up tfgrid-ai-agent --cpu 8 --memory 16384 --disk 200

# Gitea with custom domain (requires gateway pattern)
tfgrid-compose up tfgrid-gitea --pattern gateway --domain git.example.com
```

---

## Troubleshooting Setup Issues

### Installation Problems

```bash
# Check system requirements
curl --version
git --version
make --version

# Re-run installation
curl -sSL install.tfgrid.studio/install.sh | sh
```

### Login Issues

```bash
# Check mnemonic format
tfgrid-compose login --check

# Re-enter credentials
tfgrid-compose login
```

### Deployment Failures

```bash
# Check prerequisites
tfgrid-compose up tfgrid-ai-agent --dry-run

# View detailed logs
tfgrid-compose logs tfgrid-ai-agent

# Clean and retry
tfgrid-compose down tfgrid-ai-agent
tfgrid-compose up tfgrid-ai-agent
```

### Network Issues

```bash
# Check WireGuard connection
sudo wg show

# Reconnect if needed
tfgrid-compose ssh tfgrid-ai-agent
# Then from VM: sudo systemctl restart wg-quick@wg0
```

---

## Next Steps

Now that you're set up:

### Learn More
- **[Quick Start](quickstart.md)** - 5-minute deployment guide
- **[AI Agent Guide](../guides/ai-agent.md)** - Complete AI agent workflows
- **[Gitea Guide](../guides/gitea.md)** - Self-hosted Git service
- **[App Registry](../guides/app-registry.md)** - Discover more apps

### Advanced Topics
- **[Pattern Documentation](../patterns/overview.md)** - Deployment patterns
- **[CLI Reference](../cli/app-commands.md)** - Complete command reference
- **[Troubleshooting](../troubleshooting/guide.md)** - Common issues and solutions

### Community
- **GitHub**: [tfgrid-studio](https://github.com/tfgrid-studio)
- **Discussions**: [Community forum](https://github.com/orgs/tfgrid-studio/discussions)
- **Issues**: [Report bugs](https://github.com/tfgrid-studio/tfgrid-compose/issues)

---

**Setup Complete!** 🎉 You're now ready to deploy applications on ThreeFold Grid.

**Quick Commands Reference:**
```bash
# Daily workflow
tfc search              # Browse apps
tfc up <app>           # Deploy app
tfc status             # Check status
tfc ssh                # Access VM
tfc logs               # View logs
tfc down               # Clean up