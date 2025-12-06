# TFGrid AI Stack Guide

**Complete AI-powered development environment with integrated Git hosting and live deployment.**

## Overview

The AI Stack provides a seamless, end-to-end development experience on ThreeFold Grid. Deploy a complete environment where AI generates code, automatically creates Git repositories, and serves live websites - all with a single command.

**Key Features:**

- 🤖 **AI-Powered Development** - Integrated tfgrid-ai-agent for code generation
- 🏝️ **Single-VM Deployment** - All components co-located on one secure VM
- 🌐 **Automatic Git Hosting** - Gitea repositories created automatically for every project
- 📦 **Project Management** - Create, manage, and deploy multiple projects concurrently
- 🔒 **Flexible Privacy** - Private development with optional public access
- ⚡ **One-Command Setup** - Deploy complete stack in under 10 minutes

## Quick Start

### Prerequisites

- TFGrid account with mnemonic configured
- `tfgrid-compose` installed (available via app registry)

### Quick Start

Deploy the complete AI development environment:

```bash
# Basic deployment
tfgrid-compose up tfgrid-ai-stack

# Advanced deployment with node selection
tfgrid-compose up tfgrid-ai-stack --whitelist-node=920 --whitelist-farm=FastFarm

# Deployment with resource constraints
tfgrid-compose up tfgrid-ai-stack --max-cpu-usage=80 --min-uptime-days=7

# Authenticate with Qwen AI
tfgrid-compose login

# Create your first AI project
tfgrid-compose create "modern web application"

# Monitor AI development progress
tfgrid-compose monitor web-application

# View project in Gitea web interface
tfgrid-compose projects
```

> **Important:** Always run `tfgrid-compose login` (or `t login` if you use the alias) **after each deployment or redeploy of tfgrid-ai-stack and before any `create`, `run`, or `publish` command**. Qwen must be authenticated for the AI agent to work.

**Access URLs:**

- Gitea Git Interface: `http://your-ip/git/`
- AI Projects: `http://your-ip/git/tfgrid-ai-agent/project-name/`
- API Endpoints: `http://your-ip/api/`

### Public Access (Optional)

For public access with SSL:

```bash
# Deploy with domain
tfgrid-compose up tfgrid-ai-stack --domain yourdomain.com

# Access via HTTPS
# Gitea: https://yourdomain.com/git/
# Projects: https://yourdomain.com/git/tfgrid-ai-agent/project-name/
```

## Usage

### Advanced Deployment Options

The tfgrid-ai-stack supports comprehensive filtering and deployment options for optimal node selection:

#### Node and Farm Filtering
```bash
# Whitelist preferred nodes (only use these nodes)
tfgrid-compose up tfgrid-ai-stack --whitelist-node=920,891

# Whitelist preferred farms (only use farms with these names)
tfgrid-compose up tfgrid-ai-stack --whitelist-farm=FastFarm,ReliableFarm

# Blacklist problematic nodes (avoid these specific nodes)
tfgrid-compose up tfgrid-ai-stack --blacklist-node=920,615

# Blacklist specific farms (avoid farms with these names)
tfgrid-compose up tfgrid-ai-stack --blackfarm=SlowFarm,ProblematicFarm

# Combine filters for precise control
tfgrid-compose up tfgrid-ai-stack \
  --whitelist-farm=FastFarm \
  --blacklist-node=920 \
  --max-cpu-usage=80
```

#### Resource Health Filters
```bash
# Ensure low CPU usage for better performance
tfgrid-compose up tfgrid-ai-stack --max-cpu-usage=70

# Prefer nodes with low disk usage
tfgrid-compose up tfgrid-ai-stack --max-disk-usage=60

# Ensure minimum uptime for stability
tfgrid-compose up tfgrid-ai-stack --min-uptime-days=7

# Combine multiple health criteria
tfgrid-compose up tfgrid-ai-stack \
  --max-cpu-usage=80 \
  --max-disk-usage=70 \
  --min-uptime-days=3
```

#### Specific Deployment Targets
```bash
# Deploy to specific farm
tfgrid-compose up tfgrid-ai-stack --farm-id=42

# Deploy to specific node (exact match)
tfgrid-compose up tfgrid-ai-stack --node-id=920

# Private network configuration
tfgrid-compose up tfgrid-ai-stack --network=main
```

