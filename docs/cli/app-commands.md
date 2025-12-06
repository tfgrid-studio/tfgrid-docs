# App-Specific Commands

How apps define custom commands and how to use them.

---

## Design Philosophy

**Universal CLI, App-Specific Features**

tfgrid-compose provides a **universal interface** where:

- 🔧 **Generic commands** work for all apps (up, down, status, logs, ssh)
- 🎯 **App-specific commands** are defined by each app (create, run, stop, etc.)
- 🚀 **One CLI** to rule them all

Each app becomes a **first-class citizen** with its own command set!

---

## How It Works

### **1. Apps Define Commands in Manifest**

Apps declare their commands in `tfgrid-compose.yaml`:

```yaml
name: tfgrid-ai-agent
version: 0.3.0
description: AI-powered coding agent

# App-specific commands
commands:
  create:
    script: /opt/ai-agent/scripts/create-project.sh
    description: Create a new AI coding project
    args: "[project-name]"
    
  run:
    script: /opt/ai-agent/scripts/run-project.sh
    description: Start agent loop for a project
    args: "[project-name]"
    
  projects:
    script: /opt/ai-agent/scripts/status-projects.sh
    description: Show status of all projects
```

### **2. tfgrid-compose Routes Commands**

When you run a command:

```bash
tfgrid-compose create my-project
```

tfgrid-compose:

1. ✅ Checks if it's a generic command
2. ✅ If not, looks in active app's manifest
3. ✅ Finds the script path
4. ✅ Executes on the VM via SSH

**You get native app functionality through the universal CLI!**

---

## Generic Commands

These work for **all apps**:

| Command | Description |
|---------|-------------|
| `up` | Deploy app to ThreeFold Grid |
| `down` | Destroy deployment |
| `status` | Check app status |
| `logs` | View app logs |
| `ssh` | Connect to VM |
| `exec` | Execute arbitrary command |
| `list` | List deployed apps |
| `switch` | Change active app |
| `config` | Manage configuration settings |

**Universal operations** - Every app supports these.

---

## App-Specific Commands

### **tfgrid-ai-agent Commands**

The AI coding agent provides these commands:

```bash
# Project Management
tfgrid-compose create [project-name]     # Create new AI project
tfgrid-compose run [project-name]        # Start agent loop
tfgrid-compose stop [project-name]       # Stop agent loop
tfgrid-compose remove [project-name]     # Delete project

# Monitoring
tfgrid-compose projects                  # Show all projects
tfgrid-compose monitor [project-name]    # Watch progress
tfgrid-compose logs [project-name]       # View project logs
tfgrid-compose summary [project-name]    # Show summary

# Operations
tfgrid-compose edit [project-name]       # Edit configuration
tfgrid-compose restart [project-name]    # Restart agent
tfgrid-compose stopall                   # Stop all projects

# Setup
tfgrid-compose login                     # Configure git credentials
tfgrid-compose version                   # Show agent version
```

### **Future Apps**

Other apps will define their own commands:

**Example: web-dashboard**
```bash
tfgrid-compose deploy      # Deploy dashboard
tfgrid-compose config      # Configure settings
tfgrid-compose backup      # Backup data
```

**Example: api-backend**
```bash
tfgrid-compose migrate     # Run migrations
tfgrid-compose seed        # Seed database
tfgrid-compose scale N     # Scale to N instances
```

**Each app = Custom command set!**

---

## Command Discovery

### **See Available Commands**

```bash
# Generic help
tfgrid-compose help

# App-specific: Try a command
tfgrid-compose create
# → If not recognized, shows: "Unknown command: create"
# → Lists available app commands
```

### **Read App Manifest**

```bash
# View app's commands
cat ~/.config/tfgrid-compose/apps/tfgrid-ai-agent/tfgrid-compose.yaml

# Look for 'commands:' section
```

---

## Usage Patterns

### **Pattern 1: Interactive Projects (AI Agent)**

```bash
# Create project (interactive prompts)
tfgrid-compose create music-website
  → Prompts for duration, template, etc.

# Start the agent
tfgrid-compose run music-website
  → Agent loop begins coding

# Monitor progress
tfgrid-compose monitor music-website
  → Watch in real-time

# Check results
tfgrid-compose summary music-website
  → See what was accomplished
```

### **Pattern 2: Service Management (Web Apps)**

