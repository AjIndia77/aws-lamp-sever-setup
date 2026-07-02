# 02. Apache Installation

Install and configure the Apache web server on Ubuntu.

## 1. Check Available Apache Version

```bash
apt policy apache2
```

This shows the Apache version available in the Ubuntu repository before installing.

## 2. Install Apache

```bash
sudo apt install apache2 -y
```

## 3. Start and Enable the Service

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

`start` runs Apache now; `enable` makes it start automatically on every server reboot.

## 4. Verify Apache Is Running

```bash
sudo systemctl status apache2
sudo ss -tlnp
```

`systemctl status` confirms the service is active, and `ss -tlnp` confirms Apache is listening on port 80.

## 5. Test in Browser

Open your instance's public IP in a browser: *** http://YOUR_PUBLIC_IP***

You should see the default Apache landing page:

Apache2 Ubuntu Default Page

![Apache Default Page](../screenshots/[apache-default-page].png)

---
**Previous:** [← 01. EC2 Setup](./01-ec2-setup.md) | **Next:** [03. PHP Installation →](./03-php-installation.md)