# Versioning Policy

**Status:** Active  
**Adopted:** October 14, 2025  
**Applies to:** tfgrid-compose and all TFGrid Studio projects

## Overview

TFGrid Studio follows [Semantic Versioning 2.0.0](https://semver.org/) for all projects. This document defines our versioning standards, release processes, and backward compatibility commitments.

---

## Semantic Versioning Standard

### Format: MAJOR.MINOR.PATCH

Given a version number **MAJOR.MINOR.PATCH** (e.g., 1.2.3):

#### **MAJOR Version (X.0.0)**
Increment when making **incompatible API changes** or **breaking changes**.

**Examples:**

- Changing CLI command structure (e.g., `tfgrid-compose deploy` → `tfgrid-compose up`)
- Removing or renaming core features
- Changing pattern contract requirements
- Modifying state file format without migration
- Removing deprecated features
- Changing default behavior in non-backward-compatible ways

**User Impact:** May require code/script changes, migration steps, or configuration updates.

#### **MINOR Version (x.Y.0)**
Increment when adding **new features** in a **backward-compatible** manner.

**Examples:**

- Adding new patterns (e.g., adding k3s pattern)
- Adding new CLI commands (e.g., `tfgrid-compose monitor`)
- Adding new configuration options
- Adding new networking modes
- Introducing new capabilities without breaking existing functionality
- Adding optional parameters to existing commands

**User Impact:** No changes required; new features available opt-in.

#### **PATCH Version (x.y.Z)**
Increment when making **backward-compatible bug fixes**.

**Examples:**

- Fixing validation logic
- Correcting error messages
- Improving performance
- Documentation corrections
- Security patches (non-breaking)
- Dependency updates (non-breaking)
- Minor UI/UX improvements

**User Impact:** Seamless upgrade; fixes only.

---

## Version 0.x.y (Pre-1.0) Policy

### Special Rules for Pre-1.0 Versions

During the **0.x.y** phase, the project is considered **pre-release** with more flexibility:

- **0.y.0**: May include breaking changes (similar to MAJOR in 1.x.y+)
- **0.y.z**: Bug fixes and minor features
- **0.1.0**: Initial development
- **0.9.0**: Release candidate, feature-complete, near 1.0
- **1.0.0**: First stable release, API locked

### Current Status (tfgrid-compose)

**Version:** 0.9.0 (October 2025)

**What this means:**

- Core features are production-ready
- API is stable but may evolve based on feedback
- Breaking changes possible but minimized
- Path to 1.0.0 within 2-3 months

---

## Release Process

### 1. Version Decision

**Before any release, determine version bump:**

```bash
# Current version: 0.9.0

# Bug fix only → 0.9.1
# New backward-compatible feature → 0.10.0
# Breaking change (pre-1.0) → 0.10.0 or 1.0.0
# Breaking change (post-1.0) → 2.0.0
```

### 2. Update Version References

**Files to update:**

- `cli/tfgrid-compose` (VERSION variable)
- `Makefile` (help text)
- `README.md` (version badge)
- Any other version references

### 3. Update Changelog

**Format:** Keep a `CHANGELOG.md` using [Keep a Changelog](https://keepachangelog.com/) format.

```markdown
# Changelog

## [0.9.1] - 2025-10-15

### Fixed
- WireGuard connection timeout handling
- State file permission issues

### Changed
- Improved error messages for missing prerequisites

## [0.9.0] - 2025-10-14

### Added
- OpenTofu priority support
- Multi-deployment management
- Architecture documentation

### Changed
- Renamed from tfgrid-deployer to tfgrid-compose

### Fixed
- Version inconsistency across files
```

### 4. Git Tagging

**Tag format:** `vMAJOR.MINOR.PATCH`

```bash
# Create annotated tag
git tag -a v0.9.1 -m "Release v0.9.1: Bug fixes and improvements"

# Push tag to remote
git push origin v0.9.1
```

### 5. Release Notes

**Create GitHub Release with:**

- Version number
- Release date
- Changelog excerpt
- Installation instructions
- Breaking changes (if any)
- Migration guide (if needed)

---

## Backward Compatibility

### Commitment

**Post-1.0.0:**

- MAJOR version changes only for breaking changes
- Deprecation warnings before removal
- Migration guides for breaking changes
- Support for N-1 version during transition

**Pre-1.0.0 (0.x.y):**

- Best effort to maintain compatibility
- Breaking changes documented in changelog
- Migration notes provided when needed

### Deprecation Policy

**Post-1.0.0:**

1. **Announce:** Mark feature as deprecated with warning
2. **Grace Period:** Minimum 1 MINOR version (e.g., 1.2.0 → 1.3.0)
3. **Remove:** In next MAJOR version (e.g., 2.0.0)

**Example:**
```bash
# Version 1.2.0
tfgrid-compose old-command
# Warning: 'old-command' is deprecated, use 'new-command' instead
# Will be removed in v2.0.0

# Version 1.3.0
# Still works, still warns

# Version 2.0.0
# 'old-command' removed
```

---

## Breaking Changes

### Definition

A breaking change is any modification that:
- Requires users to modify existing scripts/code
- Changes default behavior in incompatible ways
- Removes or renames commands/features
- Changes output formats (CLI, logs, state files)
- Changes configuration schema

### Communication

**When introducing breaking changes:**

1. **Document** in CHANGELOG under "BREAKING CHANGES" section
2. **Provide** migration guide
3. **Announce** in release notes prominently
4. **Support** with automated migration tools when possible

**Example Breaking Change Documentation:**
```markdown
## [2.0.0] - 2026-01-15

### BREAKING CHANGES

- **CLI Command Structure**: Renamed `deploy` to `up` and `destroy` to `down`
  - Migration: Replace `tfgrid-compose deploy` with `tfgrid-compose up`
  - Migration: Replace `tfgrid-compose destroy` with `tfgrid-compose down`

- **State Directory**: Changed from `.tfgrid-deployer` to `.tfgrid-compose`
  - Migration: Run `tfgrid-compose migrate-state` to automatically upgrade
```

---

## Version Progression Path

### tfgrid-compose Roadmap

```
v0.9.0  ← Current (Oct 2025)
  ↓     Core features complete, production-ready
  ↓
v0.9.1  ← Bug fixes, OpenTofu refinements
  ↓
v0.9.2  ← Minor improvements
  ↓
v0.10.0 ← Multi-deployment, monitoring features
  ↓
v0.11.0 ← CI/CD, complete documentation, shell completion
  ↓
v0.12.0 ← Testing suite, community feedback incorporated
  ↓
v1.0.0  ← Official Stable Release (Q1 2026)
  ↓     API locked, backward compatibility guaranteed
  ↓
v1.1.0  ← Pattern registry, community patterns
  ↓
v1.2.0  ← Plugin system
  ↓
v2.0.0  ← Major architectural changes (if needed)
```

### Path to 1.0.0

**Requirements for 1.0.0 release:**

- ✅ All core patterns production-ready
- ✅ Comprehensive documentation
- ✅ Test coverage >80%
- ✅ CI/CD pipeline operational
- ✅ No known critical bugs
- ✅ Community feedback incorporated
- ✅ Migration guides prepared
- ✅ Performance benchmarks met
- ✅ Security review completed
- ✅ API/CLI stable and finalized

**Estimated Timeline:** Q1 2026 (2-3 months from v0.9.0)

---

## Release Cadence

### Post-1.0.0 Release Schedule

- **PATCH releases**: As needed (bug fixes, security)
- **MINOR releases**: Monthly or bi-monthly (features)
- **MAJOR releases**: Annually or as needed (breaking changes)

### Pre-1.0.0 Release Schedule

- **More frequent releases** during active development
- **Focus on feedback** and iteration
- **Version bumps** based on feature completion

---

## Version Checking

### In Scripts

**Users can check version programmatically:**

```bash
# Get version
tfgrid-compose --version
# Output: TFGrid Compose v0.9.0

# Check minimum version in scripts
REQUIRED_VERSION="0.9.0"
CURRENT_VERSION=$(tfgrid-compose --version | grep -oP '\d+\.\d+\.\d+')

if [[ "$(printf '%s\n' "$REQUIRED_VERSION" "$CURRENT_VERSION" | sort -V | head -n1)" != "$REQUIRED_VERSION" ]]; then
    echo "Error: tfgrid-compose $REQUIRED_VERSION or higher required"
    exit 1
fi
```

### Version API

**Future enhancement:**

```bash
# Check for updates
tfgrid-compose update check

# Auto-update
tfgrid-compose update install
```

---

## Multi-Project Coordination

### TFGrid Studio Version Alignment

**Projects:**

- `tfgrid-compose`: Orchestration platform
- `tfgrid-ai-agent`: AI agent application
- `tfgrid-docs`: Documentation site
- `tfgrid-portal`: Web portal

**Version Independence:**

- Each project has its own version
- No requirement for version alignment
- Compatibility documented in release notes

**Example:**
```
tfgrid-compose v0.9.0
  Compatible with: tfgrid-ai-agent v0.3.0+
  
tfgrid-ai-agent v0.3.2
  Requires: tfgrid-compose v0.8.0+
```

---

## Examples

### Example 1: Bug Fix Release

```
Current: v0.9.0
Change: Fixed WireGuard timeout issue
Next: v0.9.1 (PATCH bump)
```

### Example 2: New Feature

```
Current: v0.9.1
Change: Added multi-deployment management
Next: v0.10.0 (MINOR bump)
```

### Example 3: Breaking Change (Pre-1.0)

```
Current: v0.10.5
Change: Changed state directory structure
Next: v0.11.0 (MINOR bump in 0.x, includes breaking change)
Note: Provide migration tool
```

### Example 4: Breaking Change (Post-1.0)

```
Current: v1.5.3
Change: Removed deprecated 'deploy' command
Next: v2.0.0 (MAJOR bump)
Note: Announced in v1.2.0, removed in v2.0.0
```

---

## Frequently Asked Questions

### Why start at 0.9.0 instead of 0.1.0 or 1.0.0?

**Answer:** v0.9.0 signals:
- Core features are complete and production-ready
- API is stable but may evolve slightly based on feedback
- Close to 1.0.0 but not locked in yet
- Allows flexibility while showing maturity

### When will we reach 1.0.0?

**Answer:** When we meet the 1.0.0 requirements checklist (see above). Estimated Q1 2026, approximately 2-3 months after comprehensive testing and community feedback.

### Can I use 0.x versions in production?

**Answer:** Yes! Version 0.9.0+ is production-ready. The 0.x designation means the API may evolve, not that it's unstable.

### What if I find a critical bug?

**Answer:** We'll release a PATCH version (e.g., 0.9.0 → 0.9.1) immediately. Security issues take highest priority.

### How do I stay updated?

**Answer:**

- Watch the GitHub repository
- Subscribe to release notifications
- Check changelog regularly
- Use `tfgrid-compose update check` (future)

---

## References

- [Semantic Versioning 2.0.0](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [GitHub Release Best Practices](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)

---

## Document Updates

| Date | Version | Changes |
|------|---------|---------|
| 2025-10-14 | 1.0 | Initial versioning policy created |

---

**Status:** Active  
**Next Review:** After v1.0.0 release  
**Maintained by:** TFGrid Studio Team
