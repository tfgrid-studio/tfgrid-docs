# Installation Guide

Complete setup guide for TFGrid Compose.

---

## Prerequisites

### Required Software

| Tool | Purpose | Installation |
|------|---------|--------------|
| **Terraform or OpenTofu** | Infrastructure provisioning | [Install](https://opentofu.org/docs/intro/install/) |
| **Ansible** | Platform configuration | [Install](https://docs.ansible.com/ansible/latest/installation_guide/) |
| **WireGuard** | Secure networking | [Install](https://www.wireguard.com/install/) |
| **Git** | Version control | Usually pre-installed |

### ThreeFold Account

1. **Create Account:** [ThreeFold Connect](https://manual.grid.tf/documentation/threefold_token/buy_sell_tft/threefold_connect.html)
2. **Get TFT Tokens:** Purchase or earn TFT
3. **Save Mnemonic:** 12-word recovery phrase

---

## Install Prerequisites

### Ubuntu/Debian

```bash
# OpenTofu
curl --proto '=https' --tlsv1.2 -fsSL https://get.opentofu.org/install-opentofu.sh | bash

# Ansible
sudo apt update
sudo apt install ansible

# WireGuard
sudo apt install wireguard

# Optional tools
sudo apt install jq curl wget git
```

### macOS

```bash
# Using Homebrew
brew install opentofu ansible wireguard-tools jq
```

### Verify Installation

```bash
# Check versions
tofu version         # or: terraform version
ansible --version
wg --version
git --version
```

---

## Install TFGrid Compose

### Method 1: Install Script (Recommended)

```bash
# Clone repository
git clone https://github.com/tfgrid-compose/tfgrid-deployer
cd tfgrid-deployer

# Run install script (automatically sets up PATH)
make install

# Verify installation
tfgrid-compose --version
```

The install script:
- ✅ Creates `~/.local/bin/tfgrid-compose` symlink
- ✅ Adds to PATH in your shell config
- ✅ Works with bash, zsh, fish

### Method 2: Manual Installation

```bash
# Clone repository
git clone https://github.com/tfgrid-compose/tfgrid-deployer
cd tfgrid-deployer

# Make CLI executable
chmod +x cli/tfgrid-compose

# Add to PATH (choose your shell)
# Bash/Zsh:
echo 'export PATH="$PATH:'$(pwd)'/cli"' >> ~/.bashrc
source ~/.bashrc

# Fish:
echo 'set -gx PATH $PATH '(pwd)'/cli' >> ~/.config/fish/config.fish
source ~/.config/fish/config.fish

# Verify
tfgrid-compose --version
```

---

## Configure ThreeFold

### 1. Store Mnemonic Securely

```bash
# Create config directory
mkdir -p ~/.config/threefold

# Save mnemonic (replace with your actual mnemonic)
echo "your twelve word mnemonic phrase here" > ~/.config/threefold/mnemonic

# Set secure permissions (owner-only read/write)
chmod 600 ~/.config/threefold/mnemonic

# Verify
cat ~/.config/threefold/mnemonic
```

**Security Note:** The mnemonic file should have `600` permissions (owner-only access).

### 2. Set Environment Variable

The mnemonic needs to be set as an environment variable for Terraform:

#### Bash/Zsh

```bash
# Set for current session
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)

# Or add to shell config for persistence
echo 'export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)' >> ~/.bashrc
source ~/.bashrc
```

#### Fish

```bash
# Set for current session
set -x TF_VAR_mnemonic (cat ~/.config/threefold/mnemonic)

# Or add to fish config for persistence
echo 'set -x TF_VAR_mnemonic (cat ~/.config/threefold/mnemonic)' >> ~/.config/fish/config.fish
source ~/.config/fish/config.fish
```

### 3. Verify Configuration

```bash
# Check mnemonic is set
echo $TF_VAR_mnemonic

# Should output your 12-word mnemonic
```

---

## Setup Workspace (Optional but Recommended)

TFGrid Compose works best with an organized workspace structure:

```bash
# Create standard workspace
mkdir -p ~/code/github.com/tfgrid-compose
cd ~/code/github.com/tfgrid-compose

# Clone deployer
git clone https://github.com/tfgrid-compose/tfgrid-deployer

# Clone apps you want to deploy
git clone https://github.com/tfgrid-compose/tfgrid-ai-agent

# Your workspace should look like:
# ~/code/github.com/tfgrid-compose/
# ├── tfgrid-deployer/
# └── tfgrid-ai-agent/
```

This structure:
- ✅ Keeps everything organized
- ✅ Makes relative paths consistent
- ✅ Follows standard conventions
- ✅ Easy to manage multiple apps

---

## Verify Installation

### 1. Check CLI

```bash
# Show help
tfgrid-compose --help

# Should display:
# TFGrid Compose - Universal deployment orchestrator
# 
# Commands:
#   up [app]       - Deploy application
#   down [app]     - Destroy deployment
#   status [app]   - Show deployment status
#   ...
```

### 2. Check Prerequisites

```bash
# Run built-in validation
cd tfgrid-deployer
make check-prereqs

# Should verify:
# ✅ Terraform/OpenTofu installed
# ✅ Ansible installed
# ✅ WireGuard installed
# ✅ Mnemonic configured
```

### 3. Test Deployment (Optional)

```bash
# Deploy the AI agent to verify everything works
cd ~/code/github.com/tfgrid-compose/tfgrid-deployer
tfgrid-compose up ../tfgrid-ai-agent

# If successful, you'll see:
# ✅ Infrastructure deployed
# ✅ WireGuard configured
# ✅ Platform configured
# ✅ Application deployed
# ✅ Deployment complete!

# Clean up test deployment
tfgrid-compose down ../tfgrid-ai-agent
```

---

## Troubleshooting

### Command Not Found

```bash
# Check if tfgrid-compose is in PATH
which tfgrid-compose

# If not found, add manually:
export PATH="$PATH:$HOME/code/github.com/tfgrid-compose/tfgrid-deployer/cli"
```

### Mnemonic Not Set

```bash
# Check if mnemonic is configured
echo $TF_VAR_mnemonic

# If empty, set it:
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)
```

### Permission Denied

```bash
# Make CLI executable
chmod +x ~/code/github.com/tfgrid-compose/tfgrid-deployer/cli/tfgrid-compose

# Fix mnemonic permissions
chmod 600 ~/.config/threefold/mnemonic
```

### Terraform/OpenTofu Not Found

```bash
# Install OpenTofu (recommended)
curl --proto '=https' --tlsv1.2 -fsSL https://get.opentofu.org/install-opentofu.sh | bash

# Or install Terraform
# See: https://www.terraform.io/downloads
```

### Ansible Not Found

```bash
# Ubuntu/Debian
sudo apt install ansible

# macOS
brew install ansible

# Verify
ansible --version
```

### WireGuard Not Found

```bash
# Ubuntu/Debian
sudo apt install wireguard

# macOS
brew install wireguard-tools

# Verify
wg --version
```

---

## Optional Configuration

### Context File

Create a context file to avoid specifying app paths:

```bash
# In your deployer directory
cd ~/code/github.com/tfgrid-compose/tfgrid-deployer
echo "app: ../tfgrid-ai-agent" > .tfgrid-compose.yaml

# Now you can run commands without app path:
tfgrid-compose up        # instead of: tfgrid-compose up ../tfgrid-ai-agent
tfgrid-compose status    # instead of: tfgrid-compose status ../tfgrid-ai-agent
```

### Shell Aliases

Add convenient aliases to your shell config:

```bash
# Bash/Zsh (~/.bashrc or ~/.zshrc)
alias tfc='tfgrid-compose'
alias tfc-up='tfgrid-compose up'
alias tfc-down='tfgrid-compose down'
alias tfc-status='tfgrid-compose status'

# Fish (~/.config/fish/config.fish)
alias tfc='tfgrid-compose'
alias tfc-up='tfgrid-compose up'
alias tfc-down='tfgrid-compose down'
alias tfc-status='tfgrid-compose status'
```

### SSH Key Setup

If you don't have SSH keys:

```bash
# Generate ed25519 key (recommended)
ssh-keygen -t ed25519 -C "your_email@example.com"

# Or RSA key
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Accept default location (~/.ssh/id_ed25519 or ~/.ssh/id_rsa)
```

---

## Next Steps

Installation complete! 🎉

### What's Next?

1. **[Quick Start Guide](quickstart.md)** - Deploy your first application
2. **[Core Concepts](concepts.md)** - Understand how TFGrid Compose works
3. **[Pattern Documentation](../patterns/overview.md)** - Learn about deployment patterns

---

## Upgrade

To upgrade to the latest version:

```bash
cd ~/code/github.com/tfgrid-compose/tfgrid-deployer
git pull origin main

# Re-install if needed
make install
```

---

## Uninstall

To completely remove TFGrid Compose:

```bash
# Remove symlink
rm ~/.local/bin/tfgrid-compose

# Remove from PATH (edit your shell config and remove the export line)
# Bash: nano ~/.bashrc
# Fish: nano ~/.config/fish/config.fish

# Remove workspace (optional)
rm -rf ~/code/github.com/tfgrid-compose
```

---

**Ready to deploy?** → [Quick Start Guide](quickstart.md)
