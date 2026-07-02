# 08. Security Groups

AWS Security Groups act as a virtual firewall for the EC2 instance, controlling inbound and outbound traffic at the instance level.

## What Is a Security Group?

A Security Group is **stateful** — meaning if an inbound rule allows a request, the matching outbound response is automatically allowed too, without needing a separate outbound rule.

## Inbound Rules Used in This Project

| Type | Protocol | Port | Source | Purpose |
|---|---|---|---|---|
| SSH | TCP | 22 | My IP | Secure server login/administration |
| HTTP | TCP | 80 | Anywhere (0.0.0.0/0) | Public website access |
| HTTPS | TCP | 443 | Anywhere (0.0.0.0/0) | Secure website access (post-SSL) |
| FTP | TCP | 21 | My IP | File uploads via vsftpd |

## Why "My IP" Instead of "Anywhere" for SSH/FTP

Restricting SSH and FTP to a specific IP significantly reduces the attack surface. Leaving port 22 open to `0.0.0.0/0` exposes the server to constant automated brute-force login attempts from bots scanning the internet. Since this server is managed from a known location, scoping access to "My IP" is a simple, effective hardening step.

> **Trade-off:** If your IP changes (e.g., different network, mobile hotspot), you'll need to update the security group rule before you can SSH in again. For a production team environment, a bastion host or VPN would be a more scalable solution.

## How to Update a Security Group Rule

1. Go to **EC2 → Security Groups** in the AWS Console.
2. Select the security group attached to the instance.
3. Under **Inbound Rules**, click **Edit inbound rules**.
4. Update the **Source** field for the relevant rule (e.g., change "My IP" if your IP has changed).
5. Save.

## Verifying Open Ports from the Server

To confirm which ports are actually listening on the instance itself (as opposed to what the Security Group allows):

```bash
sudo ss -tlnp
```

This is useful for cross-checking that a service (Apache, vsftpd, etc.) is actually bound to the port the Security Group has opened — a Security Group rule alone doesn't guarantee a service is running.

---
**Previous:** [← 07. FTP User Setup](./07-ftp-user-setup.md) | **Next:** [09. SSL Installation →](./09-ssl-installation.md)