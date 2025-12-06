# Context System

Learn how tfgrid-compose's context system makes managing multiple apps seamless.

---

## Overview

The **context system** allows you to work with multiple deployed apps without constantly specifying which one you're targeting. It's like setting a "current directory" for your deployed apps.

**Key Features:**

- 🎯 **Smart auto-detection** - Single app? No setup needed!
- 🔄 **Select between apps** - `tfgrid-compose select`
- 🔄 **Select between projects** - `tfgrid-compose select-project` (for apps that support it)
- 📝 **Context-aware commands** - Commands use active app and project
- 🌐 **Both modes** - Explicit or contextual

---

## How It Works

### **1. Smart Context Detection**

When you have **exactly one app** deployed, tfgrid-compose automatically uses it:

```bash
# Deploy first app
tfgrid-compose up tfgrid-ai-agent

# These just work! No switch needed
tfgrid-compose create my-project
tfgrid-compose projects
tfgrid-compose logs my-project
```

✅ **No configuration required**

### **2. Multiple Apps - Explicit Context**

With multiple apps, you need to set context:

```bash
# Deploy second app
tfgrid-compose up some-web-app

# Now you need to specify which app
tfgrid-compose select tfgrid-ai-agent

# Commands now target tfgrid-ai-agent
tfgrid-compose create my-project
```

### **3. Check Current Context**

```bash
# List all deployed apps
tfgrid-compose list

# Output:
#   * tfgrid-ai-agent (active) - 10.1.3.2
#     some-web-app - 10.1.3.5
#
# ℹ Active context: tfgrid-ai-agent
```

---

## Commands

### **select** - Change Active App

Set which app commands should target:

```bash
# Select an app
tfgrid-compose select tfgrid-ai-agent

# All subsequent commands use this app
tfgrid-compose create project1
tfgrid-compose run project1
```

**When to use:**

- You have multiple apps deployed
- You want to work with a specific app for a while

### **unselect** - Clear Active App

Clear the current app selection:

```bash
# Clear app context
tfgrid-compose unselect

# Commands now require explicit app
tfgrid-compose create project  # Error
tfgrid-compose tfgrid-ai-agent create project  # Works
```

### **select-project** - Change Active Project

For apps that manage multiple projects (like tfgrid-ai-agent):

```bash
# Interactive selection
tfgrid-compose select-project

# Direct selection
tfgrid-compose select-project my-project

# Commands now use selected project
tfgrid-compose run       # Runs my-project
tfgrid-compose logs      # Logs for my-project
```

### **unselect-project** - Clear Project Selection

```bash
# Clear project context
tfgrid-compose unselect-project

# Commands require project name
tfgrid-compose run            # Error
tfgrid-compose run my-project # Works
```

### **list** - Show All Apps

See all deployed apps and which is active:

```bash
tfgrid-compose list
```

**Output shows:**

- ✅ All deployed apps with IPs
- ⭐ Current active context (marked with `*`)
- 📊 Auto-context status (single app)

---

## Context Behavior

### **Single App Scenario**

```bash
# You deploy one app
tfgrid-compose up tfgrid-ai-agent

# NO switch needed!
tfgrid-compose create project
# → Automatically uses tfgrid-ai-agent
```

✅ **Zero friction for common case**

### **Multiple Apps Scenario**

```bash
# You have multiple apps
tfgrid-compose list
#   * tfgrid-ai-agent (active)
#     web-dashboard
#     api-backend

# Commands without context fail helpfully
tfgrid-compose create project
# → Error: Multiple apps. Use: tfgrid-compose select <app>

# Set context
tfgrid-compose select web-dashboard

# Now commands work
tfgrid-compose deploy
```

✅ **Clear errors guide you**

---

## Command Resolution Order

When you run a command, tfgrid-compose checks:

1. **Is it a generic command?** (up, down, status, logs, ssh, etc.)
   
   - If yes: Execute generic command
   
2. **Is it app-specific?** (create, run, stop for ai-agent, etc.)
   
   - Check context (auto or explicit)
   - Route to app's command script
   
3. **Explicit app in command?**

   - `tfgrid-compose tfgrid-ai-agent create` overrides context

---

## Best Practices

### **Working with One App**

```bash
# Deploy
tfgrid-compose up tfgrid-ai-agent

# Just use commands directly
tfgrid-compose create project
tfgrid-compose run project
tfgrid-compose projects
```

**No `select` needed!**

### **Working with Multiple Apps**

```bash
# Set context at start of work session
tfgrid-compose select tfgrid-ai-agent

# Work on multiple projects
tfgrid-compose create project1
tfgrid-compose create project2
tfgrid-compose run project1

# Select when changing apps
tfgrid-compose select web-dashboard
tfgrid-compose deploy
```

**One `select` per work session**

### **Quick Commands Without Selection**

```bash
# Explicit app name (no select needed)
tfgrid-compose tfgrid-ai-agent create quick-project
tfgrid-compose web-dashboard status
```

**Override context for one-off commands**

---

## Technical Details

### **Context Storage**

- **Location:** `~/.config/tfgrid-compose/context.yaml`
- **Format:** YAML with app and project context
- **Scope:** Global (all directories)

**Example context file:**
```yaml
active_app: tfgrid-ai-agent
active_project: my-project
```

### **Auto-Detection Logic**

```
1. Check deployed apps count
2. If count == 1:
   - Use that app automatically
   - No context file needed
3. If count > 1:
   - Check context file
   - If missing: Prompt to select
4. If count == 0:
   - Show help to deploy
```

---

## Examples

### **Example 1: Fresh Start**

```bash
# Deploy your first app
tfgrid-compose up tfgrid-ai-agent

# Start using it immediately
tfgrid-compose create music-website
tfgrid-compose run music-website
tfgrid-compose monitor music-website
```

### **Example 2: Multiple Apps**

```bash
# Deploy apps
tfgrid-compose up tfgrid-ai-agent
tfgrid-compose up web-dashboard

# Set context
tfgrid-compose select tfgrid-ai-agent

# Work with AI agent
tfgrid-compose create backend-api
tfgrid-compose run backend-api

# Select other app
tfgrid-compose select web-dashboard
tfgrid-compose deploy
```

### **Example 3: Mixed Mode**

```bash
# Have context set to ai-agent
tfgrid-compose select tfgrid-ai-agent

# Use contextual commands
tfgrid-compose create project1

# Quick command on other app (no selection change)
tfgrid-compose web-dashboard status

# Continue with ai-agent (context unchanged)
tfgrid-compose run project1
```

---

## See Also

- [App-Specific Commands](app-commands.md) - Commands unique to each app
- [AI Agent Guide](../guides/ai-agent.md) - Using the AI coding agent
- [CLI Reference](../cli/registry.md) - Full command reference