#### Public Access Configuration
```bash
# Deploy with SSL domain
tfgrid-compose up tfgrid-ai-stack --domain=yourdomain.com

# Force redeployment with cache refresh
tfgrid-compose up tfgrid-ai-stack --force --refresh

# Interactive node selection
tfgrid-compose up tfgrid-ai-stack --interactive
```

### Project Management

#### Create a New Project
```bash
# Create project with AI (automatically creates Gitea repo)
tfgrid-compose create "modern portfolio website"

# Output:
# ✅ Project 'portfolio' created
# 📝 Repository: http://your-ip/git/tfgrid-ai-agent/portfolio/
# 🌐 Gitea Web: http://your-ip/git/tfgrid-ai-agent/portfolio/
# 🤖 AI Agent: Running (commits automatically pushed)
```

#### List Projects
```bash
tfgrid-compose projects

# Output:
# 📁 portfolio
#    Status: running
#    🌐 Gitea: http://your-ip/git/tfgrid-ai-agent/portfolio/
#    🏢 Organization: tfgrid-ai-agent
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

#### Monitor Project Logs
```bash
# Real-time monitoring of AI development
tfgrid-compose monitor web-application

# Output shows:
# 🤖 AI Agent: Generating code for web-application
# 📝 Creating repository: tfgrid-ai-agent/web-application
# 🌐 Deploying to: http://your-ip/web-application
# ✅ Project completed in 5m 32s
```
#### AI-Powered Publishing
```bash
# Publish project for web hosting (AI intelligently handles publishing)
tfgrid-compose publish mathweb

# Or interactive mode - select project from list
tfgrid-compose publish

# Output shows:
# 🤖 AI-Powered Project Publisher
# 📂 Project: mathweb
# 🏢 Organization: tfgrid-ai-agent
# 🚀 Starting AI agent publishing process...
# 🧠 AI creates intelligent nginx configuration
# 🌐 Access URLs:
#    Git:  http://10.1.3.2/git/tfgrid-ai-agent/mathweb
#    Web:  http://10.1.3.2/web/tfgrid-ai-agent/mathweb
# 🎉 AI Agent publishing process completed!
```

The AI agent intelligently:
- **Detects organization** from project Git remote URL
- **Analyzes project type** (React, Vue, Static, API)
- **Creates optimal nginx configuration** for the project
- **Sets proper permissions** for web access
- **Handles deployment URLs** automatically

**Complete Workflow:**
```bash
tfgrid-compose create "chemistry website"    # AI builds website
tfgrid-compose run                           # AI develops website  
tfgrid-compose publish                       # AI agent publishes to web hosting
# Now accessible at: http://your-ip/web/tfgrid-ai-agent/chemistry-website
```

##### Non-interactive Automation (CLI)

You can fully automate project creation, running, and publishing from the CLI
without interactive prompts. This is useful for scripts, demos, or repeated
workflows.

**Requirements:**

- Stack deployed and healthy (`tfgrid-compose up tfgrid-ai-stack ...`)
- Qwen authenticated: `tfgrid-compose login` (must be re-run after redeploys)

**Example: one-shot create → run → publish**

```bash
tfgrid-compose create \
  --project mathweb \
  --time 30m \
  --prompt "create a math website about Fourier transforms" \
  --git-name "Your Name" \
  --git-email "you@example.com" \
  --run yes \
  --publish yes \
  --non-interactive
```

This will, in order:

- Create the `mathweb` project non-interactively (no prompts)
- Start the AI agent loop for `mathweb` and wait until it finishes
  (the same point where logs show `Stop signal received, exiting loop`)
- After the loop completes and final commits are pushed to Gitea, run the
  AI-powered publish flow for `mathweb`.

**Notes:**

- `--time` controls the time budget for the AI loop (e.g. `30m`, `1h`,
  `2h30m`, `indefinite`).
- Use `--prompt` for a custom project prompt, or `--template` with one of:
  `website`, `porting`, `translation`, `editing`, `copywriting`, `other`.
- `--git-name` and `--git-email` override the git identity for this project
  only; if omitted, the global git config (or a safe default) is used.
- In non-interactive mode, `--publish yes` requires `--run yes`, so the
  publish step always runs **after** the agent has finished its loop.

##### Publish URL Scheme

When you publish a project from the AI Stack, it is always exposed using a
simple **organization / repository** layout:

- **Git:** `http://<ip-or-mycelium>/git/<org>/<project>`
- **Web:** `http://<ip-or-mycelium>/web/<org>/<project>`

