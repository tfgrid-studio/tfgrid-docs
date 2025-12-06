# TFGrid Nextcloud Guide

**Nextcloud All-in-One cloud platform on ThreeFold Grid.**

## Overview

Deploy a complete Nextcloud instance with automatic management, built-in backups, and enterprise features.

**Key Features:**

- 📁 **File Sync & Share** - Access files from anywhere
- 📅 **Calendar & Contacts** - Integrated productivity apps
- 🎥 **Talk** - Video calls and chat
- 📝 **Office** - Collaborative document editing
- 🔒 **End-to-end Encryption** - Optional E2EE for sensitive files
- 💾 **BorgBackup** - Automatic encrypted backups

## Quick Start

### Prerequisites

- TFGrid account with mnemonic configured
- `tfgrid-compose` installed ([Installation Guide](../getting-started/installation.md))
- Domain with DNS pointing to your server

### Deploy

```bash
# Deploy from registry
tfgrid-compose up tfgrid-nextcloud

# Or with custom configuration
tfgrid-compose up tfgrid-nextcloud --var DOMAIN=cloud.example.com
```

### Complete Setup

After deployment:

1. **Open AIO Interface**: `https://<server-ip>:8443`
2. **Enter your domain** in the setup wizard
3. **Set admin password**
4. **Configure optional features** (Talk, Office, etc.)
5. **Click "Start containers"**
6. **Access Nextcloud**: `https://your-domain.com`

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DOMAIN` | Yes | - | Public domain for Nextcloud |
| `NEXTCLOUD_DATADIR` | No | Docker volume | Custom data directory |
| `NEXTCLOUD_UPLOAD_LIMIT` | No | 10G | Maximum upload size |
| `NEXTCLOUD_MEMORY_LIMIT` | No | 512M | PHP memory limit |
| `NEXTCLOUD_TIMEZONE` | No | UTC | Server timezone |

### Example Configuration

```bash
DOMAIN=cloud.example.com
NEXTCLOUD_UPLOAD_LIMIT=10G
NEXTCLOUD_MEMORY_LIMIT=1024M
NEXTCLOUD_TIMEZONE=America/New_York
```

## Architecture

```
┌─────────────┐     ┌───────────────────────────────────┐
│   Internet  │────▶│  Nextcloud AIO                    │
│             │     │  (Apache + Let's Encrypt)         │
└─────────────┘     └───────────────┬───────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
              ┌─────▼─────┐   ┌─────▼─────┐   ┌─────▼─────┐
              │ Nextcloud │   │ PostgreSQL│   │   Redis   │
              │   App     │   │  Database │   │   Cache   │
              └───────────┘   └───────────┘   └───────────┘
```

**Components (managed by AIO):**

- **Apache** - Web server with SSL
- **Nextcloud** - Main application
- **PostgreSQL** - Database
- **Redis** - Caching
- **Collabora/OnlyOffice** - Document editing (optional)
- **Talk** - Video conferencing (optional)
- **Imaginary** - Image processing (optional)

## Commands

### Backup & Restore

```bash
# Trigger AIO backup
tfgrid-compose backup

# Check backup status
tfgrid-compose backup-status

# Show restore instructions
tfgrid-compose restore
```

**Note:** Nextcloud AIO uses BorgBackup. Backups are managed through the AIO interface at `https://<server-ip>:8443`.

### Logs

```bash
# All logs
tfgrid-compose logs

# Specific service
tfgrid-compose logs mastercontainer
tfgrid-compose logs nextcloud
tfgrid-compose logs database
tfgrid-compose logs redis
tfgrid-compose logs apache

# Follow logs
tfgrid-compose logs nextcloud --follow
```

### Management

```bash
# Run Nextcloud occ commands
tfgrid-compose occ status
tfgrid-compose occ user:list
tfgrid-compose occ files:scan --all
tfgrid-compose occ maintenance:mode --on

# Check for updates
tfgrid-compose update

# Restart services
tfgrid-compose restart

# Health check
tfgrid-compose healthcheck
```

## Resource Requirements

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| CPU | 2 cores | 4 cores |
| Memory | 4 GB | 8 GB |
| Disk | 50 GB | 200 GB |

**Estimated Cost:** $30-50/month on ThreeFold Grid

## AIO Interface

The All-in-One interface (`https://<server-ip>:8443`) provides:

- **Container Management** - Start/stop/restart containers
- **Backup Configuration** - Set up automated backups
- **Update Management** - Check and apply updates
- **Optional Features** - Enable/disable apps
- **Domain Configuration** - Change domain settings

### Accessing AIO Interface

1. Open `https://<server-ip>:8443` in your browser
2. Accept the self-signed certificate warning
3. Use the password shown during initial setup

## Backup System

### BorgBackup Features

- **Deduplication** - Only stores changes, saves space
- **Encryption** - All backups are encrypted
- **Compression** - Reduces backup size
- **Remote Support** - Backup to external storage

### Configure Backups

1. Open AIO interface
2. Go to "Backup and restore"
3. Set backup location (local or remote)
4. Configure backup schedule
5. Set retention policy

### Restore from Backup

1. Stop all Nextcloud containers
2. Open AIO interface
3. Go to "Backup and restore"
4. Select backup to restore
5. Click "Restore"
6. Wait for completion
7. Restart containers

## SSL Configuration

### Automatic SSL (Default)

Nextcloud AIO handles SSL automatically using Let's Encrypt.

### Requirements

1. Domain DNS must point to server IP
2. Ports 80 and 443 must be accessible
3. Valid domain name (not IP address)

### Behind Reverse Proxy

If running behind another reverse proxy, configure AIO:

1. Set `APACHE_PORT` and `APACHE_IP_BINDING` environment variables
2. Configure your reverse proxy to forward to AIO

## Optional Features

Enable these in the AIO interface:

| Feature | Description | Resources |
|---------|-------------|-----------|
| **Collabora** | Document editing | +2GB RAM |
| **OnlyOffice** | Alternative office suite | +2GB RAM |
| **Talk** | Video conferencing | +1GB RAM |
| **Imaginary** | Image processing | +512MB RAM |
| **Fulltextsearch** | Full-text search | +1GB RAM |
| **ClamAV** | Antivirus scanning | +1GB RAM |

## Troubleshooting

### Common Issues

**AIO interface not accessible**

1. Check Docker is running:
   ```bash
   systemctl status docker
   ```
2. Check mastercontainer is running:
   ```bash
   docker ps | grep nextcloud-aio-mastercontainer
   ```
3. Ensure port 8443 is open

**SSL certificate errors**

1. Verify domain DNS:
   ```bash
   dig +short your-domain.com
   ```
2. Check AIO logs:
   ```bash
   tfgrid-compose logs mastercontainer
   ```
3. Ensure ports 80/443 are accessible

**Slow performance**

1. Enable Redis caching (enabled by default)
2. Increase PHP memory limit:
   ```bash
   NEXTCLOUD_MEMORY_LIMIT=1024M
   ```
3. Check available resources:
   ```bash
   free -h
   df -h
   ```

**File upload fails**

1. Check upload limit:
   ```bash
   tfgrid-compose occ config:system:get upload_max_filesize
   ```
2. Increase limit in `.env`:
   ```bash
   NEXTCLOUD_UPLOAD_LIMIT=20G
   ```

### Health Check

```bash
tfgrid-compose healthcheck
```

Checks:
- Docker service
- Mastercontainer status
- Nextcloud container status
- Database and Redis
- HTTP response

## Mobile & Desktop Apps

### Desktop Sync Client

Download from: https://nextcloud.com/install/#install-clients

Configure:
1. Server address: `https://your-domain.com`
2. Login with your credentials
3. Select folders to sync

### Mobile Apps

- **iOS**: App Store - "Nextcloud"
- **Android**: Play Store or F-Droid - "Nextcloud"

## Security Best Practices

1. **Use strong admin password**
2. **Enable 2FA** - Settings → Security → Two-Factor Authentication
3. **Regular backups** - Configure automated daily backups
4. **Keep updated** - Apply updates promptly via AIO
5. **Review sharing** - Audit shared files/folders regularly
6. **Enable encryption** - For sensitive data

## Migration

### From Existing Nextcloud

1. Export data using Nextcloud's migration tools
2. Deploy TFGrid Nextcloud
3. Import data via AIO restore or manual migration

### To Another Server

1. Create backup via AIO interface
2. Deploy TFGrid Nextcloud on new server
3. Restore backup on new server

## Related Documentation

- [Custom Apps Guide](custom-apps.md)
- [TFGrid Compose Reference](tfgrid-compose.md)
- [Nextcloud Documentation](https://docs.nextcloud.com/)
- [Nextcloud AIO GitHub](https://github.com/nextcloud/all-in-one)
