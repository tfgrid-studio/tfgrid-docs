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
# Deploy official app from registry
tfgrid-compose up tfgrid-ai-stack

# Or deploy by name
tfgrid-compose up tfgrid-ai-agent

# Or use local path
tfgrid-compose up ./my-app
```

**That's it!** Your application is now running on ThreeFold Grid. 🎉

---

## 📚 Where to go next

### 1. New to TFGrid Studio?

- **[Introduction](getting-started/introduction.md)** – what TFGrid Studio and `tfgrid-compose` are.
- **[Installation](getting-started/installation.md)** – install and configure `tfgrid-compose`.
- **[ThreeFold Setup](getting-started/threefold-setup.md)** – wallet, TFT, and account setup.
- **[Quick Start](getting-started/quickstart.md)** – deploy your first application in a few minutes.

### 2. Prefer a visual dashboard?

- **[TFGrid Dashboard](guides/tfgrid-dashboard.md)** – GUI alternative to the CLI for apps, deployments, and commands.
- **[TFGrid AI Stack](guides/tfgrid-ai-stack.md)** – full AI workspace (dashboard, agent, and Gitea).

### 3. Need more depth?

- **[Guides](guides/tfgrid-compose.md)** – product and workflow guides for TFGrid Compose and apps.
- **[Deployment patterns](patterns/overview.md)** – single‑VM, gateway, and k3s patterns.
- **[CLI reference](cli/app-commands.md)** – detailed commands and flags.

### 4. Building on top of TFGrid Studio?

- **[Submit your app](development/submit-app.md)** – share apps with the community.
- **[Pattern contract](development/pattern-contract.md)** – how patterns communicate.
- **[Architecture overview](architecture/overview.md)** – how the system fits together.

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
Built from ThreeFold open-source **proven, working code implementations**:
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
- **Contributing:** See [contributing guide](community/contributing.md)

---

## 📜 License

**FOSS Repositories:** Apache 2.0 License  
**Commercial Repositories:** Business Source License / Proprietary

See individual repositories for details.

---

**Made with 🔥 for the decentralized web**

[Get Started](getting-started/quickstart.md) • [View on GitHub](https://github.com/tfgrid-studio)