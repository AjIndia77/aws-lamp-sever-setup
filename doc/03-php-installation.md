# 03. PHP Installation

The server requires two PHP versions: **8.1** and **7.4** (to support different application requirements).

## 1. Check Available PHP Packages

```bash
apt policy php
```

This shows package availability, version, and which repository it comes from.

On Ubuntu 24.04, PHP 8.1 and PHP 7.4 aren't available in the default repository. You'll need to add the Ondřej Surý PHP repository, which provides multiple PHP versions.

## 2. Add the PHP Repository

```bash
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```

> If `add-apt-repository` isn't found, install `software-properties-common` first (command above), then retry.

Re-run `apt policy php` to confirm both versions are now available before proceeding.

## 3. Install PHP 8.1

```bash
sudo apt install -y \
  php8.1 \
  php8.1-cli \
  php8.1-common \
  php8.1-mysql \
  php8.1-curl \
  php8.1-xml \
  php8.1-mbstring \
  php8.1-zip \
  php8.1-gd \
  libapache2-mod-php8.1 \
  php8.1-fpm
```

## 4. Install PHP 7.4

```bash
sudo apt install -y \
  php7.4 \
  php7.4-cli \
  php7.4-common \
  php7.4-mysql \
  php7.4-curl \
  php7.4-xml \
  php7.4-mbstring \
  php7.4-zip \
  php7.4-gd \
  libapache2-mod-php7.4 \
  php7.4-fpm
```

## 5. Verify the Installation

```bash
php8.1 -v
php7.4 -v
```

Check all installed PHP packages:

```bash
dpkg -l | grep php
```

This confirms all the required PHP modules are present.

## 6. Set the Default PHP Version

Check the available alternatives:

```bash
sudo update-alternatives --config php
```

*Example output:*
Selection    Path
0            /usr/bin/php8.1
1            /usr/bin/php7.4
2            /usr/bin/php8.1

Choose the version you want as default, then verify:

```bash
php -v
```

## 7. Check Which PHP Version Apache Is Using

Create a test file:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Open in browser:
***http://YOUR_PUBLIC_IP/info.php***

This confirms whether Apache is currently serving PHP 7.4 or PHP 8.1.

> ⚠️ **Security note:** `phpinfo()` exposes detailed server configuration. Delete this file once you're done checking:
> ```bash
>sudo rm /var/www/html/info.php
> ```

---
**Previous:** [← 02. Apache Installation](./02-apache-installation.md) | **Next:** [04. MySQL Installation →](./04-mysql-installation.md)