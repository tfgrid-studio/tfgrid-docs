# TFGrid AI Stack Pattern

**Complete AI development platform with integrated Git hosting**

The TFGrid AI Stack pattern deploys a comprehensive AI-powered development environment on ThreeFold Grid, combining AI coding assistance, Git hosting, and monitoring in a single deployment.

---

## Overview

```
Internet
    ↓
[Gateway VM] ← nginx, APIs, monitoring, SSL
    ↓ (WireGuard VPN)
[AI Agent VM] ← project creation, code generation
    ↓
[Gitea VM] ← Git repositories, web interface
```

**Perfect For:**

- AI-assisted development workflows
- Team development environments
- Self-hosted code generation platforms
- Complete development stacks

---

## Quick Start

```bash
tfgrid-compose up tfgrid-ai-stack
```

**Deploy time:** ~10 minutes  
**Cost:** ~$15-20/month

---

## Features

- 🤖 **AI-Powered Development** - Create projects with AI assistance using Qwen
- 📦 **Integrated Git Hosting** - Built-in Gitea for version control
- 🌐 **Public Deployment** - Automatic web deployment with SSL
- 📊 **Monitoring** - Prometheus + Grafana dashboards
- 🔒 **Secure Networking** - WireGuard VPN for inter-VM communication
- ⚡ **One-Click Deployment** - Single command setup
- 🔄 **Automated Backups** - Scheduled backups with retention policies

---

## Architecture

### Components

| Component | Purpose | Default Resources |
|-----------|---------|-------------------|
| **Gateway VM** | Public API gateway, nginx, monitoring, SSL | 2 CPU, 4GB RAM |
| **AI Agent VM** | Project creation, code generation | 4 CPU, 8GB RAM |
| **Gitea VM** | Git repositories, web interface | 2 CPU, 4GB RAM |

### Network Flow

```
User Request → Gateway VM (Public IPv4)
                    ↓
              nginx reverse proxy
                    ↓
         ┌─────────┴─────────┐
         ↓                   ↓
    AI Agent API        Gitea Web
    (WireGuard)        (WireGuard)
```

All inter-VM communication uses WireGuard VPN for security.

---

## Usage

### Deploy

```bash
# Deploy with defaults
tfgrid-compose up tfgrid-ai-stack

# Deploy with custom domain
tfgrid-compose up tfgrid-ai-stack --var domain=ai.example.com --var ssl_email=admin@example.com
```

### Authenticate with Qwen

```bash
tfgrid-compose login
```

### Create a Project

```bash
# Interactive project creation
tfgrid-compose create

# Or with arguments
tfgrid-compose create my-portfolio "A personal portfolio website"
```

### Manage Projects

```bash
# List all projects
tfgrid-compose projects

# Monitor a project
tfgrid-compose monitor my-portfolio

# View project logs
tfgrid-compose logs my-portfolio

# Stop a project
tfgrid-compose stop my-portfolio

# Remove a project
tfgrid-compose remove my-portfolio
```

### Access Services

```bash
# Open web interface in browser
tfgrid-compose launch

# Get deployment addresses
tfgrid-compose address

# SSH into the stack
tfgrid-compose ssh
```

### Backup & Restore

```bash
# Manual backup
tfgrid-compose backup

# Restore from backup
tfgrid-compose restore backup-2025-12-06.tar.gz
```

---

## Configuration

### Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `domain` | string | "" | Domain for public access |
| `ssl_email` | string | "" | Email for SSL certificate |
| `vm_cpu` | number | 8 | Total CPU cores |
| `vm_memory` | number | 16384 | Total memory (MB) |
| `vm_disk` | number | 200 | Total disk (GB) |

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `API_RATE_LIMIT` | 100r/m | API rate limiting |
| `BACKUP_RETENTION_DAYS` | 30 | Backup retention period |
| `BACKUP_SCHEDULE` | 0 2 * * * | Backup cron schedule |

---

## Custom Commands

The TFGrid AI Stack provides extensive custom commands:

### Project Management

| Command | Description |
|---------|-------------|
| `create` | Create a new AI coding project |
| `run` | Start agent loop for a project |
| `stop` | Stop agent loop for a project |
| `monitor` | Monitor project progress |
| `projects` | Show status of all projects |
| `remove` | Delete a project |
| `logs` | View project logs |
| `summary` | Show project summary |
| `restart` | Restart agent for a project |
| `stopall` | Stop all running projects |

### Authentication

| Command | Description |
|---------|-------------|
| `login` | Authenticate with Qwen (OAuth) |
| `whoami` | Check Qwen authentication status |

### Hosting

| Command | Description |
|---------|-------------|
| `publish` | Publish project for web hosting |
| `unpublish` | Remove project from web hosting |
| `hosting-status` | Show hosting status |

### System

| Command | Description |
|---------|-------------|
| `launch` | Open web interface in browser |
| `status` | Show system status |
| `backup` | Trigger manual backup |
| `restore` | Restore from backup |
| `set-gitea` | Configure Gitea organization |

---

## Resource Requirements

### Minimum

- **CPU**: 8 cores
- **Memory**: 16 GB
- **Disk**: 200 GB

### Recommended

- **CPU**: 8+ cores
- **Memory**: 16+ GB
- **Disk**: 200+ GB

---

## Comparison with Other Patterns

| Feature | single-vm | gateway | k3s | tfgrid-ai-stack |
|---------|-----------|---------|-----|-----------------|
| VMs | 1 | 2+ | 3+ | 3 |
| Public IPv4 | Optional | ✅ | Optional | ✅ |
| SSL | Manual | ✅ | Via Ingress | ✅ |
| AI Integration | ❌ | ❌ | ❌ | ✅ |
| Git Hosting | ❌ | ❌ | ❌ | ✅ |
| Monitoring | ❌ | ❌ | Optional | ✅ |
| Deploy Time | 2-3 min | 5-7 min | 8-10 min | ~10 min |

---

## Troubleshooting

### Authentication Issues

```bash
# Re-authenticate with Qwen
tfgrid-compose login

# Check authentication status
tfgrid-compose whoami
```

### Project Not Starting

```bash
# Check project status
tfgrid-compose projects

# View logs
tfgrid-compose logs my-project

# Restart the project
tfgrid-compose restart my-project
```

### Network Issues

```bash
# Check WireGuard status
tfgrid-compose ssh
sudo wg show

# Check service status
systemctl status nginx
systemctl status gitea
```

---

## Full Documentation

- **[TFGrid AI Stack Guide](../guides/tfgrid-ai-stack.md)** - Complete usage guide
- **[Source Code](https://github.com/tfgrid-studio/tfgrid-ai-stack)** - Application repository
- **[Pattern Source](https://github.com/tfgrid-studio/tfgrid-compose/tree/main/patterns/tfgrid-ai-stack)** - Pattern implementation

---

## Next Steps

- **[Deploy your first project](../guides/tfgrid-ai-stack.md)** - Get started with AI development
- **[Single-VM Pattern](single-vm.md)** - Simpler deployments
- **[Gateway Pattern](gateway.md)** - Production web apps
- **[K3s Pattern](k3s.md)** - Kubernetes clusters
