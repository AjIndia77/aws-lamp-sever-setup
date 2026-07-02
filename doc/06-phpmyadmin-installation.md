# 06. phpMyAdmin Installation

> ⚠️ On Ubuntu 24.04, the phpMyAdmin package from the default repository requires PHP 8.2+, which conflicts with this project's PHP 8.1 requirement. This doc walks through diagnosing that and installing phpMyAdmin manually instead.

## 1. Check the Default Repository Package

```bash
apt policy phpmyadmin
```

Output:
phpmyadmin:
Installed: 4:5.2.1+dfsg-3
Candidate: 4:5.2.1+dfsg-3
Version table:
*** 4:5.2.1+dfsg-3 500
500 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu noble/universe amd64 Packages
100 /var/lib/dpkg/status

This is the Ubuntu repository package, which requires PHP 8.2+ on Ubuntu 24.04.

## 2. Confirm No Compatible Alternative Exists

Check enabled repositories:
```bash
grep -R ^deb /etc/apt/sources.list.d/
```

Check available package versions:
```bash
apt-cache madison phpmyadmin
```

Search for alternatives:
```bash
apt search phpmyadmin
```

**Findings:**
1. Ubuntu 24.04 (noble) only ships phpMyAdmin 5.2.1 in its repository.
2. That package requires PHP 8.2+ due to an Ubuntu packaging decision.
3. No PHP 8.1–compatible phpMyAdmin package exists in the Ubuntu repository.

## 3. Install phpMyAdmin Manually

Since PHP 8.1 is the version chosen for this project, install **phpMyAdmin 5.2.3** directly — it officially supports PHP 7.2 and newer, making it compatible with 8.1.

```bash
cd /tmp
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz
```

Extract it:
```bash
tar -xzf phpMyAdmin-5.2.3-all-languages.tar.gz
```

Move it into Apache's web root:
```bash
sudo mv phpMyAdmin-5.2.3-all-languages /var/www/html/phpmyadmin
```

Set correct ownership and permissions:
```bash
sudo chown -R www-data:www-data /var/www/html/phpmyadmin
sudo chmod -R 755 /var/www/html/phpmyadmin
```

## 4. Create the Configuration File

```bash
cd /var/www/html/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
```

Generate a secure blowfish secret (used to encrypt session data):
```bash
openssl rand -base64 32
```

Copy the generated string, then edit the config:
```bash
sudo nano /var/www/html/phpmyadmin/config.inc.php
```

Find:
```php
$cfg['blowfish_secret'] = '';
```

Replace with:
```php
$cfg['blowfish_secret'] = 'PASTE_THE_RANDOM_STRING_HERE';
```

Save the file.

## 5. Restart Apache and Test

```bash
sudo systemctl restart apache2
```

Open in browser:
http://YOUR_PUBLIC_IP/phpmyadmin

You should see the phpMyAdmin login portal.

## 6. Create a Login-Capable MySQL User

Login to MySQL:
```bash
sudo mysql
```

Check the root user's authentication method:
```sql
SELECT user, host, plugin FROM mysql.user;
```

If `root` uses `auth_socket`, phpMyAdmin (which logs in with a username/password over the web) won't be able to authenticate as root. Create a dedicated admin user instead:

```sql
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'YourSecurePassword';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

> Replace `YourSecurePassword` with your own strong password — never commit real credentials to a public repository.

Log into phpMyAdmin using:
- **Username:** `admin`
- **Password:** *(the one you set above)*

---
**Previous:** [← 05. Node.js Installation](./05-nodejs-installation.md) | **Next:** [07. FTP User Setup →](./07-ftp-user-setup.md)