# Core Concepts

Understanding the key concepts behind TFGrid Compose will help you deploy and manage applications effectively.

---

## Apps

An **app** is a deployable unit that includes everything needed to run on ThreeFold Grid:

- **Manifest** (`tfgrid-compose.yaml`) - Defines the app's requirements and configuration
- **Deployment hooks** - Scripts that run during deployment lifecycle
- **Source code** - Your application code (optional, can be pulled from git)

```
my-app/
├── tfgrid-compose.yaml      # App manifest (required)
├── deployment/              # Lifecycle hooks
│   ├── setup.sh
│   ├── configure.sh
│   └── healthcheck.sh
└── src/                     # Your application code
```

Apps can come from:

- **Registry** - Official and community apps (`tfgrid-compose up tfgrid-ai-stack`)
- **Local path** - Your own apps (`tfgrid-compose up ./my-app`)
- **Git URL** - Any git repository (`tfgrid-compose up https://github.com/org/app`)

---

## Manifests

The **manifest** (`tfgrid-compose.yaml`) is the heart of every app. It declares:

```yaml
name: my-app
version: "1.0.0"
description: My application

# Which deployment pattern to use
patterns:
  recommended: single-vm
  supported:
    - single-vm
    - gateway

# Resource requirements
resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096
    recommended: 8192
  disk:
    min: 50
    recommended: 100

# Deployment lifecycle hooks
hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

See the [Custom Apps Guide](../guides/custom-apps.md) for complete manifest reference.

---

## Deployment Patterns

**Patterns** are reusable infrastructure templates that define how your app is deployed:

### single-vm

Single virtual machine with private networking.

```bash
tfgrid-compose up my-app --pattern=single-vm
```

**Best for:** Development, databases, AI agents, internal services

**Features:**

- Private WireGuard/Mycelium networking
- Fast deployment (2-3 minutes)
- Cost-effective ($10-30/month)

### gateway

Multi-VM with public IPv4, SSL, and load balancing.

```bash
tfgrid-compose up my-app --pattern=gateway --domain=myapp.com
```

**Best for:** Production web apps, SaaS, e-commerce

**Features:**

- Public IPv4 address
- Automatic SSL via Let's Encrypt
- Reverse proxy and load balancing
- Private backend network

### k3s

Kubernetes cluster using lightweight K3s.

```bash
tfgrid-compose up my-app --pattern=k3s
```

**Best for:** Microservices, cloud-native apps, enterprise workloads

**Features:**

- Control plane + worker nodes
- MetalLB load balancer
- Nginx Ingress Controller
- Management node with kubectl, helm, k9s

See [Deployment Patterns](../patterns/overview.md) for detailed documentation.

---

## Hooks

**Hooks** are shell scripts that run at specific points during deployment:

| Hook | When it runs | Purpose |
|------|--------------|---------|
| `setup.sh` | After VM is ready | Install dependencies, create directories |
| `configure.sh` | After setup | Configure services, start application |
| `healthcheck.sh` | After configure | Verify deployment is working |

Hooks run **on the VM**, not on your local machine.

Example `setup.sh`:
```bash
#!/usr/bin/env bash
set -euo pipefail

apt-get update
apt-get install -y nginx postgresql
mkdir -p /opt/my-app
```

---

## State Management

TFGrid Compose tracks deployment state in a `.tfgrid-compose/` directory:

```
.tfgrid-compose/
├── state.yaml           # Deployment metadata
├── terraform/           # Infrastructure state
├── inventory.ini        # Ansible inventory
└── *.log               # Deployment logs
```

**Key state information:**

- VM IP addresses (WireGuard, Mycelium, public)
- Node IDs used
- Deployment status
- Contract IDs

Commands that use state:

```bash
tfgrid-compose status    # Show deployment status
tfgrid-compose ssh       # Connect to VM
tfgrid-compose logs      # View logs
tfgrid-compose down      # Destroy deployment
```

---

## Versioning

TFGrid Compose uses **Git commit hashes** as the primary version identifier:

```bash
$ tfgrid-compose up tfgrid-ai-stack
✅ Application loaded: tfgrid-ai-stack 24c9148
ℹ Git commit: 24c9148
ℹ Last updated: 2025-11-11 22:47:49
```

**Benefits:**

- **Precise** - Every deployment has a unique, immutable version
- **Automatic** - No manual version bumping required
- **Traceable** - Know exactly which code is running

---

## Registry

The **App Registry** is a catalog of deployable applications:

- **Official apps** - Maintained by TFGrid Studio (tfgrid-ai-stack, tfgrid-ai-agent, tfgrid-gitea)
- **Community apps** - Verified contributions from the community

```bash
# Search the registry
tfgrid-compose search ai

# Deploy from registry
tfgrid-compose up tfgrid-ai-stack
```

Browse apps at [registry.tfgrid.studio](https://registry.tfgrid.studio).

---

## Context System

The **context system** lets you work with multiple deployments without specifying which one each time:

```bash
# With one deployment, context is automatic
tfgrid-compose ssh       # Connects to the only deployment

# With multiple deployments, select one
tfgrid-compose select tfgrid-ai-stack
tfgrid-compose ssh       # Connects to selected deployment
```

See [Context System](../cli/context-system.md) for details.

---

## Next Steps

- **[Quick Start](quickstart.md)** - Deploy your first app
- **[Custom Apps Guide](../guides/custom-apps.md)** - Create your own apps
- **[Deployment Patterns](../patterns/overview.md)** - Learn about patterns
- **[CLI Reference](../cli/app-commands.md)** - Complete command reference