```bash
# Deploy service
tfgrid-compose deploy

# Check status
tfgrid-compose status

# Update configuration (use config set instead)
tfgrid-compose config set key value

# Restart
tfgrid-compose restart
```

### **Pattern 3: Data Operations (Databases)**

```bash
# Run migrations
tfgrid-compose migrate

# Backup data
tfgrid-compose backup production

# Restore data
tfgrid-compose restore backup-2024.tar.gz
```

**Each app defines its own patterns!**

---

## Command Arguments

### **Optional Arguments**

Many commands take optional project/resource names:

```bash
# Interactive selection (if no name given)
tfgrid-compose run
  → Shows list, pick one

# Direct specification
tfgrid-compose run my-project
  → Runs immediately
```

### **Positional vs Flags**

```bash
# Positional (simple)
tfgrid-compose create my-project

# With flags (advanced)
tfgrid-compose create my-project --template=web --duration=2h
```

**Apps decide their own syntax!**

---

## Creating App Commands

### **For App Developers**

**1. Write the script:**

```bash
#!/bin/bash
# /opt/myapp/scripts/my-command.sh

echo "Executing custom command..."
# Your logic here
```

**2. Add to manifest:**

```yaml
commands:
  my-command:
    script: /opt/myapp/scripts/my-command.sh
    description: Do something awesome
    args: "[optional-arg]"
```

**3. Deploy:**

```bash
tfgrid-compose up myapp
```

**4. Use:**

```bash
tfgrid-compose my-command
```

**That's it!** Your app now has custom commands.

---

## Best Practices

### **Design Your Commands**

✅ **DO:**

- Use clear, action-oriented names (`create`, `deploy`, `backup`)
- Make commands idempotent when possible
- Provide helpful error messages
- Support both interactive and non-interactive modes

❌ **DON'T:**

- Use generic names that conflict (`status`, `logs` are reserved)
- Require complex flag combinations
- Make breaking changes without version bump

### **Command Naming**

```bash
# Good (clear actions)
create, run, stop, deploy, backup, migrate

# Avoid (ambiguous)
do-stuff, process, handle, manage
```

### **Arguments**

```bash
# Good (optional, with selection)
tfgrid-compose run [project-name]
  → Interactive if omitted

# Good (clear meaning)
tfgrid-compose backup --output=/path/to/backup.tar.gz

# Avoid (too many required args)
tfgrid-compose complex command arg1 arg2 arg3 arg4
```

---

## Technical Implementation

### **Command Resolution**

```
User runs: tfgrid-compose create my-project

1. Parse command line
   → Command: "create"
   → Args: ["my-project"]

2. Check generic commands
   → Not found

3. Get active context
   → App: tfgrid-ai-agent

4. Load app manifest
   → commands.create.script: /opt/ai-agent/scripts/create-project.sh

5. Execute on VM
   → ssh root@10.1.3.2 "/opt/ai-agent/scripts/create-project.sh my-project"
```

### **Script Execution**

Commands run with:

- **SSH TTY** - Interactive prompts work
- **Arguments passed** - Via command line
- **Exit codes** - Propagated to local CLI

---

## Examples

### **Example 1: AI Agent Workflow**

```bash
# Configure credentials (one time)
tfgrid-compose login

# Create projects
tfgrid-compose create website-redesign
tfgrid-compose create api-refactor
tfgrid-compose create docs-update

# Start agents
tfgrid-compose run website-redesign
tfgrid-compose run api-refactor

# Check status
tfgrid-compose projects

# Monitor specific project
tfgrid-compose monitor website-redesign

# View results
tfgrid-compose summary website-redesign
tfgrid-compose logs website-redesign
```

### **Example 2: Multi-App Deployment**

```bash
# Deploy multiple apps
tfgrid-compose up tfgrid-ai-agent
tfgrid-compose up web-dashboard
tfgrid-compose up api-backend

# Switch context for different tasks
tfgrid-compose switch tfgrid-ai-agent
tfgrid-compose create backend-rewrite

tfgrid-compose switch api-backend
tfgrid-compose migrate

tfgrid-compose switch web-dashboard
tfgrid-compose deploy
```

---

## See Also

- [Context System](context-system.md) - Managing multiple apps
- [Node Selection](node-selection.md) - Advanced node filtering and selection
- [AI Agent Guide](../guides/ai-agent.md) - Complete AI agent reference
- [App Development](../development/submit-app.md) - Creating your own apps
