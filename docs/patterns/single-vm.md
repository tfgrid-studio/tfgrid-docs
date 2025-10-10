# Single-VM Pattern

**Simple VM deployment for development and internal services**

The single-vm pattern is the simplest deployment pattern in TFGrid Compose. It creates a single virtual machine with private networking accessible via WireGuard VPN.

---

## Overview

```
┌──────────────────────────┐
│    Your Application      │
│    Private Networking    │
│    🔒 WireGuard Access   │
└──────────────────────────┘
```

**Perfect For:**
- AI agents & coding environments
- Databases (PostgreSQL, MongoDB, Redis)
- Internal services & APIs
- Development environments

---

## Quick Start

```bash
tfgrid-compose up tfgrid-ai-agent --pattern=single-vm
```

**Deploy time:** 2-3 minutes  
**Cost:** $10-30/month

---

## Features

- ✅ **Isolated VM environment** - Complete isolation from your local machine
- ✅ **Private networking** - Secure WireGuard VPN access
- ✅ **Fast deployment** - Up and running in 2-3 minutes
- ✅ **Cost-effective** - Starting at $10/month
- ✅ **No public IP needed** - Perfect for internal services

---

## Example: TFGrid AI Agent

Deploy an isolated AI coding environment:

```bash
$ tfgrid-compose up tfgrid-ai-agent --pattern=single-vm

✨ Deployed! AI agent ready in 2 minutes.
```

Access your AI agent:

```bash
$ tfgrid-compose ssh tfgrid-ai-agent
```

---

## Configuration

The single-vm pattern uses the following default configuration:

- **CPU:** 2 cores
- **Memory:** 4 GB RAM
- **Storage:** 50 GB SSD
- **Network:** WireGuard VPN + Mycelium overlay

You can customize these in your `tfgrid-compose.yaml` manifest.

---

## Use Cases

### Development Environments
Set up isolated development environments that won't affect your local machine:

```bash
tfgrid-compose up my-dev-env --pattern=single-vm
```

### Databases
Deploy databases with private network access:

```bash
tfgrid-compose up my-postgres --pattern=single-vm
```

### Internal APIs
Run internal services accessible only via VPN:

```bash
tfgrid-compose up my-api --pattern=single-vm
```

---

## Full Documentation

For complete implementation details, see the [single-vm pattern source](https://github.com/tfgrid-studio/tfgrid-compose/tree/main/patterns/single-vm).

---

## Next Steps

- [Deploy your first app](../getting-started/quickstart.md)
- [Learn about the Gateway pattern](gateway.md) for public web apps
- [Explore K3s pattern](k3s.md) for Kubernetes deployments
