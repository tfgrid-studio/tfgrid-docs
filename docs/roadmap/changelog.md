# Changelog

All notable changes to TFGrid Studio will be documented in this file.

---

## [1.0.0] - 2025-10-09

### 🎉 Major Rebrand & Launch

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

**tfgrid-compose (formerly tfgrid-deployer) - v1.0.0**
- Full deployment orchestration (Terraform + Ansible + WireGuard)
- Context file support (`.tfgrid-compose.yaml`)
- Agent subcommand for AI operations
- Auto-install with PATH setup
- Input validation & idempotency protection
- Remote command execution (`exec`)
- State management system
- Single-VM pattern (production ready)

**tfgrid-ai-agent - v2.0.0**
- AI coding with Qwen integration
- Loop technique for iterative development
- Project management system
- Systemd service management
- Critical bug fix: Unified project directory structure

**tfgrid-www - v1.0.0**
- Beautiful marketing landing page
- Pricing tiers (Community/Pro/Business/Enterprise)
- Feature showcase
- GitHub Pages deployment

**tfgrid-docs - v1.0.0**
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
