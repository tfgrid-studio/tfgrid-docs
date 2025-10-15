# Changelog

All notable changes to TFGrid Studio will be documented in this file.

---

## [0.9.0] - 2025-10-14

### Added
- **OpenTofu Priority Support**: Now checks for and prefers OpenTofu over Terraform
- **Versioning Policy**: Comprehensive versioning documentation following Semantic Versioning 2.0.0
- **Architecture Documentation**: Complete system architecture guide
- **Troubleshooting Guide**: Comprehensive error handling and recovery procedures
- **Community Documentation**: Code of Conduct, Contributing Guide, Security Policy

### Changed
- **Version Standardization**: Unified version to 0.9.0 across all files following SemVer
- **Documentation Migration**: Moved all user-facing docs to tfgrid-docs for centralized access
- **Install Messages**: Updated to recommend OpenTofu as primary option

### Fixed
- **Version Inconsistency**: Resolved conflicting versions (was showing 2.0.0, 1.0.0, 0.1.0-mvp)
- **Tool Detection**: Fixed hardcoded terraform references to support OpenTofu
- **README Paths**: Updated tfgrid-deployer references to tfgrid-compose

---

## [Pre-0.9.0] - 2025-10-09

### 🎉 TFGrid Studio Launch

**Rebranded to TFGrid Studio**
- Changed organization from `tfgrid-compose` to `tfgrid-studio`
- Better positioning as complete development platform (not just deployment)
- Renamed `tfgrid-deployer` to `tfgrid-compose` (more accurate product name)

**New Websites**
- ✅ **tfgrid.studio** - Marketing landing page
  - Modern Astro + TailwindCSS v4
  - Hero, features, pricing, CTA sections
  - Responsive design
  - GitHub Pages hosting (zero cost)
  
- ✅ **docs.tfgrid.studio** - Documentation site
  - MkDocs Material theme
  - Complete guides and roadmap
  - Auto-deployment via GitHub Actions
  - Custom domain configured

**Updated Documentation**
- Comprehensive getting started guides
- Current status (v1.0.0)
- Planned features roadmap
- Source repository acknowledgment
- Architecture documentation

### Added

**tfgrid-compose (formerly tfgrid-deployer)**
- Full deployment orchestration (Terraform + Ansible + WireGuard)
- Context file support (`.tfgrid-compose.yaml`)
- Agent subcommand for AI operations
- Auto-install with PATH setup
- Input validation & idempotency protection
- Remote command execution (`exec`)
- State management system
- Single-VM pattern (production ready)

**tfgrid-ai-agent**
- AI coding with Qwen integration
- Loop technique for iterative development
- Project management system
- Systemd service management
- Critical bug fix: Unified project directory structure

**tfgrid-www**
- Beautiful marketing landing page
- Pricing tiers (Community/Pro/Business/Enterprise)
- Feature showcase
- GitHub Pages deployment

**tfgrid-docs**
- Complete documentation structure
- Getting started guides
- Roadmap documentation
- Source acknowledgment

### Changed

- **Organization name:** `tfgrid-compose` → `tfgrid-studio`
- **Main CLI repo:** `tfgrid-deployer` → `tfgrid-compose`
- **Domains:** 
  - Main site: `tfgrid.studio`
  - Docs: `docs.tfgrid.studio`

### Fixed

- AI agent project directory structure (unified to `/opt/ai-agent/projects/`)
- All documentation references updated to new organization
- Git remotes updated across all repositories

---

## [0.9.0] - 2025-10-08

### Initial Development

- Initial scaffolding of tfgrid-deployer
- Basic single-VM pattern
- Terraform + Ansible automation
- WireGuard networking
- Context file support

---

**Format:** Based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)  
**Versioning:** [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
