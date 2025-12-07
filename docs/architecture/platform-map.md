# TFGrid Studio Platform Map

**High-level overview of how TFGrid Studio components, repositories, and websites fit together.**

This page complements the low-level `Architecture` docs by giving a product-level map of:

- Core CLI and patterns
- Official apps (AI Agent, AI Stack, Gitea)
- Registry and install flows
- Documentation and marketing sites
- Dashboards (local vs planned hosted)

---

## 1. High-Level Picture

```text
Developer / User
   │
   ├─ CLI: tfgrid-compose  ("t")
   │    ├─ Patterns: single-vm, gateway, k3s
   │    └─ Apps: tfgrid-ai-agent, tfgrid-ai-stack, tfgrid-gitea, community apps
   │
   ├─ Local Dashboard: tfgrid-compose dashboard
   │    └─ Visual wrapper over the same CLI + registry + state
   │
   ├─ Documentation: docs.tfgrid.studio
   │
   └─ Web Properties:
        - tfgrid.studio            (marketing)
        - install.tfgrid.studio    (installer)
        - registry.tfgrid.studio   (app catalogue)
```

At the center is **`tfgrid-compose`**, which knows how to:

- Read app manifests (`tfgrid-compose.yaml`)
- Apply patterns (single-vm, gateway, k3s)
- Deploy and manage apps on ThreeFold Grid

Everything else (apps, registry, dashboard, docs, websites) is built on top of this core.

---

## 2. Core CLI and Patterns

### 2.1 `tfgrid-compose` CLI

- Repository: `tfgrid-compose`
- Role: universal deployer/orchestrator for ThreeFold Grid.
- Responsibilities:
  - Parse commands (`up`, `down`, `status`, `logs`, `ssh`, `exec`, `create`, `run`, `publish`, etc.).
  - Load context files (`.tfgrid-compose.yaml`).
  - Orchestrate Terraform/OpenTofu, WireGuard/Mycelium, Ansible, and app hooks.
  - Maintain state in `.tfgrid-compose/` and `deployments.yaml`.

Shortcuts:

- Default shortcut `tfgrid` is created at install time.
- Users can create their own, for example `t`, via `tfgrid-compose shortcut t`.

### 2.2 Patterns

Patterns live inside `tfgrid-compose/patterns/` and define *infrastructure + platform* for apps:

- **single-vm**  
  - One VM on the grid.  
  - WireGuard + optional Mycelium networking.  
  - Used by: `tfgrid-ai-agent`, `tfgrid-ai-stack`, `tfgrid-gitea`, and many future apps.

