# Planned Features

**Future roadmap for TFGrid Compose**

---

## 🚧 Q4 2025: Gateway Pattern

**Status:** 🚧 Planned (Scaffolded 20%)  
**Timeline:** 2-4 weeks  
**Source:** Inspired by [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway)

### Overview

The gateway pattern provides **public IPv4 access** for web applications through a gateway VM with NAT/proxy forwarding to private backend VMs.

### Source Repository Features

The [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway) repo contains a complete, working implementation with:

**✅ Gateway Types:**
- NAT-based gateway (nftables)
- Proxy-based gateway (HAProxy + Nginx)
- SSL/TLS termination (Let's Encrypt)

**✅ Network Features:**
- Public IPv4 on gateway VM
- WireGuard private networking
- Mycelium IPv6 overlay
- Network redundancy (both modes)
- Port forwarding configuration

**✅ Advanced Features:**
- SSL certificates with auto-renewal
- Path-based routing (`/vm1`, `/vm2`)
- Port-based access (`:8081`, `:8082`)
- Security features (disable public ports)
- Live demo system with status pages

### Integration Plan

**Extract from source repo:**
1. ✅ Terraform infrastructure configs (gateway + backend VMs)
2. ✅ Ansible playbooks (NAT, proxy, SSL setup)
3. ✅ Network configuration (WireGuard, Mycelium)
4. ✅ SSL automation (certbot integration)

**Adapt to TFGrid Compose:**
1. [ ] Create pattern directory (`patterns/gateway/`)
2. [ ] Unify with manifest system (`tfgrid-compose.yaml`)
3. [ ] Simplify configuration (auto-detect settings)
4. [ ] Test with multiple applications
5. [ ] Write pattern documentation

### Usage (After Integration)

```bash
# Deploy web app with public access
tfgrid-compose up my-webapp --pattern=gateway --domain=myapp.com

# Automatic:
# ✅ Gateway VM with public IPv4
# ✅ Backend VMs with private networking
# ✅ SSL certificate from Let's Encrypt
# ✅ Nginx reverse proxy configured
# ✅ Health checks running
```

### Architecture

```
┌───────────────────────────────┐
│  Gateway VM (Public)          │
│  - Public IPv4                │
│  - SSL termination            │
│  - Nginx reverse proxy        │
│  - NAT/Proxy forwarding       │
└───────────────────────────────┘
             ↓
┌───────────────────────────────┐
│  Backend VMs (Private)        │
│  - Web app                    │
│  - Database                   │
│  - Cache                      │
│  - Private networking only    │
└───────────────────────────────┘
```

### Use Cases

- Production web applications
- E-commerce sites
- Multi-tier applications
- Traditional hosting with public access
- SaaS platforms

### Completion Status

| Task | Status | Notes |
|------|--------|-------|
| Extract Terraform configs | ❌ TODO | Source available |
| Extract Ansible playbooks | ❌ TODO | NAT + proxy roles |
| Extract SSL automation | ❌ TODO | certbot integration |
| Create pattern metadata | ❌ TODO | `pattern.yaml` |
| Adapt to manifest system | ❌ TODO | Auto-configure |
| Test with demo app | ❌ TODO | Validation |
| Write documentation | ❌ TODO | User guide |

**Progress:** 20% (scaffolded structure only)

---

## 🚧 Q1 2026: K3s Pattern

**Status:** 🚧 Planned (Scaffolded 20%)  
**Timeline:** 4-6 weeks  
**Source:** Inspired by [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s)

### Overview

The K3s pattern deploys **lightweight Kubernetes clusters** on ThreeFold Grid with full kubectl access and modern cloud-native features.

### Source Repository Features

The [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s) repo contains a complete, working implementation with:

**✅ Cluster Components:**
- K3s control plane nodes
- K3s worker nodes
- Management node with K9s TUI
- MetalLB load balancer
- Nginx Ingress controller

**✅ Networking:**
- Flannel CNI (VXLAN backend)
- WireGuard private network
- Mycelium IPv6 overlay
- Dual-stack IPv4/IPv6 support
- Load balancing across networks

**✅ Management:**
- K9s terminal UI
- kubectl pre-configured
- Helm chart support
- Ansible automation
- Health check verification

**✅ Advanced Features:**
- High availability support
- Auto-scaling workers
- Network mode selection
- Management node with all tools

### Integration Plan

**Extract from source repo:**
1. ✅ Terraform multi-VM configs (control, worker, management)
2. ✅ Ansible K3s playbooks (cluster setup, joining)
3. ✅ MetalLB + Nginx Ingress configs
4. ✅ Management node setup (kubectl, K9s, Helm)

**Adapt to TFGrid Compose:**
1. [ ] Create pattern directory (`patterns/k3s/`)
2. [ ] Support Helm chart deployments
3. [ ] Multi-app Kubernetes deployments
4. [ ] GitOps integration (ArgoCD/Flux)
5. [ ] Simplified kubectl access
6. [ ] Pattern documentation

### Usage (After Integration)

```bash
# Deploy microservices on Kubernetes
tfgrid-compose up my-saas --pattern=k3s

# Automatic:
# ✅ K3s cluster deployed (1 control + 3 workers)
# ✅ Management node with K9s
# ✅ MetalLB load balancer
# ✅ Nginx Ingress controller
# ✅ kubectl configured
# ✅ Helm ready

# Access cluster
tfgrid-compose kubectl get nodes
tfgrid-compose k9s  # Terminal UI
tfgrid-compose helm list
```

### Architecture

```
┌────────────────────────────────────┐
│  K3s Cluster                       │
│  ┌─────────────────────────────┐   │
│  │  Control Plane              │   │
│  │  - K3s master               │   │
│  │  - Traefik ingress          │   │
│  └─────────────────────────────┘   │
│  ┌─────────────────────────────┐   │
│  │  Worker Nodes (3+)          │   │
│  │  - Application pods         │   │
│  │  - Auto-scaling             │   │
│  └─────────────────────────────┘   │
│  ┌─────────────────────────────┐   │
│  │  Management Node            │   │
│  │  - kubectl, K9s, Helm       │   │
│  │  - Cluster access           │   │
│  └─────────────────────────────┘   │
└────────────────────────────────────┘
```

### Use Cases

- Cloud-native applications
- Microservices architectures
- Production SaaS platforms
- High availability requirements
- Auto-scaling workloads

### Completion Status

| Task | Status | Notes |
|------|--------|-------|
| Extract Terraform configs | ❌ TODO | Multi-VM setup |
| Extract Ansible playbooks | ❌ TODO | K3s + components |
| Extract MetalLB/Ingress | ❌ TODO | Load balancing |
| Create pattern metadata | ❌ TODO | `pattern.yaml` |
| Helm chart support | ❌ TODO | App deployment |
| GitOps integration | ❌ TODO | ArgoCD/Flux |
| Write documentation | ❌ TODO | Complete guide |

**Progress:** 20% (scaffolded structure only)

---

## ✅ Completed: Brand & Website (Oct 9, 2025)

**What was delivered:**
- ✅ **Rebranded to TFGrid Studio** - Better positioning as complete platform
- ✅ **Marketing website** ([tfgrid.studio](https://tfgrid.studio)) - Modern Astro + Tailwind site
- ✅ **Documentation site** ([docs.tfgrid.studio](https://docs.tfgrid.studio)) - MkDocs Material
- ✅ **Renamed repos** - tfgrid-deployer → tfgrid-compose (more accurate)
- ✅ **GitHub organization** - github.com/tfgrid-studio

**Impact:**
- Professional brand presence
- Clear product positioning
- Zero-cost hosting (GitHub Pages)
- Fast, modern web stack

---

## 📋 Q2 2026: Advanced Features

### v2.0 Enhancements

#### Monitoring & Observability
- [ ] Web dashboard (real-time metrics)
- [ ] Prometheus integration
- [ ] Loki log aggregation
- [ ] Alerting (PagerDuty/Slack)
- [ ] Cost tracking

#### Database Support
- [ ] PostgreSQL clusters
- [ ] MongoDB replica sets
- [ ] Redis caching
- [ ] Automated backups
- [ ] Point-in-time recovery

#### Developer Experience
- [ ] Shell completion (bash/zsh/fish)
- [ ] VS Code extension
- [ ] Better debugging tools
- [ ] Deployment rollback
- [ ] Preview environments

#### Testing & Quality
- [ ] Automated integration tests
- [ ] Unit tests for core functions
- [ ] CI/CD pipeline
- [ ] End-to-end tests
- [ ] Error scenario testing

---

## 📋 Q3-Q4 2026: Ecosystem & Platform

### Marketplace

**TFGrid Marketplace** - One-click app deployment

**Features:**
- [ ] App catalog
- [ ] One-click install
- [ ] App ratings & reviews
- [ ] Developer submissions
- [ ] Revenue sharing (80/20)

**Example:**
```bash
# Browse marketplace
tfgrid-compose marketplace search wordpress

# Deploy from marketplace
tfgrid-compose marketplace install wordpress
```

### Web Dashboard

**TFGrid Web** - Fully managed platform (SaaS)

**Features:**
- [ ] Web UI for deployments
- [ ] Drag-and-drop deployment
- [ ] Visual environment management
- [ ] Team collaboration
- [ ] Usage analytics
- [ ] Billing integration

**Pricing:**
```
Free:       $0 - 1 deployment
Hobbyist:   $10/mo - 3 projects
Pro:        $50/mo - 10 projects
Business:   $200/mo - Unlimited
```

### CI/CD Integration

**GitHub Actions / GitLab CI Integration**

**Features:**
- [ ] GitHub Actions workflow
- [ ] GitLab CI integration
- [ ] Automated testing
- [ ] Deployment pipelines
- [ ] Rollback automation

**Example:**
```yaml
# .github/workflows/deploy.yml
- name: Deploy to TFGrid
  uses: tfgrid-compose/deploy-action@v1
  with:
    app: ./my-app
    pattern: gateway
```

### Multi-Cloud Support

**Deploy to multiple clouds**

**Features:**
- [ ] AWS integration
- [ ] GCP integration
- [ ] Azure integration
- [ ] Hybrid deployments
- [ ] Cost optimization

---

## 🎯 Success Metrics

### User Growth Targets

| Metric | Year 1 | Year 2 | Year 3 |
|--------|--------|--------|--------|
| **Total Users** | 10,000 | 50,000 | 200,000 |
| **Paying Customers** | 100 | 500 | 2,000 |
| **GitHub Stars** | 1,000 | 5,000 | 15,000 |
| **Discord Members** | 500 | 2,000 | 10,000 |
| **Monthly Active Users** | 2,000 | 10,000 | 50,000 |

### Revenue Targets

| Year | Target ARR | Key Drivers |
|------|-----------|-------------|
| **Year 1** | $144K | Compute credits, early licenses |
| **Year 2** | $1.28M | Marketplace, enterprise sales |
| **Year 3** | $5.5M | Managed services, scaling |

### Community Targets

| Metric | Year 1 | Year 2 | Year 3 |
|--------|--------|--------|--------|
| **Contributors** | 10 | 50 | 200 |
| **Marketplace Apps** | 50 | 200 | 1,000 |
| **Partner Integrations** | 5 | 20 | 100 |

---

## 🔗 Source Repository Links

All planned patterns are based on proven, working implementations:

| Pattern | Source Repository | Status |
|---------|------------------|--------|
| **Gateway** | [mik-tf/tfgrid-gateway](https://github.com/mik-tf/tfgrid-gateway) | Complete & Working |
| **K3s** | [ucli-tools/tfgrid-k3s](https://github.com/ucli-tools/tfgrid-k3s) | Complete & Working |
| **AI Agent** | [mik-tf/tfgrid-ai-agent](https://github.com/mik-tf/tfgrid-ai-agent) | ✅ Integrated (v1.0) |

**Why this matters:**
- ✅ Not vaporware - code exists and works
- ✅ Battle-tested in production
- ✅ Proven architectures
- ✅ Shorter integration time
- ✅ Lower risk

[Learn more about source acknowledgment →](../architecture/source-repos.md)

---

## 📅 Release Timeline

| Quarter | Release | Key Features |
|---------|---------|--------------|
| **Q4 2025** | v0.10.0 | Multi-deployment, enhanced monitoring |
| **Q1 2026** | v1.0.0 | Stable release, API locked |
| **Q2 2026** | v1.1.0 | Web dashboard, pattern registry |
| **Q3 2026** | v1.2.0 | Marketplace, community patterns |
| **Q4 2026** | v2.0.0 | Major enhancements (if needed) |

---

## 🤝 How to Contribute

Want to help make these features happen?

### For Developers
- Review source repos to understand implementations
- Help with extraction and adaptation
- Write tests and documentation
- Submit pull requests

### For Users
- Test pre-release versions
- Provide feedback
- Report bugs
- Share use cases

### For Organizations
- Sponsor development
- Enterprise partnerships
- Custom feature requests
- Training and consulting

[Contributing Guide →](../contributing/overview.md)

---

**Current Status:** [v0.9.0 Production Ready](current.md)  
**Next Up:** Multi-deployment support (v0.10.0)  
**Long-term:** Full platform ecosystem