Where:

- `<ip-or-mycelium>` can be a WireGuard IPv4 address (for example
  `10.1.3.2`) or a Mycelium IPv6 address.
- `<org>` is typically `tfgrid-ai-agent` by default, or another
  organization/user if you configure Gitea differently.
- `<project>` is the project name you passed to `create` / `publish`
  (for example `mathweb`).

Examples:

- `http://10.1.3.2/git/tfgrid-ai-agent/mathweb`
- `http://10.1.3.2/web/tfgrid-ai-agent/mathweb`

#### Backup Management
```bash
# Manual backup of all projects
tfgrid-compose backup

# Restore from backup
tfgrid-compose restore backup-20251031.tar.gz

# Shows backup status and progress
```

### Stack Management

#### Check Status
```bash
tfgrid-compose status tfgrid-ai-stack

# Output:
# tfgrid-ai-stack: running
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
tfgrid-compose logs tfgrid-ai-stack

# Component-specific logs
tfgrid-compose logs ai-agent
tfgrid-compose logs gitea
tfgrid-compose logs gateway
```

#### Stop/Start Stack
```bash
# Stop all components
tfgrid-compose down tfgrid-ai-stack

# Start all components
tfgrid-compose up tfgrid-ai-stack
```

## Architecture

### Single-VM Layout

```
tfgrid-ai-stack (Single VM)
├── Nginx Reverse Proxy (Port 80/443)
│   ├── /git/ → Gitea Web UI (Port 3000)
│   ├── /api/ → AI Agent API (Port 8080)
│   └── SSL/TLS termination (optional)
├── AI Agent Service
│   ├── Node.js + Qwen CLI
│   ├── Project workspaces (/home/developer/code/)
│   ├── Automatic Git integration
│   └── API endpoints (/api/)
└── Gitea Git Server
    ├── Git repositories (/var/lib/gitea/)
    ├── Web interface (/git/)
    ├── REST API
    └── Organization: tfgrid-ai-agent
```

### Key Integration Points

- **Automatic Repo Creation**: Projects get Gitea repos on creation
- **Zero-Config Git**: Credentials and remotes configured automatically
- **Auto-Push**: AI commits pushed to Gitea immediately
- **Web Interface**: All repos accessible via `/git/` URLs
- **API Integration**: Components communicate via local APIs

### Security Model

- **Single VM:** All components share the same secure environment
- **Gitea:** Web interface accessible via `/git/` with authentication
- **AI Agent:** API endpoints via `/api/` with proper access controls
- **Projects:** Git repositories with organization-based access control
- **Credentials:** API tokens stored securely, Git credentials configured automatically

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
tfgrid-compose up tfgrid-ai-stack --domain example.com
```

### Core Configuration Options

Comprehensive configuration available via `tfgrid-compose.yaml`:

```yaml
# tfgrid-compose.yaml
resources:
  cpu: 8        # All components share resources
  memory: 16384 # 16GB total
  disk: 200     # 200GB storage

# Security & Performance Settings
api_rate_limit: "100r/m"        # API rate limiting (requests per minute)
max_concurrent_projects: 10     # Maximum concurrent project creations

# Backup Configuration
backup_retention_days: 30       # Backup retention period in days
backup_schedule: "0 2 * * *"    # Backup cron schedule (daily at 2 AM)

# ThreeFold Grid Deployment
farm_id: 0                      # Preferred farm ID (0 for auto-selection)
node_id: 0                      # Specific node ID (0 for auto-selection)

# Network Configuration
private_network_cidr: "10.1.1.0/24"  # Private network CIDR

