# TFGrid WordPress Guide

**Self-hosted WordPress with Caddy and MariaDB on ThreeFold Grid.**

## Overview

Deploy a production-ready WordPress installation with automatic SSL, database management, and backup capabilities.

**Key Features:**

- 🌐 **One-click deployment** - Deploy in under 5 minutes
- 🔒 **Automatic SSL** - Caddy handles Let's Encrypt certificates
- 💾 **Persistent storage** - Docker volumes for data safety
- 📦 **Easy backups** - Built-in backup and restore commands
- 🔧 **WP-CLI included** - Full command-line management

## Quick Start

### Prerequisites

- TFGrid account with mnemonic configured
- `tfgrid-compose` installed ([Installation Guide](../getting-started/installation.md))

### Deploy

```bash
# Deploy from registry
tfgrid-compose up tfgrid-wordpress

# Or with custom configuration
tfgrid-compose up tfgrid-wordpress --var DOMAIN=blog.example.com
```

### Access Your Site

After deployment completes:

1. **WordPress**: `https://your-domain.com`
2. **Admin Panel**: `https://your-domain.com/wp-admin`

Complete the WordPress setup wizard to create your admin account.

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DOMAIN` | Yes | - | Public domain for WordPress |
| `SSL_EMAIL` | No | - | Email for Let's Encrypt |
| `DB_PASSWORD` | No | Auto-generated | MariaDB password |
| `DB_ROOT_PASSWORD` | No | Auto-generated | MariaDB root password |

### Example Configuration

Create a `.env` file:

```bash
DOMAIN=blog.example.com
SSL_EMAIL=admin@example.com
```

Or pass variables directly:

```bash
tfgrid-compose up tfgrid-wordpress \
  --var DOMAIN=blog.example.com \
  --var SSL_EMAIL=admin@example.com
```

## Architecture

```
┌─────────────┐     ┌───────────────┐     ┌──────────────┐
│   Internet  │────▶│  Caddy :443   │────▶│  WordPress   │
│             │     │  (auto-SSL)   │     │  :8080       │
└─────────────┘     └───────────────┘     └──────┬───────┘
                                                 │
                                          ┌──────▼───────┐
                                          │   MariaDB    │
                                          │   :3306      │
                                          └──────────────┘
```

**Components:**

- **Caddy** - Reverse proxy with automatic HTTPS
- **WordPress** - Official Docker image (latest)
- **MariaDB** - Database with health checks

## Commands

### Backup & Restore

```bash
# Create full backup
tfgrid-compose backup

# List available backups
tfgrid-compose list-backups

# Restore from backup
tfgrid-compose restore --backup /path/to/backup.tar.gz
```

### Logs

```bash
# All logs
tfgrid-compose logs

# Specific service
tfgrid-compose logs wordpress
tfgrid-compose logs db
tfgrid-compose logs caddy

# Follow logs
tfgrid-compose logs wordpress --follow
```

### Management

```bash
# Open WordPress container shell
tfgrid-compose shell

# Run WP-CLI commands
tfgrid-compose wp core version
tfgrid-compose wp plugin list
tfgrid-compose wp user list
tfgrid-compose wp cache flush

# Restart services
tfgrid-compose restart

# Health check
tfgrid-compose healthcheck
```

## Resource Requirements

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| CPU | 1 core | 2 cores |
| Memory | 1 GB | 2 GB |
| Disk | 20 GB | 50 GB |

**Estimated Cost:** $10-20/month on ThreeFold Grid

## Backup Details

### What's Backed Up

- **Database** - Full MySQL dump
- **WordPress files** - wp-content, themes, plugins, uploads
- **Configuration** - .env and Caddyfile

### Backup Location

Backups are stored in `/opt/wordpress/backups/` with format:
```
wordpress_backup_YYYYMMDD_HHMMSS.tar.gz
```

### Automated Backups

Set up a cron job for automated backups:

```bash
# Add to crontab (daily at 2 AM)
0 2 * * * /opt/wordpress/scripts/backup.sh
```

## SSL Configuration

### Automatic SSL (Default)

Caddy automatically obtains and renews Let's Encrypt certificates when you provide a domain.

### Requirements for SSL

1. Domain DNS must point to your server's IP
2. Ports 80 and 443 must be accessible
3. (Optional) Provide `SSL_EMAIL` for certificate notifications

### Local Development

For local development without SSL:

```bash
DOMAIN=localhost
```

Caddy will serve HTTP only when using localhost or IP addresses.

## Customization

### Installing Plugins

```bash
# Via WP-CLI
tfgrid-compose wp plugin install woocommerce --activate

# Or via WordPress admin panel
# https://your-domain.com/wp-admin/plugins.php
```

### Installing Themes

```bash
# Via WP-CLI
tfgrid-compose wp theme install flavor --activate

# Or via WordPress admin panel
```

### PHP Configuration

To modify PHP settings, edit the WordPress container:

```bash
tfgrid-compose shell
# Inside container:
echo "upload_max_filesize = 64M" >> /usr/local/etc/php/conf.d/uploads.ini
echo "post_max_size = 64M" >> /usr/local/etc/php/conf.d/uploads.ini
```

## Troubleshooting

### Common Issues

**"Error establishing database connection"**

1. Check MariaDB is running:
   ```bash
   docker ps | grep wordpress-db
   ```
2. Verify database credentials in `/opt/wordpress/.env`
3. Check database logs:
   ```bash
   tfgrid-compose logs db
   ```

**SSL certificate not working**

1. Ensure domain DNS points to server IP:
   ```bash
   dig +short your-domain.com
   ```
2. Check Caddy logs:
   ```bash
   tfgrid-compose logs caddy
   ```
3. Verify ports 80/443 are open

**WordPress is slow**

1. Install a caching plugin (e.g., W3 Total Cache)
2. Check available memory:
   ```bash
   free -h
   ```
3. Consider upgrading VM resources

**White screen of death**

1. Enable WordPress debug mode:
   ```bash
   tfgrid-compose wp config set WP_DEBUG true
   ```
2. Check WordPress logs:
   ```bash
   tfgrid-compose logs wordpress
   ```

### Health Check

Run a comprehensive health check:

```bash
tfgrid-compose healthcheck
```

This checks:
- Docker service status
- Container health
- HTTP response
- Disk space

## Migration

### From Existing WordPress

1. Export your existing site using a plugin (e.g., All-in-One WP Migration)
2. Deploy TFGrid WordPress
3. Install the same migration plugin
4. Import your backup

### To Another Server

1. Create a backup:
   ```bash
   tfgrid-compose backup
   ```
2. Copy backup file to new server
3. Deploy TFGrid WordPress on new server
4. Restore:
   ```bash
   tfgrid-compose restore --backup /path/to/backup.tar.gz
   ```

## Security Best Practices

1. **Keep WordPress updated** - Enable auto-updates or update regularly
2. **Use strong passwords** - For admin and database
3. **Install security plugins** - Wordfence, Sucuri, etc.
4. **Regular backups** - Set up automated daily backups
5. **Limit login attempts** - Use a plugin to prevent brute force
6. **Use HTTPS** - Already enabled by default with Caddy

## Related Documentation

- [Custom Apps Guide](custom-apps.md)
- [Tarball Releases](tarball-releases.md)
- [TFGrid Compose Reference](tfgrid-compose.md)
