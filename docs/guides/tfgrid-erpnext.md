# TFGrid ERPNext Guide

**Open-source ERP system (Frappe/ERPNext) on ThreeFold Grid.**

## Overview

Deploy a complete ERPNext installation for managing your business operations including accounting, inventory, sales, HR, and more.

**Key Features:**

- 📊 **Accounting** - Complete financial management
- 📦 **Inventory** - Stock management and tracking
- 🛒 **Sales & Purchase** - Order management
- 🏭 **Manufacturing** - Production planning
- 👥 **HR & Payroll** - Employee management
- 📋 **Projects** - Task and project tracking
- 📈 **CRM** - Customer relationship management

## Quick Start

### Prerequisites

- TFGrid account with mnemonic configured
- `tfgrid-compose` installed ([Installation Guide](../getting-started/installation.md))
- Domain with DNS pointing to your server
- Minimum 4GB RAM recommended

### Deploy

```bash
# Deploy from registry
tfgrid-compose up tfgrid-erpnext

# Or with custom configuration
tfgrid-compose up tfgrid-erpnext --var DOMAIN=erp.example.com
```

### Access Your ERP

After deployment completes (10-15 minutes):

1. **ERPNext**: `https://your-domain.com`
2. **Username**: `Administrator`
3. **Password**: Found in `/opt/erpnext/config/credentials.txt`

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DOMAIN` | Yes | - | Public domain for ERPNext |
| `SSL_EMAIL` | No | - | Email for Let's Encrypt |
| `SITE_NAME` | No | Domain | ERPNext site name |
| `ADMIN_PASSWORD` | No | Auto-generated | Admin password |
| `DB_PASSWORD` | No | Auto-generated | Database password |
| `ERPNEXT_VERSION` | No | v15 | ERPNext version |
| `TIMEZONE` | No | UTC | Server timezone |

### Example Configuration

```bash
DOMAIN=erp.example.com
SSL_EMAIL=admin@example.com
ERPNEXT_VERSION=v15
TIMEZONE=America/New_York
```

## Architecture

```
┌─────────────┐     ┌───────────────┐     ┌──────────────┐
│   Internet  │────▶│  Caddy :443   │────▶│  Frontend    │
│             │     │  (auto-SSL)   │     │  (nginx)     │
└─────────────┘     └───────────────┘     └──────┬───────┘
                                                 │
                    ┌────────────────────────────┼────────────────────────────┐
                    │                            │                            │
              ┌─────▼─────┐              ┌───────▼───────┐            ┌───────▼───────┐
              │  Backend  │              │   Scheduler   │            │    Workers    │
              │ (gunicorn)│              │               │            │ (queue-short) │
              └─────┬─────┘              └───────────────┘            │ (queue-long)  │
                    │                                                 └───────────────┘
          ┌─────────┼─────────┐
          │         │         │
    ┌─────▼───┐ ┌───▼───┐ ┌───▼───┐
    │ MariaDB │ │ Redis │ │ Redis │
    │   DB    │ │ Cache │ │ Queue │
    └─────────┘ └───────┘ └───────┘
```

**Components:**

- **Caddy** - Reverse proxy with automatic HTTPS
- **Frontend** - Nginx serving static files
- **Backend** - Gunicorn running Frappe/ERPNext
- **Scheduler** - Background job scheduler
- **Workers** - Queue processors (short and long running)
- **MariaDB** - Database
- **Redis** - Caching and job queues

## Commands

### Backup & Restore

```bash
# Create site backup
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
tfgrid-compose logs backend
tfgrid-compose logs scheduler
tfgrid-compose logs worker
tfgrid-compose logs db
tfgrid-compose logs redis
tfgrid-compose logs caddy

# Follow logs
tfgrid-compose logs backend --follow
```

### Management

```bash
# Open backend shell
tfgrid-compose shell

# Run bench commands
tfgrid-compose bench --site erp.example.com list-apps
tfgrid-compose bench --site erp.example.com migrate
tfgrid-compose bench --site erp.example.com clear-cache

# Run database migrations
tfgrid-compose migrate

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
| Disk | 50 GB | 100 GB |

**Estimated Cost:** $30-50/month on ThreeFold Grid

## Initial Setup Wizard

After first login, ERPNext guides you through:

1. **Company Setup** - Name, abbreviation, country
2. **Chart of Accounts** - Select template for your country
3. **Fiscal Year** - Set your financial year
4. **Currency** - Default currency
5. **Users** - Add initial users

## Bench Commands

The `bench` command is the primary management tool for Frappe/ERPNext:

### Common Commands

```bash
# List installed apps
tfgrid-compose bench --site erp.example.com list-apps

# Clear cache
tfgrid-compose bench --site erp.example.com clear-cache

# Run migrations
tfgrid-compose bench --site erp.example.com migrate

# Reset admin password
tfgrid-compose bench --site erp.example.com set-admin-password newpassword

# Enable/disable maintenance mode
tfgrid-compose bench --site erp.example.com set-maintenance-mode on
tfgrid-compose bench --site erp.example.com set-maintenance-mode off

# Rebuild assets
tfgrid-compose bench --site erp.example.com build

# Show site config
tfgrid-compose bench --site erp.example.com show-config
```

