# Introduction to TFGrid Compose

**Universal deployment orchestrator for ThreeFold Grid applications**

---

## What is TFGrid Compose?

TFGrid Compose is a **production-ready deployment platform** that makes deploying applications on ThreeFold Grid as simple as deploying with docker-compose.

```bash
# One command to deploy any application
tfgrid-compose up my-app
```

Instead of managing Terraform configurations, Ansible playbooks, and WireGuard networks separately, TFGrid Compose unifies everything into a single, intuitive CLI tool.

---

## The Problem We Solve

### Before TFGrid Compose

Deploying applications on ThreeFold Grid required:

1. **Manual Infrastructure Setup** - Writing Terraform/OpenTofu configurations
2. **Network Configuration** - Setting up WireGuard or Mycelium manually
3. **Platform Configuration** - Writing Ansible playbooks
4. **Application Deployment** - Custom scripts for each app
5. **State Management** - Tracking deployments manually

**Result:** Hours of work, multiple repos, complex workflows.

### With TFGrid Compose

```bash
# Configure once
echo "app: ../my-app" > .tfgrid-compose.yaml

# Deploy anywhere
tfgrid-compose up
```

**Result:** Minutes to deploy, one command, consistent workflow.

---

## Core Architecture

TFGrid Compose separates concerns into three layers:

```
┌─────────────────────────────────────────────────────┐
│  Applications (Standalone, Portable)                │
│  tfgrid-ai-agent, your-app, any-app                 │
│  • Pattern-agnostic                                 │
│  • Manifest-driven (tfgrid-compose.yaml)            │
│  • Works on any infrastructure                      │
└─────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────┐
│  Deployer (Universal Orchestrator)                  │
│  tfgrid-deployer                                    │
│  • Pattern system (single-vm, gateway, k3s)         │
│  • Terraform + Ansible automation                   │
│  • State management                                 │
│  • CLI tool (tfgrid-compose)                        │
└─────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────┐
│  Infrastructure (ThreeFold Grid)                    │
│  Decentralized compute, storage, networking         │
│  • VMs, Kubernetes, networking                      │
│  • WireGuard, Mycelium                              │
│  • Cost-effective, distributed                      │
└─────────────────────────────────────────────────────┘
```

---

## Key Concepts

### 1. Applications

**Standalone, portable codebases** that can be deployed anywhere.

- Contain app source code (`src/`, `app/`, etc.)
- Include deployment manifest (`tfgrid-compose.yaml`)
- Define deployment hooks (`deployment/setup.sh`, etc.)
- **Pattern-agnostic** - work with any deployment pattern

**Example:** `tfgrid-ai-agent` is a complete application

### 2. Patterns

**Reusable deployment strategies** for different infrastructure needs.

| Pattern | Description | Status |
|---------|-------------|--------|
| **single-vm** | Single VM with private networking | ✅ Production |
| **gateway** | Gateway VM + backend VMs with public IPv4 | 🚧 Q4 2025 |
| **k3s** | Kubernetes cluster with auto-scaling | 🚧 Q1 2026 |

### 3. Deployer

**Universal orchestrator** that:
- Reads app manifests
- Selects deployment pattern
- Provisions infrastructure (Terraform)
- Configures platform (Ansible)
- Deploys application
- Manages state

### 4. Manifest

**Simple YAML file** (`tfgrid-compose.yaml`) that describes your app:

```yaml
name: my-app
version: 1.0.0

patterns:
  recommended: single-vm

resources:
  cpu: {recommended: 4}
  memory: {recommended: 8192}
  disk: {recommended: 100}

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

---

## Workflow Example

### Traditional Deployment (Without TFGrid Compose)

```bash
# 1. Navigate to project
cd my-tfgrid-project

# 2. Configure Terraform
cd infrastructure
cp terraform.tfvars.example terraform.tfvars
nano terraform.tfvars
export TF_VAR_mnemonic="..."

# 3. Deploy infrastructure
terraform init
terraform apply

# 4. Setup WireGuard
cd ../scripts
./setup-wireguard.sh

# 5. Generate Ansible inventory
./generate-inventory.sh

# 6. Run Ansible
cd ../platform
ansible-playbook site.yml

# 7. Deploy application
cd ../
./deploy-app.sh
```

**Time:** 30-60 minutes  
**Complexity:** High  
**Error-prone:** Yes

### With TFGrid Compose

```bash
# 1. One-time setup
echo "app: ../my-app" > .tfgrid-compose.yaml

