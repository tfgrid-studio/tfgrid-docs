# App Registry

The TFGrid App Registry catalogs official and verified community applications that can be deployed using `tfgrid-compose`. Applications listed here are discoverable at [registry.tfgrid.studio](https://registry.tfgrid.studio) and can be deployed with a simple command:

```bash
# Deploy official app (v0.13.4+)
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

### Git Commit Versioning

TFGrid Compose now uses **Git commit hashes as the primary version identifier** for precise code traceability:

```bash
# Example deployment showing Git commit version
✅ Application loaded: tfgrid-ai-stack 24c9148
ℹ Git commit: 24c9148
ℹ Last updated: 2025-11-11 22:47:49
ℹ Branch: main
ℹ Repository: https://github.com/tfgrid-studio/tfgrid-ai-stack.git
```

**Benefits:**
- ✅ **Precise versioning**: Every deployment has a unique, immutable version
- ✅ **Automatic version management**: No manual version bumping required  
- ✅ **Instant traceability**: Know exactly which code is running
- ✅ **Better debugging**: Can track exactly what changed between deployments
- ✅ **Consistency**: All TFGrid components use the same versioning approach

### For Developers

Apps in the registry:

- Include `tfgrid-compose.yaml` in their repository
- Follow [app guidelines](https://github.com/tfgrid-studio/app-registry/blob/main/docs/app-guidelines.md)
- Can be submitted for verification via PR

## Official Apps

Maintained by TFGrid Studio:

### tfgrid-ai-agent

AI coding agent with Qwen integration and loop technique for safe AI development.

```bash
tfgrid-compose up tfgrid-ai-agent
```

**Details:**

- **Pattern**: single-vm
- **Versioning**: Git commit-based (primary) with semantic fallback
- **Current Version**: Uses latest Git commit (e.g., `0e91178`)
- **Status**: Production Ready
- **Repo**: [tfgrid-studio/tfgrid-ai-agent](https://github.com/tfgrid-studio/tfgrid-ai-agent)
- **Docs**: [AI Agent Guide](ai-agent.md)
- **Requirements**: 4 CPU, 8GB RAM, 100GB disk

> **Git Commit Versioning**: Each deployment shows exact Git commit hash for precise code traceability

### tfgrid-gitea

Self-hosted Git service with web interface - perfect for AI agent repositories.

```bash
tfgrid-compose up tfgrid-gitea
```

**Details:**
- **Pattern**: single-vm
- **Versioning**: Git commit-based (primary) with semantic fallback
- **Current Version**: Uses latest Git commit (e.g., `4a7a91d`)
- **Status**: Production Ready
- **Repo**: [tfgrid-studio/tfgrid-gitea](https://github.com/tfgrid-studio/tfgrid-gitea)
- **Docs**: [Gitea Guide](gitea.md)
- **Requirements**: 2 CPU, 4GB RAM, 50GB disk

> **Git Commit Versioning**: Each deployment shows exact Git commit hash for precise code traceability

## Community Apps

Community-contributed apps that have passed verification:

> Ready to share your app? Submit it for verification!  
> See [Submission Guidelines →](../development/submit-app.md)

## Using Apps

### Deploy Official App

```bash
# Deploy with auto-generated name
tfgrid-compose up tfgrid-ai-agent
# → Deployment name: tfgrid-ai-agent

# Deploy with custom name (if supported)
tfgrid-compose up tfgrid-ai-agent --name=my-agent
# → Deployment name: my-agent
```

### Search and Discover Apps

```bash
# List all available apps
tfgrid-compose search

# Search by keyword
tfgrid-compose search ai
tfgrid-compose search git

# Search by tag
tfgrid-compose search --tag development
tfgrid-compose search --tag ai
```

### Deploy Community App (Unverified)

```bash
# From GitHub
tfgrid-compose up username/repo-name

# From any git URL
tfgrid-compose up https://gitlab.com/org/app
```

⚠️ **Security Note**: Always review code before deploying unverified apps!

### Get App Information

```bash
# Get detailed app information
tfgrid-compose info tfgrid-ai-agent

# View app manifest and requirements
# Shows: version, resources, patterns, documentation
```

### Enhanced Cache Management

TFGrid Compose includes an advanced cache system with Git commit-based version tracking:

```bash
# Show cache health overview
t cache status

# List all cached apps with Git commit info
t cache list
# Example output:
# ✅ tfgrid-ai-stack (24c9148)
#     Last updated: 2025-11-11 22:47:49
# ⚠️ tfgrid-ai-agent (0e91178) - [needs update]

# Check for apps needing updates
t cache outdated

# Auto-refresh stale apps
t cache refresh

# Validate cache integrity
t cache validate tfgrid-ai-stack

# Clear specific app cache
t cache clear tfgrid-ai-stack

# Clear all cache
t cache clear --all
```

**Cache Features:**
- **Git Commit Tracking**: Each cached app shows exact Git commit hash
- **Smart Invalidation**: Cache automatically updates when Git commits change
- **Health Monitoring**: Shows cache status (healthy, stale, invalid, not cached)
- **Enhanced Validation**: Detailed syntax checking with exact line numbers
- **Rate Limiting Protection**: GitHub API rate limiting with retry logic

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
- ✅ Maintained by TFGrid Studio team
- ✅ Full support and documentation
- ✅ Production-ready and tested
- ✅ Regular security updates
- ✅ Example: `tfgrid-ai-agent`, `tfgrid-gitea`

### Community Apps (Future)
- ✅ Community-contributed and maintained
- ✅ Code reviewed by TFGrid Studio team
- ✅ Security checked and approved
- ✅ Documentation verified
- ✅ Listed in registry with "Community" badge

### Unverified Apps
- ⚠️ Any public GitHub/GitLab repository
- ⚠️ Use at your own risk
- ⚠️ Not listed in official registry
- ⚠️ Deploy via direct URL: `tfgrid-compose up https://github.com/user/repo`

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

**Full Guide**: [How to Submit →](../development/submit-app.md)

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
- **Submit Your App**: [Submission Guide](../development/submit-app.md)
- **App Manifest**: [Manifest Reference](../reference/manifest.md)

---

**Registry Status**: ✅ Active (v0.13.4 CLI integration complete)
**Browse Apps Now**: [registry.tfgrid.studio](https://registry.tfgrid.studio)
**Official Apps**: 2 (tfgrid-ai-agent v0.3.0, tfgrid-gitea v1.0.0)
**Community Apps**: Coming soon - submit yours!
