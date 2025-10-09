# TFGrid Studio Documentation

**Complete development platform for ThreeFold Grid**

Build, deploy, and scale decentralized applications with `tfgrid-compose` CLI and integrated tools.

---

## 🎯 What is TFGrid Studio?

**TFGrid Studio** is a complete development platform for ThreeFold Grid. The flagship tool, **tfgrid-compose**, brings docker-compose-style simplicity to decentralized deployments.

```bash
# Deploy any application with one command
tfgrid-compose up my-app
```

**No vendor lock-in. Industry standards. Decentralized infrastructure.**

---

## ✨ Current Status

### ✅ Production Ready (v1.0.0)

| Component | Status | Description |
|-----------|--------|-------------|
| **tfgrid-compose** | ✅ v1.0.0 | Universal orchestrator with single-vm pattern |
| **tfgrid-ai-agent** | ✅ v2.0.0 | AI coding agent (reference application) |
| **Single-VM Pattern** | ✅ Production | Deploy isolated VMs with private networking |
| **Context Files** | ✅ Production | Simplified workflow with `.tfgrid-compose.yaml` |
| **Agent Subcommand** | ✅ Production | AI agent management built-in |

### 🚧 Coming Soon

| Component | Status | Timeline | Source |
|-----------|--------|----------|--------|
| **Gateway Pattern** | 🚧 Planned | Q4 2025 | Inspired by [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway) |
| **K3s Pattern** | 🚧 Planned | Q1 2026 | Inspired by [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s) |
| **Web Dashboard** | 📋 Future | Q2 2026 | SaaS offering |
| **Marketplace** | 📋 Future | Q3 2026 | One-click app deployment |

---

## 🚀 Quick Start

### 1. Install tfgrid-compose

```bash
# Clone and install
git clone https://github.com/tfgrid-studio/tfgrid-compose
cd tfgrid-compose
make install

# Verify installation
tfgrid-compose --version
```

### 2. Configure ThreeFold

```bash
# Store your mnemonic
mkdir -p ~/.config/threefold
echo "your mnemonic words" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic
```

### 3. Create Context File (Optional but Recommended)

```bash
# In your project directory
echo "app: ../tfgrid-ai-agent" > .tfgrid-compose.yaml
```

### 4. Deploy

```bash
# With context file
tfgrid-compose up

# Or specify app path
tfgrid-compose up ../tfgrid-ai-agent
```

**That's it!** Your application is now running on ThreeFold Grid. 🎉

---

## 📚 Documentation Overview

### Getting Started
- **[Introduction](getting-started/introduction.md)** - What is TFGrid Compose and why use it
- **[Installation](getting-started/installation.md)** - Install and configure tfgrid-compose
- **[Quick Start](getting-started/quickstart.md)** - Deploy your first application in 5 minutes
- **[Core Concepts](getting-started/concepts.md)** - Understand patterns, apps, and manifests

### Deployment Patterns
- **[Pattern Overview](patterns/overview.md)** - Understanding the pattern system
- **[Single-VM Pattern](patterns/single-vm/)** - ✅ Production ready
- **[Gateway Pattern](patterns/gateway/)** - 🚧 Coming Q4 2025 (source available)
- **[K3s Pattern](patterns/k3s/)** - 🚧 Coming Q1 2026 (source available)

### Applications
- **[Application Overview](applications/overview.md)** - How apps work in TFGrid Compose
- **[TFGrid AI Agent](applications/tfgrid-ai-agent/)** - ✅ AI coding agent
- **[Creating Apps](applications/creating-apps.md)** - Build your own deployable apps

### CLI Reference
- **[CLI Commands](reference/cli.md)** - Complete command reference
- **[App Manifest](reference/manifest.md)** - `tfgrid-compose.yaml` specification
- **[Context File](reference/context-file.md)** - `.tfgrid-compose.yaml` usage
- **[State Management](reference/state.md)** - How state is tracked
- **[Environment Variables](reference/environment.md)** - Configuration options

### Guides
- **[Migration Guide](guides/migration.md)** - Migrate from standalone repos
- **[Advanced Deployment](guides/deployment.md)** - Production deployment strategies
- **[Networking](guides/networking.md)** - WireGuard and Mycelium setup
- **[Security](guides/security.md)** - Security best practices
- **[Troubleshooting](guides/troubleshooting.md)** - Common issues and solutions

### Architecture
- **[System Architecture](architecture/overview.md)** - How TFGrid Compose works
- **[Design Decisions](architecture/design-decisions.md)** - Why we built it this way
- **[Source Repositories](architecture/source-repos.md)** - Acknowledgment of source work
- **[Comparison](architecture/comparison.md)** - vs standalone repos, vs other platforms

### Roadmap & Contributing
- **[Current Status](roadmap/current.md)** - ✅ What's working now (v1.0.0)
- **[Planned Features](roadmap/planned.md)** - 🚧 What's coming next
- **[Changelog](roadmap/changelog.md)** - Version history
- **[Contributing](contributing/overview.md)** - How to contribute

---

## 🎯 Use Cases

### AI/ML Development
Deploy isolated AI coding environments on ThreeFold Grid.
```bash
tfgrid-compose up tfgrid-ai-agent
```

### Web Applications (Coming Soon)
Deploy web apps with public IPv4, SSL, and reverse proxy.
```bash
tfgrid-compose up my-webapp --pattern=gateway --domain=myapp.com
```

### Cloud-Native Apps (Coming Soon)
Deploy microservices on Kubernetes clusters.
```bash
tfgrid-compose up my-saas --pattern=k3s
```

### Databases
Deploy databases with persistent storage and private networking.
```bash
tfgrid-compose up my-postgres --pattern=single-vm
```

---

## 💡 Why TFGrid Compose?

### ✅ No Vendor Lock-in
Uses industry standards: **Terraform**, **Ansible**, **Kubernetes**. Your apps aren't locked to ThreeFold Grid.

### ✅ Simple & Powerful
**Heroku-like UX** with production-ready patterns. One command to deploy, full control when needed.

### ✅ Decentralized Infrastructure
Runs on **ThreeFold Grid**: decentralized compute, no single point of failure, cost-effective.

### ✅ Open Source
**Apache 2.0 license**. Free to use, modify, and distribute. Community-driven development.

### ✅ Battle-Tested
Built from **proven, working implementations**:
- Gateway pattern: Based on [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway)
- K3s pattern: Based on [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s)
- AI agent: Based on [mik-tf/tfgrid-ai-agent](https://github.com/mik-tf/tfgrid-ai-agent)

---

## 🔗 Links

- **GitHub Organization:** [github.com/tfgrid-studio](https://github.com/tfgrid-studio)
- **Main Repository:** [tfgrid-compose](https://github.com/tfgrid-studio/tfgrid-compose)
- **AI Agent:** [tfgrid-ai-agent](https://github.com/tfgrid-studio/tfgrid-ai-agent)
- **ThreeFold Grid:** [threefold.io](https://threefold.io)

---

## 🤝 Community & Support

- **Discussions:** [GitHub Discussions](https://github.com/orgs/tfgrid-compose/discussions)
- **Issues:** Open issues in respective repositories
- **Contributing:** See [contributing guide](contributing/overview.md)

---

## 📜 License

**FOSS Repositories:** Apache 2.0 License  
**Commercial Repositories:** Business Source License / Proprietary

See individual repositories for details.

---

<div align="center">

**Made with ❤️ for the decentralized web**

[Get Started](getting-started/quickstart.md) • [View on GitHub](https://github.com/tfgrid-studio) • [Community](https://github.com/orgs/tfgrid-compose/discussions)

</div>
