# AI Agent Guide

Complete guide for deploying and using the AI coding agent on ThreeFold Grid.

---

## Overview

The **TFGrid AI Agent** is an autonomous AI coding assistant that runs on ThreeFold Grid. It can:

- 🤖 Write and edit code autonomously
- 🔄 Run iterative improvement loops
- 🧪 Test and debug automatically
- 📝 Generate documentation
- 🔧 Refactor and optimize code
- ⏰ Work within time constraints

**Deployment time:** 2-3 minutes  
**Cost:** Pay-as-you-go on ThreeFold Grid  
**Access:** Native commands via `tfgrid-compose`

**Version:** 0.3.0 (Pre-1.0)

---

## Quick Start

### 1. Deploy the AI Agent

```bash
# Deploy to ThreeFold Grid
tfgrid-compose up tfgrid-ai-agent
```

**What happens:**

1. ✅ Creates Ubuntu 24.04 VM (4 CPU, 8GB RAM, 100GB disk)
2. ✅ Configures WireGuard networking  
3. ✅ Installs Node.js 20.x, Qwen CLI
4. ✅ Prompts for git credentials (one time)
5. ✅ Sets up workspace at `/home/developer/code`
6. ✅ Runs health checks

**Duration:** ~2-3 minutes

**Output:**
```
✅ 🎉 Deployment complete!
ℹ App: tfgrid-ai-agent v0.3.0
ℹ Pattern: single-vm v1.0.0
ℹ Next steps:
  • Check status: tfgrid-compose status tfgrid-ai-agent
  • View logs: tfgrid-compose logs tfgrid-ai-agent
  • Connect: tfgrid-compose ssh tfgrid-ai-agent
```

### 2. Configure Git Identity

Configure your git identity for commit attribution:

```bash
tfgrid-compose login
```

The login flow collects:

- ThreeFold mnemonic
- GitHub token (optional)
- Gitea credentials (optional)
- Git name (for commit attribution)
- Git email (for commit attribution)

Git identity is automatically deployed to all VMs and used for all project commits.

### 3. Create Your First Project

```bash
tfgrid-compose create my-music-website
```

Interactive prompts:

1. **Project name** (if not specified)
2. **Duration** (30m, 1h, 2h, indefinite)
3. **Git credentials** (if not configured)
4. **Prompt type** (Custom or Generic template)
5. **Your coding prompt**
6. **Start now?** (y/N)

**Example session:**
```
🚀 AI-Agent Project Creator
==============================

Enter project name: music-website

⏱️  How long should the AI agent run?
Examples: 30m, 1h, 2h30m, indefinite

Enter duration: 1h

🔧 Let's set up your git credentials (used for all projects)

Enter your name (for git commits): John Doe
Enter your email (for git commits): john@example.com

✅ Git configured globally

📝 Choose prompt type:
1) Custom prompt (paste your own)
2) Generic template (select from options)

Select (1-2) [2]: 1

📋 Enter your custom prompt (press Ctrl+D when done):
Create a beautiful music website showcasing John Lennon 
and Mozart with lyrics and biography

✅ Custom prompt configured

✅ Project 'music-website' created successfully!

🚀 Do you want to start the AI agent now?
Start now? (y/N): y

Starting AI agent for project 'music-website'...

✅ AI agent loop started with PID: 12345
🛑 To stop the loop, run: tfgrid-compose stop music-website
```

### 4. Monitor Your Project

```bash
# Check all projects
tfgrid-compose projects

# Monitor specific project in real-time
tfgrid-compose monitor music-website

# View project summary
tfgrid-compose summary music-website

# View logs
tfgrid-compose logs music-website
```

### 5. Manage Projects

```bash
# Run a stopped project
tfgrid-compose run music-website

# Stop a running project
tfgrid-compose stop music-website

# Restart project
tfgrid-compose restart music-website

# Stop all projects
tfgrid-compose stopall

# Remove a project
tfgrid-compose remove music-website

# Edit project configuration
tfgrid-compose edit music-website
```

---

## Complete Command Reference

### All AI Agent Commands

