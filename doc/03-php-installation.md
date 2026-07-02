### PHP Installation 
Server required PHP versions are - 8.1 and 7.4

Before installing do software check
```sh
apt policy php
```
it shows:
- package avalibility
- version
- which repository it comes from

On Ubuntu 24.04 - PHP 8.1, PHP 7.4 is not available in the default repository. You'll need to add the Ondřej Surý PHP repository, which provides multiple PHP versions.
```bash
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```
Again check the software
- If add-apt-repository is not found
Install it first:
```bash
sudo apt install software-properties-common -y
```
Then retry.

- If it shows, install both version
Run for php 8.1:
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
Now install PHP 7.4:
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
Step 9: Verify the Installation
php8.1 -v
php7.4 -v
```
- Check Installed PHP Packages
```bash
dpkg -l | grep php
```
This confirms all the required PHP modules are present.

- Now set the **Default PHP Version**

Check the available alternatives:
```bash 
sudo update-alternatives --config php
```

*Example output:*
```bash
Selection    Path
--------------------------------
0            /usr/bin/php8.1
1            /usr/bin/php7.4
2            /usr/bin/php8.1
```
Choose any version as the default.

- Verify:
```bash
php -v
```
- Check Current PHP Version Used by Apache

Create a test file:
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Then open:
```bash
http://YOUR_PUBLIC_IP/info.php
```
It will tell us whether Apache is currently serving PHP 7.4 or PHP 8.1.