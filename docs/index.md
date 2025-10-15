# TFGrid Studio Documentation

**Complete development platform for ThreeFold Grid**

Build, deploy, and scale decentralized applications with `tfgrid-compose` CLI and integrated tools.

> 🌐 **New here?** Check out our [main website](https://tfgrid.studio) for an overview of TFGrid Studio, pricing, and features.

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

### ✅ Production Ready (v0.9.0)

| Component | Status | Description |
|-----------|--------|-------------|
| **tfgrid-compose** | ✅ v0.9.0 | Universal orchestrator with all 3 patterns |
| **tfgrid-ai-agent** | ✅ v0.9.0 | AI coding agent (reference application) |
| **Single-VM Pattern** | ✅ Production | Deploy isolated VMs with private networking |
| **Gateway Pattern** | ✅ Production | Multi-VM with public access and SSL |
| **K3s Pattern** | ✅ Production | Full Kubernetes cluster deployment |
| **Context Files** | ✅ Production | Simplified workflow with `.tfgrid-compose.yaml` |
| **Agent Subcommand** | ✅ Production | AI agent management built-in |

### 🚧 Coming Soon

| Component | Status | Timeline | Description |
|-----------|--------|----------|-------------|
| **App Registry** | 🔨 Development | v0.10.0 (Nov 2025) | Deploy apps by name: `tfgrid-compose up tfgrid-ai-agent` |
| **Multi-Deployment** | 🔨 Development | v0.10.0 (Nov 2025) | Manage multiple deployments simultaneously |
| **Web Dashboard** | 📋 Planned | Q2 2026 | Visual management interface |
| **Marketplace** | 📋 Planned | Q3 2026 | Community app ecosystem |

---

## 🚀 Quick Start

### 1. Install tfgrid-compose

```bash
# One-line installer
curl -sSL install.tfgrid.studio/install.sh | sh

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

### 3. Deploy an App

```bash
# Deploy official app (v0.10.0+)
tfgrid-compose up tfgrid-ai-agent

# Or use local path (current)
tfgrid-compose up ../tfgrid-ai-agent

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
- **[Single-VM Pattern](patterns/single-vm.md)** - ✅ Production ready
- **[Gateway Pattern](patterns/gateway.md)** - ✅ Production ready
- **[K3s Pattern](patterns/k3s.md)** - ✅ Production ready

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
- **[App Registry](guides/app-registry.md)** - 🆕 Deploy apps by name (v0.10.0+)
- **[Migration Guide](guides/migration.md)** - Migrate from standalone repos
- **[Advanced Deployment](guides/deployment.md)** - Production deployment strategies
- **[Networking](guides/networking.md)** - WireGuard and Mycelium setup
- **[Security](guides/security.md)** - Security best practices
- **[Troubleshooting](guides/troubleshooting.md)** - Common issues and solutions

### Architecture
- **[Design Decisions](architecture/design-decisions.md)** - Why we built it this way
- **[Source Repositories](architecture/source-repos.md)** - Acknowledgment of source work
- **[Comparison](architecture/comparison.md)** - vs standalone repos, vs other platforms

### Roadmap & Contributing
- **[Current Status](roadmap/current.md)** - ✅ What's working now (v0.9.0 Production Ready)
- **[Planned Features](roadmap/planned.md)** - 🚧 What's coming next
- **[Changelog](roadmap/changelog.md)** - Version history
- **[Contributing](contributing/overview.md)** - How to contribute

### Development
- **[Submit Your App](development/submit-app.md)** - Share your app with the community
- **[Pattern Contract](development/pattern-contract.md)** - How patterns communicate
- **[Pattern Development](development/pattern-development.md)** - Create custom patterns
- **[Versioning Policy](development/versioning-policy.md)** - Version management

---

## 🎯 Use Cases

{{ ... }}
Deploy isolated AI coding environments on ThreeFold Grid.
```bash
tfgrid-compose up tfgrid-ai-agent
```

### Web Applications
Deploy web apps with public IPv4, SSL, and reverse proxy.
```bash
tfgrid-compose up my-webapp --pattern=gateway --domain=myapp.com
```

### Cloud-Native Apps
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

- **Website:** [tfgrid.studio](https://tfgrid.studio) - Marketing site
- **Documentation:** [docs.tfgrid.studio](https://docs.tfgrid.studio) - You are here!
- **GitHub:** [github.com/tfgrid-studio](https://github.com/tfgrid-studio) - Organization
- **CLI Tool:** [tfgrid-compose](https://github.com/tfgrid-studio/tfgrid-compose) - Main repository
- **AI Agent:** [tfgrid-ai-agent](https://github.com/tfgrid-studio/tfgrid-ai-agent) - AI development
- **ThreeFold Grid:** [threefold.io](https://threefold.io) - Infrastructure

---

## 🤝 Community & Support

- **Discussions:** [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- **Issues:** Open issues in respective repositories
- **Contributing:** See [contributing guide](contributing/overview.md)

---

## 📜 License

**FOSS Repositories:** Apache 2.0 License  
**Commercial Repositories:** Business Source License / Proprietary

See individual repositories for details.

---

**Made with 🔥 for the decentralized web**

[Get Started](getting-started/quickstart.md) • [View on GitHub](https://github.com/tfgrid-studio)