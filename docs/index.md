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

### ✅ Production Ready (v0.10.0)

| Component | Status | Description |
|-----------|--------|-------------|
| **tfgrid-compose** | ✅ v0.10.0 | Universal orchestrator with registry integration |
| **tfgrid-ai-agent** | ✅ v0.10.0 | AI coding agent (reference application) |
| **tfgrid-gitea** | 🆕 v1.0.0 | Self-hosted Git service with web interface |
| **Single-VM Pattern** | ✅ Production | Deploy isolated VMs with private networking |
| **Gateway Pattern** | ✅ Production | Multi-VM with public access and SSL |
| **K3s Pattern** | ✅ Production | Full Kubernetes cluster deployment |
| **App Registry** | ✅ v0.10.0 | Deploy apps by name from registry |
| **Context Files** | ✅ Production | Simplified workflow with `.tfgrid-compose.yaml` |

### 🔨 In Progress (v0.10.1) - 95% Complete!

| Component | Status | Description |
|-----------|--------|-------------|
| **Login/Logout UX** | ✅ Complete | Enhanced credential management |
| **ThreeFold Setup Guide** | ✅ Complete | Complete DIY onboarding documentation |
| **Config Management** | ✅ Complete | `tfgrid-compose config` commands |
| **Docs Command** | ✅ Complete | Open docs in browser |
| **Enhanced Error Messages** | ✅ Complete | Helpful guidance throughout CLI |
| **Release** | 🔄 Testing | Final testing before release |

### 🚧 Coming Soon

| Component | Status | Timeline | Description |
|-----------|--------|----------|-------------|
| **AI Gateway Integration** | 📋 Planned | v0.11.0 (Nov) | AI agent + Gateway + Gitea workflow |
| **AI Dashboard Web UI** | 📋 Planned | v0.12.0 (Dec-Jan) | Visual project management |
| **Voice Coding** | 📋 Planned | v0.13.0 (Feb) | "Vibe code by talking" interface |
| **CI/CD Integration** | 📋 Planned | v0.13.0+ | GitHub Actions, GitLab CI |
| **Web Dashboard** | 📋 Planned | Q2 2026 | Complete visual management |
| **Managed Service** | 📋 Planned | Q3 2026+ | Email + credit card deployments |

---

## 🚀 Quick Start

### 1. Install tfgrid-compose

```bash
# One-line installer
curl -sSL install.tfgrid.studio/install.sh | sh

# Verify installation
tfgrid-compose --version
```

### 2. Set Up ThreeFold

Need a ThreeFold wallet with TFT? See the [complete setup guide](getting-started/threefold-setup.md).

Already have a wallet? Configure it:
```bash
tfgrid-compose login
# Enter your seed phrase when prompted
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
- **[ThreeFold Setup](getting-started/threefold-setup.md)** - Complete DIY guide (wallet, TFT, KYC)
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
- **[TFGrid Gitea](guides/gitea.md)** - 🆕 Self-hosted Git service
- **[Creating Apps](applications/creating-apps.md)** - Build your own deployable apps

### CLI Reference
- **[CLI Commands](reference/cli.md)** - Complete command reference
- **[App Manifest](reference/manifest.md)** - `tfgrid-compose.yaml` specification
- **[Context File](reference/context-file.md)** - `.tfgrid-compose.yaml` usage
- **[State Management](reference/state.md)** - How state is tracked
- **[Environment Variables](reference/environment.md)** - Configuration options

### Guides
- **[App Registry](guides/app-registry.md)** - 🆕 Deploy apps by name (v0.10.0+)
- **[Cache Management](guides/cache-management.md)** - 🆕 Enhanced cache system with version-based invalidation (v0.13.4+)
- **[Migration Guide](guides/migration.md)** - Migrate from standalone repos
- **[Advanced Deployment](guides/deployment.md)** - Production deployment strategies
- **[Networking](guides/networking.md)** - WireGuard and Mycelium setup
- **[Security](guides/security.md)** - Security best practices
- **[Troubleshooting](guides/troubleshooting.md)** - Common issues and solutions

### Architecture
- **[Design Decisions](architecture/design-decisions.md)** - Why we built it this way
- **[Source Repositories](architecture/source-repos.md)** - Acknowledgment of source work
- **[Comparison](architecture/comparison.md)** - vs standalone repos, vs other platforms

### Roadmap
- **[Platform Roadmap](roadmap/index.md)** - 🚀 Now / Next / Later
- **[Now - Active Development](roadmap/now.md)** - What we're building
- **[Next - Planned Features](roadmap/next.md)** - What's coming
- **[Later - Future Concepts](roadmap/later.md)** - Long-term vision

### Contributing
- **[Contributing Guide](contributing/overview.md)** - How to contribute
- **[Code of Conduct](community/code-of-conduct.md)** - Community guidelines
- **[Security Policy](community/security.md)** - Report security issues

### Development
- **[Submit Your App](development/submit-app.md)** - Share your app with the community
- **[Pattern Contract](development/pattern-contract.md)** - How patterns communicate
- **[Pattern Development](development/pattern-development.md)** - Create custom patterns
- **[Versioning Policy](development/versioning-policy.md)** - Version management

---

## 🎯 Use Cases

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
- **Gitea:** [tfgrid-gitea](https://github.com/tfgrid-studio/tfgrid-gitea) - Self-hosted Git
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