| Command | Description | Arguments | Context |
|---------|-------------|-----------|----------|
| `create` | Create new AI project | `[project-name]` | - |
| `select-project` | Select active project | `[project-name]` | Sets context |
| `unselect-project` | Clear project selection | None | Clears context |
| `run` | Start agent loop | `[project-name]` | Uses context |
| `stop` | Stop agent loop | `[project-name]` | Uses context |
| `restart` | Restart agent | `[project-name]` | Uses context |
| `projects` | Show all projects | None | - |
| `monitor` | Watch progress live | `[project-name]` | Uses context |
| `logs` | View project logs | `[project-name]` | Uses context |
| `summary` | Show project summary | `[project-name]` | Uses context |
| `edit` | Edit configuration | `[project-name]` | Uses context |
| `remove` | Delete project | `[project-name]` | Uses context |
| `stopall` | Stop all projects | None | - |
| `login` | Configure credentials | None | - |
| `update-git-config` | Update git config on VM | None | - |
| `whoami` | Check authentication | None | - |

**Note:** Arguments in `[]` are optional. If omitted, interactive selection is shown.

```bash
# Short form (context-aware)
tfgrid-compose create my-project
tfgrid-compose run my-project

# Explicit form (no context needed)
tfgrid-compose tfgrid-ai-agent create my-project
tfgrid-compose tfgrid-ai-agent run my-project
```

**Context is automatic with single app!**

---

## Project Selection & Context

The AI agent supports project-level context for streamlined command usage.

### Two-Level Context System

```
App Context (tfgrid-ai-agent)
  └── Project Context (my-project)
```

### Select Active Project

```bash
# Interactive selection (shows all projects with status)
tfgrid-compose select-project

# Direct selection
tfgrid-compose select-project my-project
```

### Context-Aware Commands

Once a project is selected, commands use it automatically:

```bash
# Select project once
tfgrid-compose select-project frontend-app

# All subsequent commands use selected project
tfgrid-compose run        # Runs frontend-app
tfgrid-compose logs       # Logs for frontend-app
tfgrid-compose monitor    # Monitors frontend-app
tfgrid-compose stop       # Stops frontend-app
```

### Override Context

Provide project name to override selection:

```bash
# Context is set to frontend-app
tfgrid-compose select-project frontend-app

# But you can still run commands on other projects
tfgrid-compose run backend-api     # Runs backend-api
tfgrid-compose logs mobile-app     # Logs for mobile-app
```

### Clear Selection

```bash
# Clear project context
tfgrid-compose unselect-project

# Commands now require project name
tfgrid-compose run          # Error: No project specified
tfgrid-compose run my-app   # Works
```

---

## Workflows

### Workflow 1: Simple Web App

```bash
# 1. Deploy
tfgrid-compose up tfgrid-ai-agent

# 2. Configure git (first time only)
tfgrid-compose login

# 3. Create project (interactive)
tfgrid-compose create simple-blog
# Prompts for:
# - Duration: 1h
# - Prompt: "Create a simple blog with React and Node.js"
# - Start now: y

# 4. Monitor progress
tfgrid-compose monitor simple-blog

# 5. Check all projects
tfgrid-compose projects

# 6. View results
tfgrid-compose summary simple-blog
tfgrid-compose ssh
cd /home/developer/code/simple-blog
ls -la
cat README.md

# 7. Stop project
tfgrid-compose stop simple-blog

# 8. Cleanup (if done)
tfgrid-compose down
```

### Workflow 2: Code Refactoring

```bash
# 1. Create refactoring project
tfgrid-compose create refactor-legacy
# Prompt: "Refactor this legacy code to TypeScript with best practices"

# 2. Upload existing code
tfgrid-compose ssh
cd /home/developer/code/refactor-legacy
# Copy your files here
git add .
git commit -m "Initial code to refactor"
exit

# 3. Run agent
tfgrid-compose run refactor-legacy

# 4. Watch it work
tfgrid-compose monitor refactor-legacy

# 5. Check progress
tfgrid-compose summary refactor-legacy

# 6. Review and iterate
tfgrid-compose logs refactor-legacy
tfgrid-compose edit refactor-legacy  # Adjust if needed
tfgrid-compose restart refactor-legacy
```

### Workflow 3: Multiple Projects (Concurrent Execution)

```bash
# Create several projects
tfgrid-compose create frontend-app
tfgrid-compose create backend-api
tfgrid-compose create mobile-app

# Run them all - executes concurrently via systemd
tfgrid-compose run frontend-app
tfgrid-compose run backend-api
tfgrid-compose run mobile-app

# All three projects run simultaneously
# Each has isolated resources: 2GB memory, 150% CPU quota
# Auto-restart on failure with 10s delay

# Check status of all running projects
tfgrid-compose projects

# Monitor specific one
tfgrid-compose monitor frontend-app

# Stop all when done
tfgrid-compose stopall
```

