# App Registry

The TFGrid App Registry catalogs official and verified community applications that can be deployed using `tfgrid-compose`. Applications listed here are discoverable at [registry.tfgrid.studio](https://registry.tfgrid.studio) and can be deployed with a simple command:

```bash
# Deploy official app (v0.10.0+)
tfgrid-compose up tfgrid-ai-agent

# Deploy community app
tfgrid-compose up username/app-name

# Deploy from any git URL
tfgrid-compose up https://gitlab.com/org/app
```

## How It Works

### For Users

1. **Browse Apps**: Visit [registry.tfgrid.studio](https://registry.tfgrid.studio)
2. **Deploy**: Run `tfgrid-compose up <app-name>` (v0.10.0+)
3. **Done**: App is automatically cloned and deployed

**No configuration needed** - apps include their own `tfgrid-compose.yaml`.

### For Developers

Apps in the registry:

- Include `tfgrid-compose.yaml` in their repository
- Follow [app guidelines](https://github.com/tfgrid-studio/app-registry/blob/main/docs/app-guidelines.md)
- Can be submitted for verification via PR

## Official Apps

Maintained by TFGrid Studio:

### tfgrid-ai-agent

AI coding agent with Qwen integration.

```bash
tfgrid-compose up tfgrid-ai-agent
```

**Details:**
- **Pattern**: single-vm
- **Repo**: [tfgrid-studio/tfgrid-ai-agent](https://github.com/tfgrid-studio/tfgrid-ai-agent)
- **Docs**: [AI Agent Guide](ai-agent.md)

## Community Apps

Community-contributed apps that have passed verification:

> Ready to share your app? Submit it for verification!  
> See [Submission Guidelines →](https://github.com/tfgrid-studio/app-registry/blob/main/docs/submit-app.md)

## Using Apps

### Deploy Official App

```bash
# Deploy with auto-generated name
tfgrid-compose up tfgrid-ai-agent
# → Deployment name: tfgrid-ai-agent

# Deploy with custom name
tfgrid-compose up tfgrid-ai-agent --name=dev
# → Deployment name: dev
```

### Deploy Community App (Unverified)

```bash
# From GitHub
tfgrid-compose up username/repo-name

# From any git URL
tfgrid-compose up https://gitlab.com/org/app
```

⚠️ **Security Note**: Always review code before deploying unverified apps!

### List Available Apps

```bash
# Search registry
tfgrid-compose search

# Search by keyword
tfgrid-compose search ai

# Get app details
tfgrid-compose info tfgrid-ai-agent
```

### Manage Cached Apps

```bash
# Pull/update app
tfgrid-compose pull tfgrid-ai-agent

# List cached apps
tfgrid-compose cache

# Clean unused apps
tfgrid-compose prune
```

## App Structure

All apps must include `tfgrid-compose.yaml`:

```yaml
name: my-app
version: 1.0.0
description: My application

patterns:
  recommended: single-vm
  
resources:
  cpu:
    recommended: 4
  memory:
    recommended: 8192
  disk:
    recommended: 100

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

See [App Manifest Reference](../development/app-manifest.md) for full specification.

## Registry Structure

Apps are organized by verification status:

### Official Apps
- Maintained by TFGrid Studio
- Full support and documentation
- Production-ready

### Verified Apps
- Community-contributed
- Code reviewed by TFGrid Studio
- Security checked
- Documentation verified

### Unverified Apps
- Any GitHub/GitLab repo
- Use at your own risk
- Not listed in registry
- Access via direct URL

## Submitting Your App

Want to share your app with the community?

1. **Develop Your App**
   - Include `tfgrid-compose.yaml`
   - Add deployment scripts
   - Write comprehensive README

2. **Test Locally**
   ```bash
   tfgrid-compose up ./my-app
   ```

3. **Submit for Verification**
   - Fork [tfgrid-studio/app-registry](https://github.com/tfgrid-studio/app-registry)
   - Add your app to `registry/verified/community.yaml`
   - Create pull request

4. **Review Process**
   - Code review (~3 days)
   - Security check
   - Testing
   - Documentation review

5. **Approved!**
   - Merged to registry
   - Appears in `tfgrid-compose search`
   - Listed on registry.tfgrid.studio

**Full Guide**: [How to Submit →](https://github.com/tfgrid-studio/app-registry/blob/main/docs/submit-app.md)

## App Cache Location

Apps are cached locally:

```bash
~/.config/tfgrid-compose/
├── apps/                    # Cached app repositories
│   ├── tfgrid-ai-agent/
│   └── other-apps/
├── deployments/             # Your deployments
└── registry.yaml            # Cached registry
```

## Version Pinning

Pin apps to specific versions:

```bash
# Latest version (default)
tfgrid-compose up tfgrid-ai-agent

# Specific version
tfgrid-compose up tfgrid-ai-agent:v0.9.0

# Specific branch
tfgrid-compose up tfgrid-ai-agent:develop
```

## Registry API

For automation and integrations:

```bash
# JSON API
curl https://registry.tfgrid.studio/api/apps.json

# YAML API
curl https://registry.tfgrid.studio/api/apps.yaml

# App details
curl https://registry.tfgrid.studio/api/apps/tfgrid-ai-agent.json
```

## Security

### Official Apps
- ✅ Audited by TFGrid Studio
- ✅ Regular security updates
- ✅ Full support

### Verified Apps
- ✅ Code reviewed
- ✅ Security checked
- ✅ No hardcoded secrets
- ✅ Documentation verified

### Unverified Apps
- ⚠️ Use at your own risk
- ⚠️ Review code before deploying
- ⚠️ Check for secrets/malicious code

**Best Practice**: Always inspect app code before deployment:

```bash
# Clone and inspect
git clone https://github.com/username/app
cd app
# Review code, check for secrets
less deployment/setup.sh

# Deploy if satisfied
tfgrid-compose up ./app
```

## Resources

- **Registry Website**: [registry.tfgrid.studio](https://registry.tfgrid.studio) ✅ Live
- **Registry Data**: [tfgrid-studio/app-registry](https://github.com/tfgrid-studio/app-registry)
- **Submit Apps**: [Submission Guide](https://github.com/tfgrid-studio/app-registry/blob/main/docs/submit-app.md)
- **App Guidelines**: [Development Guidelines](https://github.com/tfgrid-studio/app-registry/blob/main/docs/app-guidelines.md)

---

**Registry Deployment**: v0.10.0 (CLI integration coming soon)  
**Browse Apps Now**: [registry.tfgrid.studio](https://registry.tfgrid.studio)
