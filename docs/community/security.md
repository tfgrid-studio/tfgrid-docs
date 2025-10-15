# Security Policy

**Version:** 1.0  
**Last Updated:** October 14, 2025  
**Applies To:** All TFGrid Studio projects

---

## Reporting Security Vulnerabilities

**We take security seriously.** If you discover a security vulnerability, please report it responsibly.

### 🔒 How to Report

**DO NOT** create public GitHub issues for security vulnerabilities.

**Instead, use one of these secure channels:**

#### 1. GitHub Security Advisory (Preferred)
1. Go to the repository's Security tab
2. Click "Report a vulnerability"
3. Fill out the private advisory form
4. Submit

#### 2. Email
Send details to: **[security@tfgrid.studio](mailto:security@tfgrid.studio)**

**Encrypt with PGP:** (optional but recommended)
```
-----BEGIN PGP PUBLIC KEY BLOCK-----
[PGP key will be published separately]
-----END PGP PUBLIC KEY BLOCK-----
```

### 📋 What to Include

**Please provide:**
- **Description:** Clear explanation of the vulnerability
- **Impact:** What an attacker could do
- **Affected versions:** Which versions are vulnerable
- **Steps to reproduce:** Detailed reproduction steps
- **Proof of concept:** Code/commands demonstrating the issue (if applicable)
- **Suggested fix:** If you have ideas (optional)
- **Your contact info:** For follow-up questions

**Example report:**
```
Title: Mnemonic exposure in debug logs

Description:
When running with debug mode, the ThreeFold mnemonic is logged
in plain text to .tfgrid-compose/terraform-apply.log

Impact:
An attacker with access to log files could steal wallet credentials
and drain funds from the ThreeFold account.

Affected Versions:
tfgrid-compose v0.9.0 and earlier

Steps to Reproduce:
1. Enable debug mode
2. Run: tfgrid-compose --debug up <app>
3. Check: .tfgrid-compose/terraform-apply.log
4. Mnemonic visible in output

Suggested Fix:
Redact TF_VAR_mnemonic from all log output
```

---

## Response Timeline

We aim to respond quickly to security reports:

| Stage | Timeline |
|-------|----------|
| **Acknowledgment** | Within 24 hours |
| **Initial Assessment** | Within 3 business days |
| **Severity Rating** | Within 5 business days |
| **Patch Development** | Varies by severity (see below) |
| **Disclosure** | After patch is released |

### Severity-Based Timelines

**Critical (CVSS 9.0-10.0):**
- Emergency patch within 7 days
- Immediate notification to users

**High (CVSS 7.0-8.9):**
- Patch within 30 days
- Advance notification to users

**Medium (CVSS 4.0-6.9):**
- Patch in next regular release
- Standard release notes

**Low (CVSS 0.1-3.9):**
- May be batched with other fixes
- Documented in changelog

---

## What Happens Next?

### 1. Triage
We will:
- Confirm the vulnerability
- Assess severity using CVSS
- Determine affected versions
- Identify fix priority

### 2. Fix Development
We will:
- Develop a patch
- Test thoroughly
- Prepare security advisory
- Plan coordinated disclosure

### 3. Disclosure
We will:
- Release patched version
- Publish security advisory
- Update CHANGELOG
- Credit you (if desired)

### 4. Communication
You will receive:
- Regular updates on progress
- Credit in security advisory (optional)
- Recognition in Hall of Fame (optional)

---

## Supported Versions

We provide security updates for:

| Version | Supported |
|---------|-----------|
| 0.9.x   | ✅ Yes (current) |
| 0.8.x   | ❌ No |
| < 0.8   | ❌ No |

**Recommendation:** Always use the latest version.

---

## Security Best Practices

### For Users

#### Mnemonic Security

**Store securely:**
```bash
# Create secure config directory
mkdir -p ~/.config/threefold
chmod 700 ~/.config/threefold

# Store mnemonic with restricted permissions
echo "your twelve word mnemonic here" > ~/.config/threefold/mnemonic
chmod 600 ~/.config/threefold/mnemonic

# Verify permissions
ls -la ~/.config/threefold/mnemonic
# Should show: -rw------- (600)
```

**Never:**
- ❌ Commit mnemonics to version control
- ❌ Share mnemonics in chat/email
- ❌ Store in publicly accessible locations
- ❌ Use same mnemonic across environments

**Do:**
- ✅ Use environment variables in CI/CD
- ✅ Rotate mnemonics regularly
- ✅ Use separate mnemonics for dev/prod
- ✅ Back up securely (encrypted offline storage)

#### SSH Key Management

**Use strong keys:**
```bash
# Generate Ed25519 key (recommended)
ssh-keygen -t ed25519 -f ~/.ssh/tfgrid_ed25519

# Or RSA 4096-bit
ssh-keygen -t rsa -b 4096 -f ~/.ssh/tfgrid_rsa
```

**Protect private keys:**
```bash
# Set correct permissions
chmod 600 ~/.ssh/tfgrid_ed25519
chmod 644 ~/.ssh/tfgrid_ed25519.pub

# Use passphrase (recommended)
ssh-keygen -p -f ~/.ssh/tfgrid_ed25519
```

#### WireGuard Security

**Key management:**
- Generated keys stored in Terraform state
- Private keys in `/etc/wireguard/` (600 permissions)
- Don't share WireGuard configurations

**Network isolation:**
- WireGuard creates private networks
- No default internet routing
- Explicitly configure external access if needed

#### Deployment Security