**New Feature:** Projects execute in parallel using systemd template services (`tfgrid-ai-project@{name}.service`). Each project runs in an isolated service instance with dedicated resource limits (2GB memory, 150% CPU quota) and automatic failure recovery. You can now run unlimited concurrent AI agents simultaneously.

---

## Best Practices

### 1. Clear Prompts

**Good:**
```
"Create a REST API with Express.js and PostgreSQL for a todo app with authentication"
"Refactor this code to use async/await and add error handling"
"Generate API documentation with examples for all endpoints"
```

**Bad:**
```
"Make it better" (too vague)
"Build everything" (too broad)
"Fix bugs" (which bugs?)
```

### 2. Time Constraints

```bash
# Short tasks
tfgrid-compose create quick-fix
# Duration: 30m

# Medium projects
tfgrid-compose create web-dashboard  
# Duration: 2h

# Long-running projects
tfgrid-compose create full-refactor
# Duration: indefinite
```

### 3. Monitor Progress

```bash
# Always monitor long-running tasks
tfgrid-compose run big-project &
tfgrid-compose monitor big-project

# Check all projects regularly
tfgrid-compose projects
```

### 4. Resource Management

```bash
# Stop when not needed
tfgrid-compose stopall

# Remove completed projects
tfgrid-compose remove old-project

# Destroy VM when done
tfgrid-compose down
```

---

##Configuration

### Environment Variables

Set these on your local machine before deployment:

```bash
# Required
export TF_VAR_mnemonic=$(cat ~/.config/threefold/mnemonic)

# Optional
export QWEN_API_KEY='your-api-key'        # Qwen API key
export GITHUB_TOKEN='your-token'           # GitHub personal token
export GITEA_URL='https://gitea.example'   # Gitea URL
export GITEA_TOKEN='your-token'            # Gitea personal token
```

### Custom Resources

Edit `tfgrid-ai-agent/tfgrid-compose.yaml`:

```yaml
resources:
  cpu:
    minimum: 2
    recommended: 4     # Change this
  memory:
    minimum: 4096
    recommended: 8192  # Change this
  disk:
    minimum: 50
    recommended: 100   # Change this
```

Then redeploy:
```bash
tfgrid-compose down && tfgrid-compose up tfgrid-ai-agent
```

---

## Git Integration

### Setup GitHub

```bash
# 1. Show SSH key (from VM)
tfgrid-compose ssh
cat ~/.ssh/id_rsa.pub

# 2. Add to GitHub
# Copy the key and add it to https://github.com/settings/keys

# 3. Setup git remote in project
tfgrid-compose ssh
cd /home/developer/code/my-project
git remote add origin git@github.com:user/repo.git
git push -u origin main
```

### Setup Gitea

```bash
# 1. Show SSH key (from VM)
tfgrid-compose ssh
cat ~/.ssh/id_rsa.pub

# 2. Add to your Gitea instance
# Go to Gitea → Settings → SSH Keys

# 3. Setup git remote in project
cd /home/developer/code/my-project
git remote add origin git@git.example.com:user/repo.git
git push -u origin main
```

### Manual Git Configuration

```bash
# SSH into VM
tfgrid-compose ssh

# Configure git
cd /home/developer/code/my-project
git remote add origin git@github.com:user/repo.git
git push -u origin main
```

---

## Monitoring & Logs

### Real-time Monitoring

```bash
# Watch agent progress
tfgrid-compose monitor my-project

# View live logs
tfgrid-compose logs my-project

# View project summary
tfgrid-compose summary my-project
```

### SSH Access

```bash
# SSH into VM
tfgrid-compose ssh

# Check running processes
ps aux | grep ai-agent

# View project files
ls -la /home/developer/code/

# Check system logs (per-project systemd services)
journalctl -u tfgrid-ai-project@my-project.service -f
```

### Status Overview

```bash
# All projects status
tfgrid-compose projects

# Deployment status
tfgrid-compose status

# Per-project service status on VM
tfgrid-compose ssh
systemctl status tfgrid-ai-project@my-project.service
systemctl list-units 'tfgrid-ai-project@*'  # List all project services
```

---

## Troubleshooting

### Agent Won't Start

```bash
# Check logs
tfgrid-compose logs my-project

# Check service status (per-project)
tfgrid-compose ssh
systemctl status tfgrid-ai-project@my-project.service
journalctl -u tfgrid-ai-project@my-project.service -n 50

# Restart service if needed
systemctl restart tfgrid-ai-project@my-project.service
```

### SSH Connection Issues

```bash
# Check WireGuard
sudo wg show

# Test connectivity (get IP from status)
tfgrid-compose status
ping -c 3 10.1.3.2  # Use your actual IP

# Reconnect if needed
tfgrid-compose ssh
```

