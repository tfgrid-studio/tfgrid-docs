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
ℹ App: tfgrid-ai-agent v0.9.0
ℹ Pattern: single-vm v0.9.0
ℹ Next steps:
  • Check status: tfgrid-compose status tfgrid-ai-agent
  • View logs: tfgrid-compose logs tfgrid-ai-agent
  • Connect: tfgrid-compose ssh tfgrid-ai-agent
```

### 2. Configure Git Credentials (Optional)

If not set during deployment:

```bash
tfgrid-compose login
```

Prompts for:

- Your name (for git commits)
- Your email

**Set once, used for all projects!**

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

| Command | Description | Arguments |
|---------|-------------|-----------|
| `create` | Create new AI project | `[project-name]` |
| `run` | Start agent loop | `[project-name]` |
| `stop` | Stop agent loop | `[project-name]` |
| `restart` | Restart agent | `[project-name]` |
| `projects` | Show all projects | None |
| `monitor` | Watch progress live | `[project-name]` |
| `logs` | View project logs | `[project-name]` |
| `summary` | Show project summary | `[project-name]` |
| `edit` | Edit configuration | `[project-name]` |
| `remove` | Delete project | `[project-name]` |
| `stopall` | Stop all projects | None |
| `login` | Configure git credentials | None |
| `version` | Show agent version | None |

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

### Workflow 3: Multiple Projects

```bash
# Create several projects
tfgrid-compose create frontend-app
tfgrid-compose create backend-api
tfgrid-compose create mobile-app

# Run them all
tfgrid-compose run frontend-app
tfgrid-compose run backend-api
tfgrid-compose run mobile-app

# Check status of all
tfgrid-compose projects

# Monitor specific one
tfgrid-compose monitor frontend-app

# Stop all when done
tfgrid-compose stopall
```

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
make down && make up
```

---

## Git Integration

### Setup GitHub

```bash
# 1. Show SSH key
make exec CMD='ai-agent git-show-key'

# 2. Add to GitHub
# Copy the key and add it to https://github.com/settings/keys

# 3. Setup project with GitHub
make exec CMD='ai-agent git-setup my-project github'
```

### Setup Gitea

```bash
# 1. Show SSH key
make exec CMD='ai-agent git-show-key'

# 2. Add to your Gitea instance
# Go to Gitea → Settings → SSH Keys

# 3. Setup project with Gitea
make exec CMD='ai-agent git-setup my-project gitea'
```

### Manual Git Configuration

```bash
# SSH into VM
make ssh

# Configure git
cd /opt/ai-agent/projects/my-project
git remote add origin git@github.com:user/repo.git
git push -u origin main
```

---

## Monitoring & Logs

### Real-time Monitoring

```bash
# Watch agent progress
make exec CMD='ai-agent monitor my-project'

# View live logs
make exec CMD='ai-agent logs my-project'
```

### SSH Access

```bash
# SSH into VM
make ssh

# Check running processes
ps aux | grep ai-agent

# View project files
ls -la /opt/ai-agent/projects/

# Check system logs
journalctl -u ai-agent -f
```

### Status Overview

```bash
# All projects status
make exec CMD='ai-agent status'

# Deployment status
make status

# VM addresses
make address
```

---

## Troubleshooting

### Agent Won't Start

```bash
# Check logs
make exec CMD='ai-agent logs <project>'

# Verify Qwen login
make exec CMD='qwen whoami'

# Re-login if needed
make exec CMD='qwen login'
```

### SSH Connection Issues

```bash
# Check WireGuard
sudo wg show wg-ai-agent

# Restart WireGuard if needed
make wg

# Test connectivity
ping -c 3 $(cat .tfgrid-compose/state.yaml | grep vm_ip | awk '{print $2}')
```

### Out of Disk Space

```bash
# SSH in and check
make ssh
df -h

# Clean up old projects
ai-agent remove old-project-name

# Or increase disk size (requires redeployment)
# Edit tfgrid-compose.yaml → resources.disk
make down && make up
```

### Performance Issues

```bash
# Check resource usage
make ssh
htop

# Consider upgrading resources
# Edit tfgrid-compose.yaml
# Increase CPU/memory
make down && make up
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
make exec CMD='ai-agent run big-project' &
make exec CMD='ai-agent monitor big-project'
```

### 4. Git Workflow

```bash
# Setup git first
make exec CMD='ai-agent git-setup project github'

# Agent will auto-commit
# Review before pushing
make ssh
cd /opt/ai-agent/projects/project
git log
git diff
git push
```

### 5. Resource Management

```bash
# Stop projects when not needed
make exec CMD='ai-agent stopall'

# Remove completed projects
make exec CMD='ai-agent remove old-project'

# Destroy VM when not in use
make down
```

---

## Advanced Usage

### Multiple Projects

```bash
# Create and run multiple projects
make exec CMD='ai-agent create frontend'
make exec CMD='ai-agent create backend'
make exec CMD='ai-agent create docs'

make exec CMD='ai-agent run frontend' &
make exec CMD='ai-agent run backend' &
make exec CMD='ai-agent run docs' &

# Monitor all
make exec CMD='ai-agent status'
```

### Custom Agent Scripts

```bash
# SSH in
make ssh

# Create custom script
cat > /opt/ai-agent/custom-agent.sh << 'EOF'
#!/bin/bash
ai-agent create "$1"
ai-agent run "$1" &
ai-agent monitor "$1"
EOF

chmod +x /opt/ai-agent/custom-agent.sh

# Use it
/opt/ai-agent/custom-agent.sh my-new-project
```

### Scheduled Runs

```bash
# SSH in and setup cron
make ssh
crontab -e

# Add scheduled agent runs
0 2 * * * ai-agent run nightly-refactor
0 9 * * * ai-agent run morning-docs
```

---

## FAQ

**Q: Can I run multiple agents simultaneously?**  
A: Yes! Each project runs independently. Use `ai-agent status` to see all.

**Q: How much does it cost?**  
A: Depends on ThreeFold Grid pricing. Typically $10-30/month for a 4CPU/8GB VM.

**Q: Can I access from multiple machines?**  
A: Yes, deploy once and use `tfgrid-compose exec` from any machine with access.

**Q: What if my local machine disconnects?**  
A: Agent keeps running on the VM. Reconnect anytime with `make exec`.

**Q: Can I use my own AI API keys?**  
A: Yes, set `QWEN_API_KEY` before deployment or SSH in and configure.

**Q: How do I backup projects?**  
A: SSH in and copy `/opt/ai-agent/projects/` or use git push.

**Q: Can I upgrade the agent version?**  
A: Yes, `make down` then `make up` with updated tfgrid-ai-agent repo.

---

## Next Steps

- **Try it:** Deploy and create your first project
- **Explore:** Test different prompts and workflows
- **Share:** Push your projects to GitHub/Gitea
- **Scale:** Deploy multiple instances for different teams

**Need help?** 
- Check logs: `make exec CMD='ai-agent logs <project>'`
- SSH debug: `make ssh`
- Community: https://forum.threefold.io

---

**Ready to code with AI on ThreeFold Grid!** 🚀
