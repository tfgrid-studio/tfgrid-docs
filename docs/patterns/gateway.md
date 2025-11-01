# Gateway Pattern

**Multi-VM with public access and SSL for production web apps**

The gateway pattern deploys a multi-VM architecture with a reverse proxy gateway that provides public IPv4 access, automatic SSL/TLS certificates, and load balancing for your backend services.

---

## Overview

```
Internet → [Gateway VM] → [Backend VMs]
        (Public IPv4)   (Private Network)
        (SSL/TLS)       (Your App + DB)
```

**Perfect For:**

- Production websites & web apps
- E-commerce sites
- SaaS applications
- Anything needing public HTTPS access

---

## Quick Start

```bash
tfgrid-compose up my-saas --pattern=gateway --domain=myapp.com
```

**Deploy time:** 5-7 minutes  
**Cost:** $30-100/month

---

## Features

- 🔒 **Free SSL certificates** - Automatic Let's Encrypt SSL/TLS
- 🌐 **Public IPv4 included** - Direct internet access
- ⚖️ **Load balancing** - Distribute traffic across backends
- ❤️ **Health checks** - Automatic failover for reliability
- 🔄 **Reverse proxy** - Nginx-based gateway with custom configs
- 🛡️ **Private backend network** - Backends only accessible via gateway

---

## Example Deployment

Deploy a SaaS application with SSL:

```bash
$ tfgrid-compose up my-saas --pattern=gateway --domain=myapp.com

✨ Live with SSL in 5 minutes!
```

The gateway pattern will:
1. Create a gateway VM with public IPv4
2. Deploy your backend application VMs
3. Configure reverse proxy and SSL
4. Set up health checks and load balancing

---

## Architecture

### Gateway VM
- Public IPv4 address
- Nginx reverse proxy
- Let's Encrypt SSL automation
- Load balancer
- Health check monitoring

### Backend VMs
- Private network only
- Your application code
- Database services
- Internal APIs

### Network Flow
```
User → Public IP → Gateway VM → WireGuard/Mycelium → Backend VMs
```

---

## Configuration

Example `tfgrid-compose.yaml` for gateway pattern:

```yaml
name: my-webapp
pattern: gateway

gateway:
  domain: myapp.com
  ssl: true
  
backends:
  - name: app
    cpu: 2
    memory: 4096
    port: 3000
  
  - name: db
    cpu: 2
    memory: 8192
    private: true
```

---

## Use Cases

### Production Web Apps
Deploy full-stack web applications with SSL:

```bash
tfgrid-compose up my-webapp --pattern=gateway --domain=example.com
```

### E-commerce Sites
Run online stores with secure payments:

```bash
tfgrid-compose up my-store --pattern=gateway --domain=store.com
```

### SaaS Applications
Launch multi-tenant SaaS products:

```bash
tfgrid-compose up my-saas --pattern=gateway --domain=app.mycompany.com
```

---

## SSL/TLS Configuration

The gateway pattern automatically handles SSL certificate:

1. **Automatic issuance** - Let's Encrypt certificates on deployment
2. **Auto-renewal** - Certificates renew automatically
3. **HTTPS redirect** - HTTP traffic automatically redirects to HTTPS
4. **Modern security** - TLS 1.2+ with strong cipher suites

---

## Load Balancing

When you deploy multiple backend instances:

```yaml
backends:
  - name: app-1
    port: 3000
  - name: app-2
    port: 3000
  - name: app-3
    port: 3000
```

The gateway automatically:
- Distributes traffic across all instances
- Performs health checks
- Routes traffic away from unhealthy instances
- Provides zero-downtime deployments

---

## Full Documentation

For complete implementation details, see the [gateway pattern source](https://github.com/tfgrid-studio/tfgrid-compose/tree/main/patterns/gateway).

---

## Next Steps

- [Deploy your first gateway app](../getting-started/quickstart.md)
- [Learn about Single-VM pattern](single-vm.md) for simpler deployments
- [Explore K3s pattern](k3s.md) for cloud-native applications
