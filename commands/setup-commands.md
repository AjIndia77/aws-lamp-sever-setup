# Setup Commands — Quick Reference

A consolidated, copy-paste-ready reference of all commands used across this project. For explanations, troubleshooting, and context behind each step, see the corresponding file in [`/docs`](../docs).

---

## 1. System Update & Utilities

```bash
sudo apt update
sudo apt upgrade -y
lsb_release -a

sudo apt install -y \
  software-properties-common \
  ca-certificates \
  curl \
  wget \
  unzip \
  zip \
  git
```

## 2. Apache

```bash
apt policy apache2
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
sudo ss -tlnp
```

## 3. PHP (7.4 & 8.1)

```bash
apt policy php

sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

sudo apt install -y \
  php8.1 php8.1-cli php8.1-common php8.1-mysql php8.1-curl \
  php8.1-xml php8.1-mbstring php8.1-zip php8.1-gd \
  libapache2-mod-php8.1 php8.1-fpm

sudo apt install -y \
  php7.4 php7.4-cli php7.4-common php7.4-mysql php7.4-curl \
  php7.4-xml php7.4-mbstring php7.4-zip php7.4-gd \
  libapache2-mod-php7.4 php7.4-fpm

php8.1 -v
php7.4 -v
dpkg -l | grep php

sudo update-alternatives --config php
php -v
```

## 4. MySQL

```bash
sudo apt install mysql-server -y
sudo systemctl status mysql
sudo systemctl enable mysql
sudo mysql_secure_installation

sudo mysql
```
```sql
SELECT VERSION();
exit;
```

## 5. Node.js LTS

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y
node -v
npm -v
```

## 6. phpMyAdmin (Manual Install — PHP 8.1 Compatible)

```bash
cd /tmp
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz
tar -xzf phpMyAdmin-5.2.3-all-languages.tar.gz
sudo mv phpMyAdmin-5.2.3-all-languages /var/www/html/phpmyadmin

sudo chown -R www-data:www-data /var/www/html/phpmyadmin
sudo chmod -R 755 /var/www/html/phpmyadmin

cd /var/www/html/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
openssl rand -base64 32
sudo nano /var/www/html/phpmyadmin/config.inc.php

sudo systemctl restart apache2
```
```sql
-- Inside mysql shell:
SELECT user, host, plugin FROM mysql.user;
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'YourSecurePassword';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

## 7. FTP (vsftpd)

```bash
sudo apt update
sudo apt install vsftpd -y
sudo systemctl enable vsftpd
sudo systemctl start vsftpd
sudo systemctl status vsftpd

sudo adduser ftpuser
sudo usermod -aG www-data ftpuser

sudo nano /etc/vsftpd.conf
# chroot_local_user=YES
# allow_writeable_chroot=YES

sudo systemctl restart vsftpd
```

## 8. SSL (Certbot — Pending Domain)

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d YOUR_DOMAIN -d www.YOUR_DOMAIN
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

## 9. General Diagnostics

```bash
sudo systemctl status <service>
sudo ss -tlnp
apt policy <package>
apt search <package>
dpkg -l | grep <name>
sudo journalctl -u <service> -f
sudo tail -f /var/log/apache2/error.log
```

---

> ⚠️ Replace all placeholders (`YOUR_PUBLIC_IP`, `YOUR_DOMAIN`, `YourSecurePassword`, etc.) with your actual values. Never commit real credentials, IPs, or `.pem` keys to this repository.