# SSL Configuration
ssl_email: your-email@example.com    # Let's Encrypt email for SSL
domain: yourdomain.com               # Domain for SSL (optional)
```

### Configuration Examples

#### High-Performance Configuration
```yaml
# For production environments
api_rate_limit: "200r/m"
max_concurrent_projects: 20
backup_retention_days: 90
backup_schedule: "0 1 * * *"  # Daily at 1 AM
```

#### Development Configuration
```yaml
# For development and testing
api_rate_limit: "50r/m"
max_concurrent_projects: 5
backup_retention_days: 7
backup_schedule: "0 3 * * 0"  # Weekly on Sunday at 3 AM
```

#### Specific Node Deployment
```yaml
# Deploy to specific farm and node
farm_id: 42
node_id: 920
private_network_cidr: "10.1.1.0/24"
```

#### Configuration via Environment Variables
```bash
# Set via export before deployment
export TFGRID_AI_STACK_API_RATE_LIMIT="150r/m"
export TFGRID_AI_STACK_MAX_CONCURRENT_PROJECTS=15
export TFGRID_AI_STACK_BACKUP_RETENTION_DAYS=45

# Deploy with custom configuration
tfgrid-compose up tfgrid-ai-stack
```

## How It Works

### Project Creation Flow

1. **AI Code Generation**  
  1.1. AI agent analyzes natural language requirements  
  1.2. Generates complete project code with tests  
  1.3. Validates code quality and functionality  

2. **Automatic Git Setup**  
  2.1. Creates Gitea repository via API in `tfgrid-ai-agent` organization  
  2.2. Configures Git credentials and remote automatically  
  2.3. Pushes initial commit to Gitea  

3. **Continuous Development**  
  3.1. AI agent runs iterative improvement loops  
  3.2. Each code change committed and pushed to Gitea  
  3.3. Real-time visibility in web interface  

4. **Web Interface Access**  
  4.1. Repository accessible at `/git/tfgrid-ai-agent/project-name/`  
  4.2. Full Git history and file browsing  
  4.3. Issue tracking and documentation  

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
tfgrid-compose logs tfgrid-ai-stack --verbose

# Retry deployment
tfgrid-compose up tfgrid-ai-stack --force
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

### Gitea Integration Issues

**Repository not created:**
```bash
# Check Gitea API token
tfgrid-compose exec tfgrid-ai-stack cat /opt/tfgrid-ai-stack/config/gitea.json

# Test API connectivity
curl -H "Authorization: token YOUR_TOKEN" http://localhost:3000/api/v1/user

# Check Gitea service status
systemctl status gitea
```

**Git push fails:**
```bash
# Check Git remote configuration
git remote -v

# Verify credentials
cat ~/.git-credentials

# Test Gitea connectivity
curl http://localhost:3000/api/v1/version

# Check repository exists
curl http://localhost:3000/api/v1/repos/tfgrid-ai-agent/project-name
```

**Web interface not accessible:**
```bash
# Check Nginx configuration
cat /etc/nginx/sites-available/ai-stack

# Test Nginx config
nginx -t

# Check service status
systemctl status nginx
systemctl status gitea
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
tfgrid-compose backup tfgrid-ai-stack

# Backup specific component
tfgrid-compose backup ai-agent
tfgrid-compose backup gitea

# Restore from backup
tfgrid-compose restore tfgrid-ai-stack backup-20251021.tar.gz
```

## Examples

### Complete AI Development Workflow
```bash
# Deploy the full stack
tfgrid-compose up tfgrid-ai-stack

# Authenticate AI
tfgrid-compose login

# Create project (AI generates code + creates Git repo)
tfgrid-compose create "e-commerce website with React"

# Monitor AI development (code pushed to Gitea automatically)
tfgrid-compose monitor e-commerce

# View results in web interface
# Gitea: http://your-ip/git/tfgrid-ai-agent/e-commerce/
# All commits visible immediately
```

### Multiple AI Projects
```bash
# Run multiple AI agents simultaneously
tfgrid-compose create "frontend dashboard"
tfgrid-compose create "backend API"
tfgrid-compose create "mobile app"

# All projects get automatic Git repos
tfgrid-compose projects
# Shows all projects with Gitea URLs
```

### Learning & Prototyping
```bash
# Safe environment for experimentation
tfgrid-compose create "machine learning prototype"
tfgrid-compose create "blockchain smart contract"
tfgrid-compose create "IoT dashboard"

# Full Git history and web interface for each
# No setup required - just describe what you want
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