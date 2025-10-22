# Deployment Patterns Overview

TFGrid Compose provides three deployment patterns that cover every use case from development to enterprise production.

---

## Pattern Philosophy

Each pattern is:

- ✅ **Production-ready** - Tested and verified
- ✅ **Purpose-built** - Optimized for specific use cases
- ✅ **Cost-effective** - Pay only for what you need
- ✅ **Battle-tested** - Based on proven implementations

---

## The Three Patterns

### 🔹 [Single-VM Pattern](single-vm.md)

**Simple VM deployment for development and internal services**

```bash
tfgrid-compose up my-app --pattern=single-vm
```

**Best for:**

- AI agents & coding environments
- Databases and data stores
- Internal APIs and services
- Development environments

**Deployment time:** 2-3 minutes  
**Cost:** $10-30/month

[Learn more →](single-vm.md)

---

### 🌐 [Gateway Pattern](gateway.md)

**Multi-VM with public access and SSL for production web apps**

```bash
tfgrid-compose up my-webapp --pattern=gateway --domain=myapp.com
```

**Best for:**
- Production websites
- E-commerce sites
- SaaS applications
- Public web services

**Deployment time:** 5-7 minutes  
**Cost:** $30-100/month

[Learn more →](gateway.md)

---

### 🚀 [K3s Pattern](k3s.md)

**Full Kubernetes cluster for cloud-native applications**

```bash
tfgrid-compose up my-cluster --pattern=k3s
```

**Best for:**

- Cloud-native applications
- Microservices architectures
- Enterprise deployments
- Production SaaS at scale

**Deployment time:** 10-15 minutes  
**Cost:** $100-500/month

[Learn more →](k3s.md)

---

## Choosing a Pattern

### Start Simple → Scale Up

**Development & Testing:**
Start with `single-vm` for fast, isolated development environments.

**MVP & Early Production:**
Move to `gateway` when you need public access and SSL.

**Scale & Enterprise:**
Upgrade to `k3s` for cloud-native features and horizontal scaling.

---

## Pattern Comparison

| Feature | Single-VM | Gateway | K3s |
|---------|-----------|---------|-----|
| **Public IP** | ❌ | ✅ | ✅ |
| **SSL/TLS** | ❌ | ✅ Auto | ✅ Via Ingress |
| **Load Balancing** | ❌ | ✅ | ✅ MetalLB |
| **Scaling** | Manual | Manual | Auto |
| **Complexity** | Low | Medium | High |
| **Deploy Time** | 2-3 min | 5-7 min | 10-15 min |
| **Min Cost** | $10/mo | $30/mo | $100/mo |

---

## Pattern Architecture

### Single-VM Architecture
```
Your Laptop → WireGuard VPN → Single VM
                              (App + Data)
```

### Gateway Architecture
```
Internet → Gateway VM → Private Network → Backend VMs
         (Public IP)   (Reverse Proxy)   (App + DB)
         (SSL/TLS)
```

### K3s Architecture
```
Your Laptop → Management Node → K3s Cluster
              (kubectl/helm)    ├─ Control Plane
                                ├─ Worker Nodes
                                └─ Services (MetalLB, Ingress)
```

---

## Migration Path

Patterns are designed for easy migration:

**Single-VM → Gateway**

- Add public domain configuration
- Enable SSL
- Deploy gateway VM
- Update DNS

**Gateway → K3s**

- Define Kubernetes manifests
- Deploy K3s cluster
- Migrate services to pods
- Update ingress rules

---

## Next Steps

- **New to TFGrid Compose?** → Start with [Quick Start Guide](../getting-started/quickstart.md)
- **Ready to deploy?** → Choose your pattern above
- **Need help deciding?** → See [Use Cases](../getting-started/introduction.md#use-cases)

---

## Pattern Sources

All patterns are built on proven, working implementations:

- **Single-VM:** Standard VM deployment with WireGuard
- **Gateway:** Based on [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway)
- **K3s:** Based on [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s)

See [Architecture: Source Repositories](../architecture/source-repos.md) for acknowledgments.
