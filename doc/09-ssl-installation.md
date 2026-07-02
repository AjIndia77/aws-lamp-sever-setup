# 09. SSL Installation

> **Status: Planned — Pending Domain Assignment**
> SSL/TLS setup requires a registered domain name pointed at this server. A domain is being provided separately; this document outlines the exact steps that will be followed once it's available, and will be updated with real output once completed.

## Why SSL Is Needed

Currently, the server is accessible over plain HTTP, which transmits all data — including any login forms or submitted data — unencrypted. SSL/TLS (via HTTPS) encrypts traffic between the client and server, which is:

- **Expected by users** — browsers flag HTTP-only sites as "Not Secure"
- **Required for many features** — modern APIs, service workers, and payment gateways often require HTTPS
- **A basic production requirement** — no real deployment should run public-facing HTTP-only in 2026

## Prerequisite: A Domain Name

SSL certificates (via Let's Encrypt, the free option used here) are issued for a **domain name**, not an IP address. This means:

1. A domain (e.g., `example.com` or a subdomain) needs to be pointed at this EC2 instance's public IP via a DNS **A record**.
2. DNS propagation must complete before certificate issuance will succeed.

Once the domain is provided, this section will be filled in with the actual domain used and DNS configuration steps taken.

## Planned Steps

### 1. Point the Domain to the Server

Create an A record in the domain's DNS settings:

| Type | Name | Value |
|---|---|---|
| A | `@` or subdomain | EC2 instance's public IP |

Verify propagation:
```bash
dig YOUR_DOMAIN +short
```

### 2. Install Certbot

Certbot automates obtaining and renewing Let's Encrypt certificates, and includes an Apache plugin that can configure HTTPS automatically.

```bash
sudo apt install certbot python3-certbot-apache -y
```

### 3. Obtain and Install the Certificate

```bash
sudo certbot --apache -d YOUR_DOMAIN -d www.YOUR_DOMAIN
```

Certbot will:
- Verify domain ownership
- Issue the SSL certificate
- Automatically update the Apache virtual host config to serve HTTPS
- Offer to redirect HTTP → HTTPS automatically

### 4. Verify Auto-Renewal

Let's Encrypt certificates expire every 90 days. Certbot installs a systemd timer to renew automatically — this just needs confirming:

```bash
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

### 5. Confirm HTTPS Is Working

- Visit `https://YOUR_DOMAIN` and confirm the padlock icon appears
- Confirm `http://YOUR_DOMAIN` redirects to `https://`
- Optionally test the SSL configuration quality at [SSL Labs](https://www.ssllabs.com/ssltest/)

## Security Group Update Required

Port **443** (HTTPS) must be open in the EC2 Security Group — this was already configured in [08. Security Groups](./08-security-groups.md), so no change should be needed here, just a confirmation.

---
**Previous:** [← 08. Security Groups](./08-security-groups.md) | **Next:** [10. Troubleshooting →](./10-troubleshooting.md)