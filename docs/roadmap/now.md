# Now - Active Development

**🔨 What we're building right now**

---

## Recently Completed ✅

### CLI Integration (v0.10.0 - Oct 15, 2025)

**Status:** ✅ **COMPLETED**

Deploy apps by name directly from the registry!

```bash
# Search the registry
tfgrid-compose search

# Deploy by name
tfgrid-compose up wordpress

# Manage multiple apps
tfgrid-compose list
tfgrid-compose switch ai-agent
```

**See:** [Registry Guide](../cli/registry.md) | [Release Notes](https://github.com/tfgrid-studio/tfgrid-compose/releases/tag/v0.10.0)

---

## Currently Building 🔨

### Web Dashboard

**Status:** 🔨 Planning & Design

**Goal:** Visual management interface for deployments

### What It Will Do

```
🖥️ Modern web interface to:
- View all deployments at a glance
- Deploy apps with GUI
- Monitor resources in real-time  
- Manage team access
- View logs and metrics
```

### Features Planned

**Visual Deployment:**
- Browse registry in GUI
- One-click app deployment
- Configuration wizard
- Pattern selection UI

**Real-Time Monitoring:**
- Resource usage graphs
- Live log streaming
- Health check dashboard
- Alert notifications

**Team Collaboration:**
- Multi-user access
- Role-based permissions
- Deployment history
- Activity audit log

**Topology View:**
- Visual network diagram
- VM connections
- Service relationships
- Traffic flow

### Architecture Preview

```
┌─────────────────────────────────────────┐
│         Web Dashboard (React)           │
│  ┌────────┐  ┌────────┐  ┌────────┐   │
│  │Registry│  │Deploys │  │Monitor │   │
│  └────────┘  └────────┘  └────────┘   │
└─────────────────────────────────────────┘
                    ↕️
┌─────────────────────────────────────────┐
│      Backend API (FastAPI/Go)           │
│  ┌────────┐  ┌────────┐  ┌────────┐   │
│  │ Auth  │  │  State │  │Metrics │   │
│  └────────┘  └────────┘  └────────┘   │
└─────────────────────────────────────────┘
                    ↕️
┌─────────────────────────────────────────┐
│      tfgrid-compose CLI Engine          │
└─────────────────────────────────────────┘
```

### Timeline

- **Research:** Q4 2025
- **Design:** Q4 2025
- **Development:** Q1 2026
- **Beta:** Q1 2026

---

## Try Current Features

Use the newly released CLI features:

```bash
# Install
curl -sSL install.tfgrid.studio | sh

# Deploy from local manifest
tfgrid-compose up ./my-app

# Available patterns
tfgrid-compose up app --pattern=single-vm
tfgrid-compose up app --pattern=gateway
tfgrid-compose up app --pattern=k3s

# Browse registry
open https://registry.tfgrid.studio
```

---

## Progress Updates

Follow development:
- [GitHub Project Board](https://github.com/orgs/tfgrid-studio/projects)
- [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- [Changelog](https://github.com/tfgrid-studio/tfgrid-compose/blob/main/CHANGELOG.md)

---

**Next:** [What's Planned →](next.md)
