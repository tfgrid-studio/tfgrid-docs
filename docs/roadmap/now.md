# Now - Active Development

**🔨 What we're building right now**

---

## Completed Core Features ✅

The following building blocks of TFGrid Studio are now **shipped and available**:

### Patterns

- **single-vm pattern**
  - Simple VM deployments for development and internal services.
  - Great for AI agents, databases, and internal APIs.

- **gateway pattern**
  - Multi-VM deployments with public access and SSL.
  - Perfect for production web apps and SaaS.

- **k3s (Kubernetes) pattern**
  - Full Kubernetes cluster on ThreeFold Grid.
  - Suited for microservices and enterprise workloads.

### Apps

- **tfgrid-gitea**
  - Git hosting on ThreeFold Grid.
  - Ideal companion for AI/dev workflows.

- **tfgrid-ai-agent**
  - AI agent environment focused on coding and automation.

- **tfgrid-ai-stack**
  - Complete AI development stack (AI + Git + web UI).
  - From prompt to production in one deployment.

### Dashboard

- **TFGrid Studio Dashboard (tfgrid-compose dashboard / t dashboard)**
  - Local web dashboard on top of `tfgrid-compose`.
  - Visual management for apps, deployments, CLI commands, preferences, and logs.

### Developer Tooling

- **Custom Apps Framework**
  - Create your own deployable apps with `tfgrid-compose.yaml` manifests.
  - Deployment hooks (setup, configure, healthcheck).
  - Custom commands for app-specific operations.
  - See [Custom Apps Guide](../guides/custom-apps.md).

- **Versioned Tarball Releases**
  - Build and ship versioned release tarballs.
  - Atomic deployments with instant rollback.
  - In-place updates without VM recreation.
  - See [Tarball Releases Guide](../guides/tarball-releases.md).

- **Git Commit Versioning**
  - All apps and tools use Git commit hashes as primary version identifier.
  - Precise code traceability for every deployment.
  - Automatic version management without manual bumping.

---

## Try Current Features

Use the current CLI and dashboard features:

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

# Launch local dashboard
t dashboard
```

---

## Progress Updates

Follow development:
- [GitHub Project Board](https://github.com/orgs/tfgrid-studio/projects)
- [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- [Changelog](https://github.com/tfgrid-studio/tfgrid-compose/blob/main/CHANGELOG.md)

---

**Next:** [What's Planned →](next.md)
