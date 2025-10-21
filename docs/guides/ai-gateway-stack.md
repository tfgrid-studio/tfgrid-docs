# AI Gateway Stack Guide

**Complete AI-powered development environment with integrated Git hosting and live deployment.**

## Overview

The AI Gateway Stack provides a seamless, end-to-end development experience on ThreeFold Grid. Deploy a complete environment where AI generates code, stores it in Git repositories, and serves live websites - all with a single command.

**Key Features:**

- 🤖 **AI-Powered Development** - Integrated tfgrid-ai-agent for code generation
- 🏝️ **Isolated Environment** - Secure, private VMs with no external access by default
- 🌐 **Optional Public Access** - Gateway pattern for SSL-enabled public websites and Git UI
- 📦 **Project Management** - Create, manage, and deploy multiple projects concurrently
- 🔒 **Flexible Privacy** - Private-only or public-facing deployments
- ⚡ **One-Command Setup** - Deploy complete stack in under 10 minutes

## Quick Start

### Prerequisites

- TFGrid account with mnemonic configured
- `tfgrid-compose` installed (available via app registry)

### Option 1: Private Development Environment

Deploy for personal development with internal access only:

```bash
# Search for the stack in registry
tfgrid-compose search ai-gateway-stack

# Deploy private stack
tfgrid-compose up ai-gateway-stack

# Get access information
tfgrid-compose launch
```

**Access:**

- AI Agent: `tfgrid-compose ssh ai-agent` (internal VM)
- Gitea: `http://10.x.x.x:3000` (internal IP via WireGuard)

### Option 2: Public Development Environment

Deploy with public access for collaboration and live websites:

```bash
# Deploy with custom domain
tfgrid-compose up ai-gateway-stack --domain example.com

# Initialize admin access
tfgrid-compose init
```

**Access:**

- AI Agent: `tfgrid-compose ssh ai-agent` (internal only)
- Gitea: `https://example.com/gitea` (public SSL)
- Projects: `https://example.com/project-name` (public SSL)

## Usage

### Project Management

#### Create a New Project
```bash
# Select the AI agent app
tfgrid-compose select ai-agent

# Create project with AI
tfgrid-compose create "modern portfolio website"

# Output:
# ✅ Project 'portfolio' created
# 📝 Repository: https://example.com/gitea/repos/portfolio
# 🌐 Live site: https://example.com/portfolio
# 🤖 AI Agent: Ready for development
```

#### List Projects
```bash
tfgrid-compose projects

# Output:
# Projects:
# - portfolio (https://example.com/portfolio) → https://example.com/gitea/repos/portfolio
# - blog (https://example.com/blog) → https://example.com/gitea/repos/blog
```

#### Run AI Development Loop
```bash
# Continue development on existing project
tfgrid-compose run portfolio

# Monitor AI agent progress
tfgrid-compose monitor portfolio
```

#### Update Existing Project
```bash
# Modify project requirements
tfgrid-compose update portfolio "add contact form and dark mode"
```

#### Delete Project
```bash
tfgrid-compose delete portfolio
# Removes from gateway, deletes Gitea repo, cleans up AI workspace
```

### Stack Management

#### Check Status
```bash
tfgrid-compose status ai-gateway-stack

# Output:
# ai-gateway-stack: running
# ├── gateway: running (example.com)
# ├── ai-agent: running (10.1.2.3)
# └── gitea: running (10.1.3.2)
```

#### Access Components
```bash
# SSH to AI agent VM
tfgrid-compose ssh ai-agent

# SSH to Gitea VM
tfgrid-compose ssh gitea

# SSH to gateway VM
tfgrid-compose ssh gateway
```

#### View Logs
```bash
# Stack-wide logs
tfgrid-compose logs ai-gateway-stack

# Component-specific logs
tfgrid-compose logs ai-agent
tfgrid-compose logs gitea
tfgrid-compose logs gateway
```