### Out of Disk Space

```bash
# SSH in and check
tfgrid-compose ssh
df -h

# Clean up old projects
tfgrid-compose remove old-project-name

# Or increase disk size (requires redeployment)
# Edit app's tfgrid-compose.yaml → resources.disk
tfgrid-compose down
tfgrid-compose up tfgrid-ai-agent
```

### Performance Issues

```bash
# Check resource usage
tfgrid-compose ssh
htop

# Consider upgrading resources
# Edit app's tfgrid-compose.yaml → resources (cpu/memory)
tfgrid-compose down
tfgrid-compose up tfgrid-ai-agent
```

---

## Best Practices

### 1. Project Organization

```bash
# Use descriptive names
ai-agent create webapp-dashboard  # Good
ai-agent create test1             # Bad

# One project per goal
ai-agent create frontend-app
ai-agent create backend-api
# Not: ai-agent create fullstack-everything
```

### 2. Prompts

**Good prompts:**

- "Create a REST API with Express.js and PostgreSQL for a todo app"
- "Refactor this code to use async/await instead of callbacks"
- "Add comprehensive error handling and logging"

**Bad prompts:**

- "Make it better" (too vague)
- "Build everything" (too broad)
- "Fix bugs" (which bugs?)

### 3. Monitoring

```bash
# Always monitor long-running tasks
tfgrid-compose run big-project &
tfgrid-compose monitor big-project
```

### 4. Git Workflow

```bash
# Agent auto-commits to local git
# Review before pushing
tfgrid-compose ssh
cd /home/developer/code/my-project
git log
git diff
git push origin main
```

### 5. Resource Management

```bash
# Stop projects when not needed
tfgrid-compose stopall

# Remove completed projects
tfgrid-compose remove old-project

# Destroy VM when not in use
tfgrid-compose down
```

---

## Advanced Usage

### Multiple Projects

```bash
# Create and run multiple projects
tfgrid-compose create frontend
tfgrid-compose create backend
tfgrid-compose create docs

tfgrid-compose run frontend &
tfgrid-compose run backend &
tfgrid-compose run docs &

# Monitor all
tfgrid-compose projects
```

### Custom Automation Scripts

```bash
# Create local automation script
cat > ~/ai-agent-batch.sh << 'EOF'
#!/bin/bash
for project in "$@"; do
  tfgrid-compose create "$project"
  tfgrid-compose run "$project" &
done
tfgrid-compose projects
EOF

chmod +x ~/ai-agent-batch.sh

# Use it
~/ai-agent-batch.sh frontend backend docs
```

### Scheduled Runs

```bash
# SSH into VM and setup cron
tfgrid-compose ssh
crontab -e

# Add scheduled runs (runs on VM)
0 2 * * * cd /opt/ai-agent && ./scripts/run-project.sh nightly-refactor
0 9 * * * cd /opt/ai-agent && ./scripts/run-project.sh morning-docs
```

---

## FAQ

**Q: Can I run multiple agents simultaneously?**
A: Yes! Projects run concurrently using systemd template services. Each project has dedicated resources (2GB memory, 150% CPU quota) and automatic failure recovery. You can run unlimited concurrent AI agents, each working on different projects simultaneously. Use `tfgrid-compose projects` to view all running projects.

**Q: How much does it cost?**  
A: Depends on ThreeFold Grid pricing. Typically $10-30/month for a 4CPU/8GB VM.

**Q: Can I access from multiple machines?**  
A: Yes, deploy once and use `tfgrid-compose exec` from any machine with access.

**Q: What if my local machine disconnects?**  
A: Agent keeps running on the VM. Reconnect anytime with `tfgrid-compose ssh`.

**Q: Can I use my own AI API keys?**  
A: Yes, set `QWEN_API_KEY` before deployment or SSH in and configure.

**Q: How do I backup projects?**  
A: SSH in and copy `/home/developer/code/` or use git push to remote.

**Q: Can I upgrade the agent version?**  
A: Yes, `tfgrid-compose down` then deploy updated app from registry.

---

## Next Steps

- **Try it:** Deploy and create your first project
- **Explore:** Test different prompts and workflows
- **Share:** Push your projects to GitHub/Gitea
- **Scale:** Deploy multiple instances for different teams

**Need help?** 

- Check logs: `tfgrid-compose logs <project>`
- SSH debug: `tfgrid-compose ssh`
- Community: https://forum.threefold.io

---

**Ready to code with AI on ThreeFold Grid!** 🚀
