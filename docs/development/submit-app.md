# Submitting Your App to TFGrid Registry

Share your application with the TFGrid community by submitting it to the official app registry.

## Overview

Getting your app verified means:
- ✅ Discoverable at [registry.tfgrid.studio](https://registry.tfgrid.studio)
- ✅ Listed in official registry
- ✅ Verified badge
- ✅ Increased visibility and trust
- ✅ Available via `tfgrid-compose up your-app` (v0.10.0+)

## Prerequisites

Before submitting:

1. **Working App**: Deploys successfully with tfgrid-compose
2. **Documentation**: Clear README with setup instructions
3. **App Manifest**: Valid `tfgrid-compose.yaml`
4. **Open Source**: Public repository with open source license (Apache 2.0 recommended)
5. **No Secrets**: No hardcoded credentials or API keys

## Required Files

Your repository must include:

```
your-app/
├── tfgrid-compose.yaml       # Required: App manifest
├── README.md                  # Required: Documentation
├── LICENSE                    # Required: Open source license
├── deployment/                # Recommended: Deployment scripts
│   ├── setup.sh
│   ├── configure.sh
│   └── healthcheck.sh
└── .gitignore                # Recommended: Ignore secrets
```

### Minimum tfgrid-compose.yaml

```yaml
name: your-app
version: 0.1.0
description: Brief description of your app

patterns:
  recommended: single-vm  # or gateway, k3s
  
resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096
    recommended: 8192
  disk:
    min: 50
    recommended: 100

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

See [App Manifest Reference](../reference/manifest.md) for full specification.

## Submission Process

### Step 1: Test Thoroughly

```bash
# Test local deployment
cd your-app
tfgrid-compose up .

# Verify all functionality
tfgrid-compose status
tfgrid-compose ssh

# Clean up
tfgrid-compose down
```

### Step 2: Fork App Registry

```bash
git clone https://github.com/YOUR-USERNAME/app-registry
cd app-registry
git checkout -b add-your-app
```

### Step 3: Add Your App Entry

Edit `registry/verified/community.yaml` (create if doesn't exist):

```yaml
apps:
  verified:
    - name: your-app-name
      repo: github.com/your-username/your-app-repo
      author: your-username
      description: Clear one-line description of your app
      pattern: single-vm  # or gateway, k3s
      status: beta  # beta or production
      version: v0.1.0
      verified: false  # Will be set to true after review
      tags:
        - tag1
        - tag2
        - tag3
      requirements:
        cpu: 2
        memory: 4GB
        disk: 50GB
      links:
        docs: https://github.com/your-username/your-app-repo
        repo: https://github.com/your-username/your-app-repo
```

### Step 4: Create Pull Request

```bash
git add registry/verified/community.yaml
git commit -m "feat: add YOUR-APP to verified apps"
git push origin add-your-app
```

Create PR on GitHub with:
- **Title**: `Add [Your App Name] to verified apps`
- **Description**: 
  - What does your app do?
  - Why should users try it?
  - Any special requirements or notes?

## Review Process

Our team will review your submission:

### 1. Code Review (1-3 days)
- Repository structure
- Security issues
- No hardcoded secrets
- Valid app manifest

### 2. Testing (1-2 days)
- Deploy app on test environment
- Verify all features work
- Check resource requirements
- Test cleanup process

### 3. Documentation Review (1 day)
- README clarity
- Setup instructions
- Prerequisites listed
- Troubleshooting info

### 4. Approval or Feedback
- **If approved**: Merged, `verified: true` set
- **If changes needed**: Comments on PR

**Total Timeline**: ~5-7 days

## Verification Criteria

### Must Have ✅

- [ ] Valid `tfgrid-compose.yaml`
- [ ] Clear README with setup instructions
- [ ] Open source license (Apache 2.0 recommended)
- [ ] No hardcoded secrets or credentials
- [ ] Deploys successfully
- [ ] Cleans up properly with `tfgrid-compose down`
- [ ] Works with current tfgrid-compose version

### Should Have 📋

- [ ] Comprehensive documentation
- [ ] Example configurations
- [ ] Troubleshooting guide
- [ ] Security best practices documented
- [ ] Resource requirements documented
- [ ] Regular maintenance commitment

### Nice to Have 🌟

- [ ] Automated tests
- [ ] CI/CD pipeline
- [ ] Multiple pattern support
- [ ] Configuration examples
- [ ] Video tutorial or demo

## After Verification

Once approved:

1. **Merged to Registry**: Your app appears in official registry
2. **Discoverable**: Listed at registry.tfgrid.studio
3. **Verified Badge**: Shows as verified
4. **CLI Integration**: Available via `tfgrid-compose up your-app` (v0.10.0+)
5. **Responsibility**: Maintain app, respond to issues, keep updated

## Maintaining Your App

### Updates

When you update your app:
1. Update version in `tfgrid-compose.yaml`
2. Create PR to update version in registry
3. Include changelog in PR description

### Support

- Respond to user issues in your repo
- Keep documentation updated
- Fix bugs promptly
- Announce breaking changes

### Revocation

Verification may be revoked if:
- App becomes unmaintained
- Security vulnerabilities not fixed
- Breaking changes without migration guide
- Violates community guidelines

## App Development Guidelines

### Security Best Practices

- ✅ Never hardcode credentials
- ✅ Use environment variables for secrets
- ✅ Document security requirements
- ✅ Keep dependencies updated
- ✅ Follow principle of least privilege

### Resource Requirements

- Document minimum and recommended resources
- Test with minimal resources
- Provide configuration for different sizes
- Note any resource-intensive operations

### Documentation

Your README should include:
- **Overview**: What does the app do?
- **Prerequisites**: Requirements before deployment
- **Quick Start**: Minimal steps to get running
- **Configuration**: Available options
- **Usage**: How to use the app
- **Troubleshooting**: Common issues
- **License**: Open source license

### Patterns

Choose the appropriate pattern:

**single-vm**: Single VM, private networking
- Databases, internal APIs, development environments

**gateway**: Multi-VM with public access and SSL
- Web applications, SaaS products, e-commerce

**k3s**: Kubernetes cluster
- Microservices, cloud-native apps, enterprise scale

## Examples

See official apps for reference:
- [tfgrid-ai-agent](https://github.com/tfgrid-studio/tfgrid-ai-agent) - Single-VM pattern
- [tfgrid-gateway](https://github.com/tfgrid-studio/tfgrid-gateway) - Gateway pattern
- [tfgrid-k3s](https://github.com/tfgrid-studio/tfgrid-k3s) - K3s pattern

## Get Help

- **Questions**: [Discussions](https://github.com/tfgrid-studio/app-registry/discussions)
- **Issues**: [Report problems](https://github.com/tfgrid-studio/app-registry/issues)
- **Documentation**: [TFGrid Docs](https://docs.tfgrid.studio)

---

**Thank you for contributing to the TFGrid ecosystem!** 🎉

**Ready to submit?** [Create your PR →](https://github.com/tfgrid-studio/app-registry)