#### Stop/Start Stack
```bash
# Stop all components
tfgrid-compose down ai-gateway-stack

# Start all components
tfgrid-compose up ai-gateway-stack
```

## Architecture

### VM Layout

```
ai-gateway-stack/
├── Gateway VM (Public IPv4 + SSL)
│   ├── Nginx reverse proxy
│   ├── SSL certificates (Let's Encrypt)
│   ├── Dynamic route management
│   └── Static file serving (/var/www/)
├── AI Agent VM (Private networking)
│   ├── Node.js + Qwen CLI
│   ├── Project workspaces
│   ├── Git integration
│   └── API clients
└── Gitea VM (Private networking)
    ├── Git repositories
    ├── Web UI (port 3000)
    ├── REST API
    └── User management
```

### Networking

**Private Mode:**

- All VMs on WireGuard/Mycelium private network
- No external IPv4 allocation
- Access via `tfgrid-compose ssh` only

**Public Mode:**

- Gateway VM gets public IPv4
- SSL/TLS for all public access
- Gitea and projects proxied through gateway
- AI agent remains private

### Security Model

- **AI Agent:** Isolated VM, no external access
- **Gitea:** Private by default, optional public proxy
- **Projects:** Served via gateway with SSL
- **APIs:** Token-based authentication between components
- **Data:** All user data on private VMs

## Configuration

### Environment Variables

Set before deployment:

```bash
# Gitea configuration
export GITEA_ADMIN_USER=myadmin
export GITEA_ADMIN_PASSWORD=securepass123
export GITEA_ADMIN_EMAIL=admin@example.com

# AI Agent configuration
export QWEN_API_KEY=your_qwen_key
export GIT_USER_NAME="Your Name"
export GIT_USER_EMAIL="you@example.com"

# Deploy
tfgrid-compose up ai-gateway-stack --domain example.com
```

### Resource Allocation

Default resources (customizable in `tfgrid-compose.yaml`):

```yaml
resources:
  gateway:
    cpu: 2
    memory: 4096  # 4GB
    disk: 50      # 50GB
  ai-agent:
    cpu: 4
    memory: 8192  # 8GB
    disk: 100     # 100GB
  gitea:
    cpu: 2
    memory: 4096  # 4GB
    disk: 100     # 100GB
```

## How It Works

### Project Creation Flow

1. **AI Code Generation**

   - AI agent analyzes requirements
   - Generates complete project code
   - Tests and validates locally

2. **Git Repository Setup**
   
   - Creates repository in Gitea via API
   - Initializes with generated code
   - Sets up proper Git configuration

3. **Live Deployment**

   - Copies code to gateway `/var/www/project-name/`
   - Adds Nginx location block dynamically
   - Reloads configuration without downtime

1. **Access Provisioning**
   
   - Returns repository URL for code access
   - Returns live site URL for viewing
   - Updates project registry

### Concurrent Projects

- Multiple projects can run simultaneously
- Each project gets isolated workspace
- Resources shared efficiently across VMs
- Independent deployment and management

## Security Considerations

### Private Mode (Recommended for Development)
- No external exposure
- All access via secure WireGuard tunnels
- Perfect for personal projects and testing

### Public Mode (For Collaboration)

- SSL/TLS encryption for all public access
- Gitea authentication required for repository access
- Projects served over HTTPS
- Gateway VM is the only public-facing component

### Best Practices

- Change default Gitea credentials immediately
- Use strong passwords and API tokens
- Enable 2FA on Gitea accounts
- Regular backups of project data
- Monitor resource usage and costs

## Troubleshooting

### Deployment Issues

**Stack deployment fails:**
```bash
# Check prerequisites
tfgrid-compose doctor

# View detailed logs
tfgrid-compose logs ai-gateway-stack --verbose

# Retry deployment
tfgrid-compose up ai-gateway-stack --force
```