- **gateway**  
  - Public IPv4 gateway VM + private backend VMs.  
  - Nginx/HAProxy, SSL (Let[200~'s Encrypt), path-based routing.  
  - Ideal for production web apps.

- **k3s**  
  - K3s Kubernetes clusters with control plane, workers, and management node.  
  - MetalLB, Ingress, storage, etc.

Low-level details of these patterns live in `Architecture → System Architecture` and pattern-specific docs.

---

## 3. Official Apps on Top of the Patterns

### 3.1 TFGrid AI Agent

- Repo: `tfgrid-ai-agent`
- Pattern: `single-vm`
- Purpose: isolated AI-powered coding environment.
- Key CLI flows:
  - `tfgrid-compose up tfgrid-ai-agent` – deploy AI agent VM.
  - Use AI agent scripts or `tfgrid-compose exec` for project loops.

### 3.2 TFGrid AI Stack

- Repo: `tfgrid-ai-stack`
- Pattern: `single-vm` (stacked services on one VM).
- Components:
  - Nginx reverse proxy (`/`, `/git/`, `/api/`).
  - AI Agent service (Node.js + `qwen-cli`).
  - Gitea Git server (`/git/`).
- Key CLI flows:
  - `tfgrid-compose up tfgrid-ai-stack` – deploy full AI + Git + gateway stack.
  - `tfgrid-compose create` – create AI-generated projects.
  - `tfgrid-compose run` / `monitor` – run and observe AI loops.
  - `tfgrid-compose publish` – expose projects as websites.

**URL model after `publish`:**

- Git: `http://<ip-or-mycelium>/git/<org>/<project>`
- Web: `http://<ip-or-mycelium>/web/<org>/<project>`

Where `<ip-or-mycelium>` can be a WireGuard IP (e.g. `10.1.3.2`) or a Mycelium address.

### 3.3 TFGrid Gitea

- Repo: `tfgrid-gitea`
- Pattern: `single-vm`
- Purpose: standalone Gitea deployment for Git hosting.
- Key CLI flows:
  - `tfgrid-compose up tfgrid-gitea`
  - Gitea web UI at `http://<vm-ip>:3000` or proxied behind a gateway.

### 3.4 App Registry Integration

- Repo: `tfgrid-registry`
- Role: canonical list of apps that `tfgrid-compose` can deploy.
- Powers CLI commands like:
  - `tfgrid-compose search`
  - `tfgrid-compose info <app>`
  - `tfgrid-compose up tfgrid-ai-agent`

CI in `tfgrid-registry` keeps app versions in sync and validates registry entries.

---

## 4. Registry and Install Flow

### 4.1 Install Script

- Repo: `tfgrid-install`
- Site: `https://install.tfgrid.studio`
- Purpose: One-line installer for `tfgrid-compose`.

Typical flow:

```bash
curl -sSL install.tfgrid.studio | sh
```

This script:

- Clones `tfgrid-compose`.
- Runs `make install`.
- Sets up the CLI in your PATH and creates a default shortcut.

### 4.2 App Registry

- Repo: `tfgrid-registry`
- Holds `registry/apps.yaml` and supporting schemas.
- Used by `tfgrid-compose` to:
  - Discover apps.
  - Pull their metadata and recommended patterns.

### 4.3 Registry Website

- Repo: `tfgrid-registry-website`
- Site: `https://registry.tfgrid.studio`
- Small frontend for browsing registry apps in a browser, complementary to the CLI.

---

## 5. Web and Documentation Sites

### 5.1 Marketing Site

- Repo: `tfgrid-website`
- Site: `https://tfgrid.studio`
- Role: marketing/landing site for TFGrid Studio.
- Highlights:
  - Product overview and value proposition.
  - Patterns and official apps.
  - Pricing / plans (for future commercial offerings).

### 5.2 Documentation Site

- Repo: `tfgrid-docs`
- Site: `https://docs.tfgrid.studio`
- Role: canonical documentation for:
  - Getting started and installation.
  - Patterns (single-vm, gateway, k3s).
  - Guides for `tfgrid-ai-agent`, `tfgrid-ai-stack`, `tfgrid-gitea`.
  - Architecture and roadmap.

### 5.3 Install & Registry Sites

- `https://install.tfgrid.studio`  → repo `tfgrid-install` (one-line installer script).
- `https://registry.tfgrid.studio` → repo `tfgrid-registry-website` (app catalogue frontend).

---

## 6. Dashboards

### 6.1 Local Dashboard (Today)

- Implementation lives under the `dashboard/` folder in `tfgrid-compose`.
- Documented in `Guides → TFGrid Compose Dashboard`.
- Launched with:

```bash
# CLI
tfgrid-compose dashboard
# or via shortcut
t dashboard
```

Characteristics:

- Runs completely on the user[200~'s machine.
- Talks to the local `tfgrid-compose` binary and reads the same registry and state files.
- Provides a browser UI for:
  - Browsing apps and deployments.
  - Running CLI commands via forms.
  - Managing AI Stack projects (`create`, `run`, `publish`).

### 6.2 Hosted Portal (SaaS)

The hosted portal consists of two components:

**Frontend:**
- Repo: `tfgrid-portal`
- Site: `https://portal.tfgrid.studio` (planned)
- Stack: React + Vite SPA
- Purpose: Web dashboard for managing workspaces

**Backend:**
- Repo: `tfgrid-control-plane`
- Stack: Node.js + Fastify + Postgres
- Purpose: API for users, billing (Stripe), workspace orchestration
- Calls `tfgrid-compose` to deploy workspaces on ThreeFold Grid

```
┌─────────────────┐      ┌─────────────────────┐
│  tfgrid-portal  │ ───► │ tfgrid-control-plane │ ───► tfgrid-compose ───► ThreeFold Grid
│   (Frontend)    │ API  │     (Backend API)    │
└─────────────────┘      └─────────────────────┘
```

Details of hosted pricing, billing, and control-plane architecture are tracked in the `tfgrid-internal` repository.

---

## 7. Repositories Overview

This table summarizes the main repositories and how they relate to the platform map above.

| Repository        | Type          | Role / Notes                                      |
|-------------------|---------------|---------------------------------------------------|
| `tfgrid-compose`  | Core CLI      | Universal deployer & pattern engine               |
| `tfgrid-ai-agent` | App           | Standalone AI coding agent on single-vm           |
| `tfgrid-ai-stack` | App           | AI + Git + gateway stack with `/git` and `/web`   |
| `tfgrid-gitea`    | App           | Standalone Gitea Git server                       |
| `tfgrid-registry` | Infra         | App registry consumed by `tfgrid-compose`         |
| `tfgrid-registry-website` | Site    | Frontend for registry.tfgrid.studio              |
| `tfgrid-install`  | Site/Script   | One-line installer at install.tfgrid.studio       |
| `tfgrid-docs`     | Docs          | Documentation for all components (docs.tfgrid.studio) |
| `tfgrid-website`  | Site          | Marketing site at tfgrid.studio                  |
| `tfgrid-portal`   | App (planned) | Hosted web portal (SaaS frontend)                 |
| `tfgrid-control-plane` | App (planned) | Backend API for portal (users, billing)      |
| `tfgrid-marketplace` | App (planned) | App marketplace                                  |
| `tfgrid-enterprise`  | App (planned) | Enterprise extensions & packaging                |
| `community`       | Meta          | Community docs, discussions, ecosystem table      |
| `tfgrid-internal` | Meta/Internal | Internal design docs, roadmaps, experiments       |

For a deeper, low-level view of how the CLI works internally, see:

- `Architecture → System Architecture`
- `Architecture → Source Repositories`