**State files contain sensitive data:**
```bash
# Add to .gitignore
echo ".tfgrid-compose/" >> .gitignore
echo ".terraform/" >> .gitignore
echo "*.tfstate*" >> .gitignore

# Never commit:
# - .tfgrid-compose/
# - Terraform state files
# - WireGuard configs
# - Private keys
```

**Clean up after testing:**
```bash
# Remove sensitive logs
rm -rf .tfgrid-compose/

# Remove WireGuard configs
sudo rm -f /etc/wireguard/wg*.conf
```

### For Contributors

#### Code Security

**Never commit:**
- API keys or tokens
- Passwords or secrets
- Private keys
- Mnemonics or seed phrases
- Personal data

**Use secure coding practices:**
- Validate all inputs
- Sanitize log output
- Use parameterized commands (avoid injection)
- Check file permissions
- Handle errors securely

**Example - Redacting sensitive data:**
```bash
# ❌ Bad: Logs mnemonic
echo "Using mnemonic: $TF_VAR_mnemonic"

# ✅ Good: Redacts mnemonic
if [ -n "$TF_VAR_mnemonic" ]; then
    echo "Mnemonic loaded successfully"
else
    echo "Mnemonic not found"
fi
```

#### Dependency Security

**Keep dependencies updated:**
```bash
# Check for updates
ansible --version
terraform --version
tofu --version

# Update to latest
sudo apt update && sudo apt upgrade
```

**Review third-party code:**
- Audit pattern contributions
- Review Terraform modules
- Check Ansible roles
- Verify scripts before execution

### For Maintainers

#### Release Security

**Before release:**
- Review all PRs for security issues
- Run security scanners
- Check for hardcoded secrets
- Verify dependency versions
- Test in isolated environment

**Release checklist:**
- [ ] No hardcoded credentials
- [ ] Dependencies up to date
- [ ] Security advisory reviewed
- [ ] CHANGELOG includes security fixes
- [ ] Version bumped appropriately

#### Incident Response

**If vulnerability found:**
1. Assess severity immediately
2. Notify security team
3. Develop fix in private
4. Test thoroughly
5. Coordinate disclosure
6. Release patch
7. Notify users

---

## Common Security Issues

### Mnemonic Exposure

**Risk:** High - Can lead to fund theft

**Vectors:**
- Logs containing mnemonics
- Committed to version control
- Insecure file permissions
- Exposed in error messages

**Prevention:**
- Redact from all logs
- Check file permissions
- Use `.gitignore`
- Sanitize error output

### State File Exposure

**Risk:** Medium - Contains deployment details

**Vectors:**
- Committed to version control
- Publicly accessible storage
- Insufficient file permissions

**Prevention:**
- Add to `.gitignore`
- Use secure state backends
- Restrict file permissions
- Clean up after use

### SSH Key Compromise

**Risk:** High - Unauthorized VM access

**Vectors:**
- Weak key strength
- No passphrase
- Insecure storage
- Shared keys

**Prevention:**
- Use Ed25519 or RSA 4096
- Use passphrases
- Restrict permissions (600)
- Unique keys per deployment

### WireGuard Config Exposure

**Risk:** Medium - Network access

**Vectors:**
- World-readable files
- Committed to repos
- Shared configurations

**Prevention:**
- 600 permissions on configs
- Don't commit configs
- Rotate keys regularly

---

## Security Features

### Current Security Measures

**tfgrid-compose implements:**

1. **Mnemonic Protection**
   - Environment variable isolation
   - File permission checks
   - Warning on insecure permissions

2. **SSH Key Security**
   - System key usage (not embedded)
   - Key-based auth only (no passwords)
   - Multiple key type support

3. **WireGuard Encryption**
   - End-to-end encrypted tunnels
   - Private network isolation
   - Automatic key generation

4. **State Isolation**
   - Local state directory
   - Git-ignored by default
   - Per-deployment separation

5. **Input Validation**
   - Path validation
   - Manifest validation
   - Prerequisite checks

### Planned Security Enhancements

**Roadmap:**
- [ ] Encrypted state backends (v0.11.0)
- [ ] Secrets management integration (v0.12.0)
- [ ] Automated security scanning (v1.0.0)
- [ ] Supply chain verification (v1.1.0)
- [ ] Zero-knowledge deployment options (v2.0.0)

---

## Security Audits

**Status:** No formal audit completed yet

**Planned:**
- Community review: Ongoing
- First formal audit: Planned for v1.0.0
- Regular audits: Annually after 1.0

**Want to help?**
- Review code for security issues
- Report vulnerabilities responsibly
- Contribute security improvements

---

## Hall of Fame

We recognize security researchers who help keep TFGrid Studio secure.

**Contributors:**
- *Be the first!*

**Acknowledgment options:**
- GitHub profile link
- Twitter handle
- Company name
- Anonymous

---

## Contact

**Security Team:** [security@tfgrid.studio](mailto:security@tfgrid.studio)  
**General Inquiries:** [community@tfgrid.studio](mailto:community@tfgrid.studio)  
**Code of Conduct:** [Code of Conduct](code-of-conduct.md)

---

## Legal

### Safe Harbor

TFGrid Studio supports safe harbor for security researchers who:
- Make good faith effort to avoid harm
- Follow responsible disclosure
- Don't access/modify user data without permission
- Don't perform DoS attacks
- Don't spam or cause disruption

We will not pursue legal action against researchers who follow these guidelines.

### Scope

**In scope:**
- tfgrid-compose
- tfgrid-ai-agent
- tfgrid-docs website
- Official infrastructure

**Out of scope:**
- Third-party services (ThreeFold Grid itself)
- User deployments
- Community projects
- Social engineering

---

**Thank you for helping keep TFGrid Studio secure!** 🔒