**Component not accessible:**
```bash
# Check component status
tfgrid-compose status ai-agent

# Restart component
tfgrid-compose restart ai-agent

# Check networking
tfgrid-compose exec ai-agent ping gitea
```

### Project Creation Issues

**AI generation fails:**
```bash
# Check AI agent logs
tfgrid-compose logs ai-agent

# Verify Qwen API key
tfgrid-compose exec ai-agent env | grep QWEN

# Restart AI agent
tfgrid-compose restart ai-agent
```

**Git push fails:**
```bash
# Check Gitea connectivity
tfgrid-compose exec ai-agent curl http://gitea:3000

# Verify API token
tfgrid-compose exec ai-agent cat ~/.gitea_token

# Check repository permissions
tfgrid-compose ssh gitea
sudo -u git gitea admin user list
```

### Public Access Issues

**SSL certificate fails:**
```bash
# Check domain DNS
nslookup example.com

# Verify gateway configuration
tfgrid-compose ssh gateway
cat /etc/nginx/sites-available/default

# Renew certificates
tfgrid-compose exec gateway certbot renew
```

**Gitea not accessible:**
```bash
# Check gateway proxy configuration
tfgrid-compose ssh gateway
cat /etc/nginx/conf.d/gitea.conf

# Test local Gitea access
tfgrid-compose exec gitea curl localhost:3000

# Reload nginx
tfgrid-compose exec gateway nginx -t && nginx -s reload
```

## Advanced Usage

### Custom Project Templates

Create project templates for consistent development:

```bash
# Create template directory
tfgrid-compose ssh ai-agent
mkdir -p /opt/ai-agent/templates/react-app

# Add template files
# ... customize template ...

# Use template
tfgrid-compose create "React dashboard" --template react-app
```

### API Integration

Access stack APIs for automation:

```bash
# Gateway route management
curl -H "Authorization: Bearer $GATEWAY_TOKEN" \
  https://example.com/api/routes

# Gitea repository API
curl -H "Authorization: token $GITEA_TOKEN" \
  https://example.com/gitea/api/v1/user/repos

# AI agent project API
tfgrid-compose exec ai-agent curl localhost:8080/api/projects
```

### Backup and Restore

```bash
# Backup entire stack
tfgrid-compose backup ai-gateway-stack

# Backup specific component
tfgrid-compose backup ai-agent
tfgrid-compose backup gitea

# Restore from backup
tfgrid-compose restore ai-gateway-stack backup-20251021.tar.gz
```

## Examples

### Personal Blog
```bash
tfgrid-compose up ai-gateway-stack --domain myblog.com
tfgrid-compose create "personal blog with markdown support"
# Access: https://myblog.com/blog and https://myblog.com/gitea
```

### Team Development Platform
```bash
tfgrid-compose up ai-gateway-stack --domain dev.team.com
tfgrid-compose create "project management dashboard"
tfgrid-compose create "API backend"
tfgrid-compose create "mobile app frontend"
# Multiple projects accessible via sub-paths
```

### Learning Environment
```bash
tfgrid-compose up ai-gateway-stack  # Private mode
tfgrid-compose create "React tutorial project"
tfgrid-compose create "Node.js API example"
# Safe learning environment with full Git history
```

## Related Documentation

- [TFGrid Compose CLI Reference](../reference/cli.md)
- [AI Agent Guide](../applications/tfgrid-ai-agent/)
- [Gitea Integration Guide](gitea.md)
- [Gateway Pattern Documentation](../patterns/gateway/)
- [Security Best Practices](../guides/security.md)

## Support

- **Documentation:** [docs.tfgrid.studio](https://docs.tfgrid.studio)
- **Issues:** [GitHub Issues](https://github.com/tfgrid-studio/tfgrid-compose/issues)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- **Community:** [Discord/Forum links]

---

**AI Gateway Stack v0.12.0** - Complete AI-powered development platform on ThreeFold Grid.