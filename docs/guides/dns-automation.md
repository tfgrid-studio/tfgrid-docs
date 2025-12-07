# DNS Automation

tfgrid-compose can automatically create DNS A records for your deployments, eliminating manual DNS configuration and enabling faster, fully automated deployments.

## Overview

When you deploy an application like WordPress, Nextcloud, or ERPNext, tfgrid-compose needs to point your domain to the deployment's IP address. With DNS automation enabled, this happens automatically during deployment.

## Supported Providers

tfgrid-compose supports the following DNS providers for fully automated DNS management:

| Provider | Authentication | Setup Complexity | Propagation Speed |
|----------|---------------|------------------|-------------------|
| **Cloudflare** | API Token | Easy | ~30 seconds |
| **Name.com** | Username + API Token | Easy | 1-5 minutes |
| **GoDaddy** | API Key + Secret | Easy | 1-5 minutes |

All three providers offer full API access without IP whitelisting requirements.

!!! tip "Recommendation"
    **Cloudflare** is recommended for the best developer experience—fastest propagation, excellent API documentation, and free CDN/SSL included.

## Provider Setup

### Cloudflare

1. Create a free account at [cloudflare.com](https://cloudflare.com)
2. Add your domain to Cloudflare
3. Go to **My Profile** (top right) → **API Tokens** → **Create Token**
4. Use the **"Edit zone DNS"** template
5. Set permissions:
    - **Zone - DNS - Edit** (to create/update DNS records)
    - **Zone - Zone - Read** (to look up zone ID automatically)
6. Zone Resources: Select "Include - All zones" or a specific zone
7. Client IP Address Filtering: Leave blank (no IP whitelisting required)
8. TTL: Leave Start/End dates blank for a non-expiring token, or set an expiration for extra security
9. Click **Continue to summary** → **Create Token**
10. Copy the generated token (shown only once)

!!! note
    You only need the API token. The zone ID and account ID are looked up automatically using your domain name.

```bash
tfgrid-compose up tfgrid-wordpress \
  --env DOMAIN=blog.example.com \
  --env DNS_PROVIDER=cloudflare \
  --env CLOUDFLARE_API_TOKEN=your-token
```

### Name.com

1. Log into [name.com](https://name.com)
2. Go to **Account** → **API Token**
3. Generate a new API token
4. Note your username and token

```bash
tfgrid-compose up tfgrid-wordpress \
  --env DOMAIN=blog.example.com \
  --env DNS_PROVIDER=name.com \
  --env NAMECOM_USERNAME=your-username \
  --env NAMECOM_API_TOKEN=your-token
```

### GoDaddy

1. Log into [developer.godaddy.com](https://developer.godaddy.com)
2. Go to **API Keys** → **Create New API Key**
3. Select **Production** environment
4. Note your API key and secret

```bash
tfgrid-compose up tfgrid-wordpress \
  --env DOMAIN=blog.example.com \
  --env DNS_PROVIDER=godaddy \
  --env GODADDY_API_KEY=your-key \
  --env GODADDY_API_SECRET=your-secret
```

## Using Cloudflare with Any Registrar

If your domain is registered with a provider that has API limitations (such as IP whitelisting requirements), you can use Cloudflare as your DNS provider while keeping the domain at your current registrar.

This approach gives you:

- Full API access without restrictions
- Faster DNS propagation (~30 seconds)
- Free SSL certificates
- Free CDN and DDoS protection

### Setup Steps

1. **Create a Cloudflare account** at [cloudflare.com](https://cloudflare.com) (free)

2. **Add your domain to Cloudflare**
    - Click "Add a Site" in the dashboard
    - Enter your domain name
    - Select the Free plan
    - Cloudflare will scan and import existing DNS records

3. **Update nameservers at your registrar**
    - Cloudflare provides two nameservers (e.g., `anna.ns.cloudflare.com`)
    - Log into your registrar's dashboard
    - Change nameservers from default to Cloudflare's nameservers
    - Save changes

4. **Wait for propagation**
    - Nameserver changes typically take 10-30 minutes
    - Cloudflare will notify you when the domain is active

5. **Create an API token** and use Cloudflare for DNS automation

!!! note
    Your domain registration (ownership, renewal, transfers) stays with your original registrar. Only DNS management moves to Cloudflare.

## Manual DNS Setup

If you prefer not to use DNS automation, select `manual` as your DNS provider:

```bash
tfgrid-compose up tfgrid-wordpress \
  --env DOMAIN=blog.example.com \
  --env DNS_PROVIDER=manual
```

tfgrid-compose will display the required DNS record details:

```
Please create an A record with your DNS provider:

  Domain: blog.example.com
  Type:   A
  Value:  203.0.113.50
  TTL:    300 (or your preference)

After creating the record, wait 1-5 minutes for propagation.
```

## Security Best Practices

- **Scope API tokens** to specific zones/domains when possible
- **Store tokens securely** using environment variables or a secrets manager
- **Never commit tokens** to version control
- **Rotate tokens periodically** for production environments

## Troubleshooting

### DNS record not created

- Verify API credentials are correct
- Check that the domain/zone exists in your DNS provider account
- Ensure API token has the required permissions

### Domain not resolving

- DNS propagation can take up to 5 minutes (30 seconds for Cloudflare)
- Clear local DNS cache: `sudo systemd-resolve --flush-caches`
- Verify with: `dig @8.8.8.8 your-domain.com`

### API rate limits

- Cloudflare: 1200 requests per 5 minutes
- Name.com/GoDaddy: Lower limits, sufficient for normal use
- Avoid rapid repeated deployments to the same domain