### Installing Additional Apps

```bash
# Get an app
tfgrid-compose bench get-app hrms

# Install app on site
tfgrid-compose bench --site erp.example.com install-app hrms
```

Popular apps:
- **HRMS** - Human Resource Management
- **Payments** - Payment gateway integrations
- **eCommerce** - Online store integration
- **Healthcare** - Hospital management

## Backup Details

### What's Backed Up

- **Database** - Full MariaDB dump
- **Public files** - Attachments, images
- **Private files** - Reports, backups
- **Configuration** - Site config

### Backup Location

Backups stored in `/opt/erpnext/backups/`:
```
erpnext_backup_<sitename>_YYYYMMDD_HHMMSS.tar.gz
```

### Automated Backups

ERPNext has built-in scheduled backups. Configure in:
**Settings → System Settings → Backup**

Or set up cron:
```bash
0 2 * * * /opt/erpnext/scripts/backup.sh
```

## SSL Configuration

### Automatic SSL (Default)

Caddy automatically obtains Let's Encrypt certificates.

### Requirements

1. Domain DNS points to server IP
2. Ports 80 and 443 accessible
3. Valid domain name

### Local Development

For local development:
```bash
DOMAIN=localhost
```

## Troubleshooting

### Common Issues

**"Site not found" error**

ERPNext takes 5-10 minutes to initialize. Wait and retry.

Check initialization status:
```bash
tfgrid-compose logs backend
```

**Slow performance**

1. Check memory usage:
   ```bash
   free -h
   ```
2. Ensure Redis is running:
   ```bash
   tfgrid-compose logs redis
   ```
3. Clear cache:
   ```bash
   tfgrid-compose bench --site erp.example.com clear-cache
   ```

**Database connection errors**

1. Check MariaDB status:
   ```bash
   tfgrid-compose logs db
   ```
2. Verify credentials in `/opt/erpnext/.env`

**Background jobs not running**

Check scheduler and workers:
```bash
tfgrid-compose logs scheduler
tfgrid-compose logs worker
```

Restart if needed:
```bash
tfgrid-compose restart
```

**PDF generation fails**

Install wkhtmltopdf (usually included):
```bash
tfgrid-compose shell
apt-get install wkhtmltopdf
```

### Health Check

```bash
tfgrid-compose healthcheck
```

Checks:
- Docker service
- All container status
- HTTP response
- Disk space

## Updating ERPNext

### Minor Updates

```bash
# Pull latest images
cd /opt/erpnext/frappe_docker
docker compose -f pwd.yml pull

# Restart containers
docker compose -f pwd.yml up -d

# Run migrations
tfgrid-compose migrate
```

### Major Version Upgrades

1. **Backup first**:
   ```bash
   tfgrid-compose backup
   ```

2. **Update version in .env**:
   ```bash
   ERPNEXT_VERSION=v16
   ```

3. **Pull and restart**:
   ```bash
   cd /opt/erpnext/frappe_docker
   docker compose -f pwd.yml pull
   docker compose -f pwd.yml up -d
   ```

4. **Run migrations**:
   ```bash
   tfgrid-compose migrate
   ```

## Data Import

### From CSV

1. Go to the DocType list (e.g., Customer)
2. Click **Menu → Import Data**
3. Download template
4. Fill in data
5. Upload and import

### From Another ERPNext

1. Export data using Data Export tool
2. Import using Data Import tool

## Customization

### Custom Fields

**Settings → Customize Form**

Add fields to any DocType without coding.

### Custom Scripts

**Settings → Client Script**

Add JavaScript for client-side customization.

### Custom Reports

**Settings → Report Builder**

Create custom reports with filters and charts.

## Security Best Practices

1. **Strong admin password** - Change default immediately
2. **Role-based access** - Configure user permissions
3. **Regular backups** - Enable scheduled backups
4. **Keep updated** - Apply security patches
5. **HTTPS only** - Already enabled by default
6. **Audit logs** - Review activity logs regularly

## Integration

### REST API

ERPNext provides a full REST API:
```bash
# Get list
curl https://erp.example.com/api/resource/Customer

# Get single record
curl https://erp.example.com/api/resource/Customer/CUST-0001

# Create record
curl -X POST https://erp.example.com/api/resource/Customer \
  -H "Authorization: token api_key:api_secret" \
  -d '{"customer_name": "New Customer"}'
```

### Webhooks

Configure webhooks in:
**Settings → Webhook**

## Related Documentation

- [Custom Apps Guide](custom-apps.md)
- [TFGrid Compose Reference](tfgrid-compose.md)
- [ERPNext Documentation](https://docs.erpnext.com/)
- [Frappe Framework](https://frappeframework.com/docs)