# 2. Deploy
tfgrid-compose up
```

**Time:** 2-3 minutes  
**Complexity:** Low  
**Error-prone:** No

---

## Benefits

### 🚀 Speed
Deploy in **2-3 minutes** instead of 30-60 minutes.

### 🎯 Simplicity
**One command** instead of multiple manual steps.

### 🔄 Consistency
Same workflow for all applications, all patterns.

### 📦 Portability
Apps are **pattern-agnostic** - deploy on single-vm, gateway, or k3s without changes.

### 🔓 No Lock-in
Uses **industry standards** (Terraform, Ansible, Kubernetes). Easy to migrate away.

### 🔒 Safety
**Idempotent operations**, state tracking, validation before deployment.

---

## Use Cases

### 1. AI/ML Development
Deploy isolated coding environments for AI agents.
```bash
tfgrid-compose up tfgrid-ai-agent
```

### 2. Web Applications (Coming Soon)
Deploy traditional web apps with public access.
```bash
tfgrid-compose up my-webapp --pattern=gateway
```

### 3. Microservices (Coming Soon)
Deploy cloud-native apps on Kubernetes.
```bash
tfgrid-compose up my-saas --pattern=k3s
```

### 4. Databases
Deploy databases with persistent storage.
```bash
tfgrid-compose up my-postgres --pattern=single-vm
```

---

## How It Works

### Deployment Flow

```
1. Read app manifest (tfgrid-compose.yaml)
        ↓
2. Select pattern (single-vm, gateway, k3s)
        ↓
3. Validate configuration
        ↓
4. Generate Terraform config
        ↓
5. Provision infrastructure (VM, networking)
        ↓
6. Setup WireGuard/Mycelium
        ↓
7. Generate Ansible inventory
        ↓
8. Configure platform (Ansible)
        ↓
9. Deploy application source
        ↓
10. Run deployment hooks (setup → configure → healthcheck)
        ↓
11. Verify deployment
        ↓
12. Save state
        ↓
13. ✅ Done! (2-3 minutes total)
```

### State Management

TFGrid Compose automatically tracks:
- Deployed infrastructure
- Network configurations
- Application state
- Pattern used
- Timestamps

Stored in: `.tfgrid-compose/state.yaml`

---

## Comparison

### vs Docker Compose

| Feature | TFGrid Compose | Docker Compose |
|---------|----------------|----------------|
| **Deployment Target** | Cloud VMs | Local containers |
| **Infrastructure** | Automated (Terraform) | None |
| **Networking** | WireGuard/Mycelium | Docker networks |
| **Use Case** | Production deployment | Development |
| **Complexity** | Simple (one command) | Simple |

### vs Kubernetes

| Feature | TFGrid Compose | Kubernetes |
|---------|----------------|------------|
| **Learning Curve** | Low | High |
| **Setup Time** | 2-3 minutes | Hours/Days |
| **Patterns** | Multiple (VM, K8s, etc.) | K8s only |
| **Simplicity** | High | Low |
| **Power** | Medium-High | High |

### vs Heroku/Vercel

| Feature | TFGrid Compose | Heroku/Vercel |
|---------|----------------|---------------|
| **Vendor Lock-in** | ❌ No | ✅ Yes |
| **Cost** | Low | Medium-High |
| **Control** | Full | Limited |
| **Infrastructure** | Decentralized | Centralized |
| **Patterns** | Multiple | Single |

---

## Source & Credits

TFGrid Compose was built by **extracting and unifying** proven, production-ready implementations:

- **Gateway Pattern:** Inspired by [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway)
- **K3s Pattern:** Inspired by [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s)
- **AI Agent App:** Based on [mik-tf/tfgrid-ai-agent](https://github.com/mik-tf/tfgrid-ai-agent)

These repositories contain complete, working code that has been validated in real deployments.

[Learn more about source repositories →](../architecture/source-repos.md)

---

## Next Steps

- **[Installation](installation.md)** - Install tfgrid-deployer and prerequisites
- **[Quick Start](quickstart.md)** - Deploy your first application in 5 minutes
- **[Core Concepts](concepts.md)** - Deep dive into patterns and manifests

---

**Ready to get started?** → [Install TFGrid Compose](installation.md)
