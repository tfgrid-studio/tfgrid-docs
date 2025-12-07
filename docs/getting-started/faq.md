# Frequently Asked Questions

Common questions about TFGrid Studio and tfgrid-compose.

---

## General

### What is TFGrid Studio?

TFGrid Studio is a complete development platform for deploying applications on ThreeFold Grid. It provides tools like `tfgrid-compose` (CLI), official apps, and documentation to make decentralized deployments simple.

### What is tfgrid-compose?

`tfgrid-compose` is the main CLI tool that orchestrates deployments on ThreeFold Grid. Think of it like docker-compose, but for decentralized infrastructure.

### Is TFGrid Studio free?

Yes! The core tools (tfgrid-compose, apps, documentation) are open source under Apache 2.0 license. You only pay for ThreeFold Grid resources (compute, storage, network).

### What's the difference between TFGrid Studio and ThreeFold Grid?

- **ThreeFold Grid** = The decentralized infrastructure (compute, storage, network)
- **TFGrid Studio** = Tools and apps that make it easy to deploy on ThreeFold Grid

---

## Getting Started

### How do I install tfgrid-compose?

```bash
curl -sSL install.tfgrid.studio/install.sh | sh
```

### Do I need a ThreeFold wallet?

Yes. You need:
1. ThreeFold Connect app (mobile wallet)
2. TFT tokens to pay for resources
3. Completed KYC verification

See the [ThreeFold Setup Guide](threefold-setup.md) for details.

### How much does it cost to run an app?

Costs vary by resources. Approximate monthly costs:
- Small VM (1 CPU, 2GB RAM): $3-5
- Medium VM (2 CPU, 4GB RAM): $8-12
- AI Agent: $5-10

### Which apps are available?

Official apps:
- **tfgrid-ai-stack** - AI + Git + Gateway (flagship)
- **tfgrid-ai-agent** - AI coding assistant
- **tfgrid-gitea** - Self-hosted Git
- **tfgrid-wordpress** - WordPress + Caddy + MariaDB
- **tfgrid-nextcloud** - File sync & collaboration
- **tfgrid-erpnext** - Business ERP

Browse all at [registry.tfgrid.studio](https://registry.tfgrid.studio)

---

## Deployment

### What deployment patterns are available?

| Pattern | Use Case |
|---------|----------|
| **single-vm** | Simple apps, development, internal services |
| **gateway** | Production apps with SSL and custom domains |
| **k3s** | Kubernetes clusters for microservices |

### How do I deploy with a custom domain?

```bash
tfgrid-compose up my-app --pattern gateway --domain myapp.com
```

The gateway pattern handles SSL certificates automatically via Let's Encrypt.

### Can I use my own DNS provider?

Yes! tfgrid-compose supports automatic DNS configuration for:
- Cloudflare
- Name.com
- Namecheap

Or you can manually point your domain to the deployment IP.

### How do I SSH into my deployment?

```bash
tfgrid-compose ssh <app-name>
```

### How do I view logs?

```bash
tfgrid-compose logs <app-name>
```

### How do I stop/destroy a deployment?

```bash
tfgrid-compose down <app-name>
```

---

## Troubleshooting

### Deployment failed - what do I check?

1. **TFT balance** - Do you have enough tokens?
2. **KYC status** - Is verification complete?
3. **Seed phrase** - Is it configured correctly?
4. **Network** - Can you reach ThreeFold Grid?

```bash
tfgrid-compose login --check
```

### Can't connect to my VM

1. Check WireGuard is running: `wg show`
2. Verify deployment status: `tfgrid-compose status`
3. Check the VM IP: `tfgrid-compose info <app>`

### SSL certificate not working

1. Ensure DNS points to your gateway IP
2. Wait a few minutes for Let's Encrypt
3. Check Caddy logs: `tfgrid-compose logs caddy`

---

## Development

### Can I create my own apps?

Yes! Create a `tfgrid-compose.yaml` manifest in your app directory. See the [Custom Apps Guide](../guides/custom-apps.md).

### How do I submit an app to the registry?

See [Submit Apps](../development/submit-app.md) for the process.

### Where do I report bugs?

- **GitHub Issues** in the relevant repository
- **Security issues** → [security@tfgrid.studio](mailto:security@tfgrid.studio)

### How do I contribute?

See our [Contributing Guide](../community/contributing.md).

---

## Still have questions?

- **Documentation:** [docs.tfgrid.studio](https://docs.tfgrid.studio)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)
- **Contact:** [tfgrid.studio/contact](https://tfgrid.studio/contact)
