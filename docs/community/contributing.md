# Contributing to TFGrid Studio

**Welcome!** We're excited that you're interested in contributing to TFGrid Studio. This guide will help you get started.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [How to Contribute](#how-to-contribute)
3. [Development Workflow](#development-workflow)
4. [Code Standards](#code-standards)
5. [Testing](#testing)
6. [Documentation](#documentation)
7. [Community](#community)

---

## Getting Started

### Prerequisites

**For tfgrid-compose development:**

- Bash 4.0+
- Git
- OpenTofu or Terraform
- Ansible
- Basic understanding of shell scripting

**For documentation:**

- Markdown knowledge
- Git
- Python 3.8+ (for local MkDocs preview)

### Set Up Development Environment

```bash
# Clone the repository
git clone https://github.com/tfgrid-studio/tfgrid-compose
cd tfgrid-compose

# Install prerequisites
# Ubuntu/Debian
sudo apt update
sudo apt install ansible git

# Install OpenTofu (recommended)
curl -L https://get.opentofu.org/install-opentofu.sh | sudo bash

# Test your setup
./cli/tfgrid-compose --version
```

### First Time Setup

1. **Fork the repository** on GitHub
2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR-USERNAME/tfgrid-compose
   cd tfgrid-compose
   ```
3. **Add upstream remote:**
   ```bash
   git remote add upstream https://github.com/tfgrid-studio/tfgrid-compose
   ```
4. **Create a branch:**
   ```bash
   git checkout -b feature/my-feature
   ```

---

## How to Contribute

### Types of Contributions

We welcome many types of contributions:

#### 🐛 Bug Reports
Found a bug? Help us fix it!

**Before reporting:**

- Search existing issues
- Try latest version
- Gather reproduction steps

**Create an issue with:**

- Clear title
- Description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, versions)
- Logs (with sensitive data removed)

#### 💡 Feature Requests
Have an idea for improvement?

**Create an issue with:**

- Clear use case
- Proposed solution
- Alternatives considered
- Willingness to implement

#### 📝 Documentation
Improve or add documentation:

- Fix typos
- Clarify confusing sections
- Add examples
- Write guides
- Translate content

#### 💻 Code Contributions
Implement features or fix bugs:

- Bug fixes
- New features
- Performance improvements
- Refactoring
- Tests

#### 🎨 Design Patterns
Create new deployment patterns:

- Follow [Pattern Contract](../development/pattern-contract.md)
- Document use cases
- Provide examples
- Test thoroughly

---

## Development Workflow

### 1. Pick an Issue

**Good first issues:**

- Look for `good-first-issue` label
- Check `help-wanted` label
- Simple bug fixes
- Documentation improvements

**Claim an issue:**

- Comment "I'd like to work on this"
- Wait for assignment (prevents duplicates)

### 2. Create a Branch

```bash
# Get latest code
git checkout main
git pull upstream main

# Create feature branch
git checkout -b type/description

# Branch naming:
# feature/add-monitoring
# fix/ssh-timeout
# docs/architecture-guide
# refactor/validation-module
```

### 3. Make Changes

**Best practices:**

- Make focused, atomic commits
- Write clear commit messages
- Test your changes
- Update documentation
- Follow code standards

### 4. Commit Your Changes

**Commit message format:**
```
type(scope): brief description

Detailed explanation (if needed)

Fixes #123
```

**Types:**

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```bash
git commit -m "feat(orchestrator): add multi-deployment support"
git commit -m "fix(validation): resolve mnemonic loading issue"
git commit -m "docs(architecture): add system diagrams"
```

### 5. Push and Create Pull Request

```bash
# Push to your fork
git push origin feature/my-feature

# Create PR on GitHub
# - Fill out the PR template
# - Link related issues
# - Request review
```

### 6. Code Review Process

**What to expect:**
- Maintainers will review within 3-5 business days
- You may be asked to make changes
- CI tests must pass
- At least one approval required

**Responding to feedback:**
- Be receptive to suggestions
- Ask questions if unclear
- Make requested changes
- Push updates to your branch
- Re-request review when ready

### 7. Merge

Once approved:
- Maintainer will merge your PR
- Branch will be deleted automatically
- Changes go into next release

---

## Code Standards

### Shell Scripts (Bash)

**Style:**
```bash
#!/usr/bin/env bash
# Brief description of script purpose

set -e  # Exit on error

# Constants in UPPERCASE
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Functions use snake_case
my_function() {
    local param=$1
    
    # Clear logic with comments
    echo "Processing: $param"
}

# Call functions
my_function "value"
```

**Best practices:**
- Use `set -e` for error handling
- Quote variables: `"$var"` not `$var`
- Use `local` for function variables
- Add comments for complex logic
- Use meaningful variable names
- Check command existence: `command -v cmd`

### Terraform/OpenTofu

**Style:**
```hcl
# Use consistent formatting
resource "grid_deployment" "vm" {
  name = "my_deployment"
  
  # Group related attributes
  network {
    node_id = var.node_id
    name    = "network"
  }
}

# Document outputs
output "primary_ip" {
  description = "Primary IP address for SSH connection"
  value       = local.primary_ip
}
```

**Requirements:**
- Implement [pattern contract](../development/pattern-contract.md)
- Use variables for configurability
- Document all outputs
- Run `terraform fmt` before committing

### Ansible

**Style:**
```yaml
---
# Use descriptive task names
- name: Install required packages
  apt:
    name:
      - git
      - curl
      - build-essential
    state: present
    update_cache: yes
  become: yes

# Use handlers for services
- name: Configure application
  template:
    src: app.conf.j2
    dest: /etc/app/config.conf
  notify: restart application

handlers:
  - name: restart application
    systemd:
      name: app
      state: restarted
```

**Best practices:**
- Use descriptive task names
- Idempotent operations
- Use handlers for restarts
- Tag tasks appropriately
- Test playbooks thoroughly

---

## Testing

### Manual Testing

**Before submitting PR:**
1. Test your changes locally
2. Try different configurations
3. Test error cases
4. Verify logs are helpful

**Test script:**
```bash
# Example test workflow
cd tfgrid-compose

# Test validation
./cli/tfgrid-compose up /nonexistent/path  # Should fail gracefully

# Test deployment (if you have access to ThreeFold Grid)
./cli/tfgrid-compose up ../tfgrid-ai-agent

# Test cleanup
./cli/tfgrid-compose down ../tfgrid-ai-agent
```

### Automated Testing

**Run test suite:**
```bash
# Run validation tests
./tests/test-validation.sh

# Add new tests for your changes
# tests/test-my-feature.sh
```

### Test Checklist

- [ ] Code follows style guide
- [ ] Manual testing completed
- [ ] Error cases handled
- [ ] Logs are clear and helpful
- [ ] Documentation updated
- [ ] No sensitive data in commits

---

## Documentation

### When to Update Docs

**Always update docs when:**
- Adding new features
- Changing behavior
- Adding CLI commands
- Modifying configuration options
- Fixing bugs that affect usage

### Documentation Structure

**tfgrid-docs/** (public-facing):
- User guides
- API documentation
- Tutorials
- Troubleshooting

**tfgrid-compose/README.md:**
- Project overview
- Quick start
- Links to full docs

### Writing Style

**Be clear and concise:**
- Short sentences
- Active voice
- Clear examples
- Step-by-step instructions

**Good example:**
```markdown
## Deploy Application

Deploy your application to ThreeFold Grid:

```bash
tfgrid-compose up /path/to/app
```

This command will:
1. Validate prerequisites
2. Create infrastructure
3. Configure networking
4. Deploy your application
```

**Bad example:**
```markdown
## Deployment

You can deploy stuff with the up command maybe.
```

---

## Community

### Code of Conduct

All contributors must follow our [Code of Conduct](code-of-conduct.md).

**Key points:**
- Be respectful
- Be inclusive
- Be constructive
- Be professional

### Communication Channels

**GitHub:**
- Issues: Bug reports, feature requests
- Discussions: Questions, ideas, help
- Pull Requests: Code contributions

**Maintainers:**
- Tag `@tfgrid-studio/maintainers` for urgent issues
- Be patient - we're volunteers

### Getting Help

**Before asking:**
1. Check documentation
2. Search existing issues
3. Read troubleshooting guide

**When asking:**
- Provide context
- Include error messages
- Show what you tried
- Be specific

---

## Release Process

### Versioning

We follow [Semantic Versioning](../development/versioning-policy.md):
- MAJOR: Breaking changes
- MINOR: New features (backward-compatible)
- PATCH: Bug fixes

### Release Cycle

**Regular releases:**
- PATCH: As needed for bug fixes
- MINOR: Monthly or when features are ready
- MAJOR: Annually or for significant changes

**Your contribution will be included in the next release after merge.**

---

## Recognition

### Contributors

All contributors are recognized in:
- GitHub contributors list
- Release notes
- Project README

### Maintainers

Outstanding contributors may be invited to become maintainers.

**Maintainer responsibilities:**
- Review pull requests
- Triage issues
- Guide contributors
- Make release decisions
- Enforce Code of Conduct

---

## Quick Reference

### Common Tasks

```bash
# Get latest code
git checkout main
git pull upstream main

# Create branch
git checkout -b feature/my-feature

# Make changes and commit
git add .
git commit -m "feat(scope): description"

# Push and create PR
git push origin feature/my-feature

# Update branch with latest main
git fetch upstream
git rebase upstream/main
git push --force-with-lease origin feature/my-feature
```

### Getting Unstuck

**Your PR isn't being reviewed?**
- Ping after 5 business days
- Check if CI failed
- Ensure you filled out PR template

**Tests are failing?**
- Check CI logs
- Run tests locally
- Ask for help in PR comments

**Merge conflict?**
- Rebase on latest main
- Resolve conflicts
- Force push to your branch

---

## Thank You!

Thank you for contributing to TFGrid Studio! Your help makes this project better for everyone.

**Questions?**
- Create a [GitHub Discussion](https://github.com/orgs/tfgrid-studio/discussions)
- Email: [contact@tfgrid.studio](mailto:contact@tfgrid.studio)

---

**Happy Contributing!** 🚀
