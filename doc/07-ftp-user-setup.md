# 07. FTP User Setup

Set up FTP access using `vsftpd` (Very Secure FTP Daemon) for file uploads to the web server.

## 1. Install vsftpd

```bash
sudo apt update
sudo apt install vsftpd -y
```

## 2. Enable and Start the Service

```bash
sudo systemctl enable vsftpd
sudo systemctl start vsftpd
```

Check status:
```bash
sudo systemctl status vsftpd
```

## 3. Create an FTP User

```bash
sudo adduser ftpuser
```

Set a password when prompted. A home directory is created automatically.

> Use a strong password here — never commit real credentials to your repo or docs. If sharing this project, use a placeholder like `YourSecurePassword`.

## 4. Grant Web Directory Access

If this user needs to manage website files:

```bash
sudo usermod -aG www-data ftpuser
```

Check and adjust ownership/permissions on `/var/www/html` as needed for the project.

## 5. Restrict FTP User to Their Home Directory (chroot)

By default, an FTP user can navigate the entire filesystem. To lock them into their home directory, edit the vsftpd config:

```bash
sudo nano /etc/vsftpd.conf
```

Ensure these lines are set:
chroot_local_user=YES
allow_writeable_chroot=YES

Restart the service to apply changes:
```bash
sudo systemctl restart vsftpd
```

## 6. Allow FTP Through the Security Group

In the AWS EC2 console, add an inbound rule for **port 21** (FTP control) to the instance's security group, scoped to your IP or as required.

> **Note:** Plain FTP transmits credentials and files unencrypted. For a production setup, prefer **SFTP** (over SSH, port 22 — already open) or configure `vsftpd` with SSL/TLS (`ssl_enable=YES`) instead of plain FTP.

## 7. Test the Connection

From a local machine:
```bash
ftp YOUR_PUBLIC_IP
```

Log in with the `ftpuser` credentials and confirm you can list/upload files.

---
**Previous:** [← 06. phpMyAdmin Installation](./06-phpmyadmin-installation.md)