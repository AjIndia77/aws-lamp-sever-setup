## ⚠️ Before installing, don't run the installer yet. On Ubuntu 24.04, the package isn't always available from the default repositories.

First check:
```bash
apt policy phpmyadmin
```
it give output as 
```bash
phpmyadmin: 
Installed: 4:5.2.1+dfsg-3 
Candidate: 4:5.2.1+dfsg-3 
Version table: 
*** 4:5.2.1+dfsg-3 500 500 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu noble/universe amd64 
Packages 100 /var/lib/dpkg/status
```
This is the Ubuntu repository package, which on Ubuntu 24.04 requires PHP 8.2+.

- Check what repositories are enabled
```bash
grep -R ^deb /etc/apt/sources.list.d/
```
```bash
apt-cache madison phpmyadmin
```
Gives output 
```sh
phpmyadmin | 4:5.2.1+dfsg-3 | http://ap-south-1.ec2.archive.ubuntu.com/ubuntu noble/universe amd64 Packages
```
Also search for available phpMyAdmin packages
```bash
apt search phpmyadmin
```
Output
```sh
Sorting... Done 
Full Text Search... Done 
adminer/noble 4.8.1-2 all Web-based database administration tool 
php-phpmyadmin-motranslator/noble,now 5.3.1-1 all [installed,automatic] translation API for PHP using Gettext MO files 
php-phpmyadmin-shapefile/noble,now 3.0.2-1 all [installed,automatic] ESRI ShapeFile library for PHP 
php-phpmyadmin-sql-parser/noble,now 5.8.2-1 all [installed,automatic] validating SQL lexer and parser 
phpliteadmin/noble 1.9.8.2-2 all web-based SQLite database admin tool 
phpliteadmin-themes/noble 1.9.8.2-2 all web-based SQLite database admin tool - themes 
phpmyadmin/noble,now 4:5.2.1+dfsg-3 all [installed] MySQL web administration tool
```
- From the output
1. Ubuntu 24.04 (noble) only has phpMyAdmin 5.2.1 in its repository.
2. That package requires PHP 8.2+ because of an Ubuntu packaging issue.
3. There is no alternate phpMyAdmin package for PHP 8.1 in the Ubuntu repository.

Since default PHP 8.1 version is choosen

Use phpMyAdmin 5.2.3, which supports PHP 7.2 and newer, so it's compatible with PHP 8.1.
```bash
cd /tmp

wget https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz
```
If that succeeds, Extract it:
```bash
tar -xzf phpMyAdmin-5.2.3-all-languages.tar.gz
```
Move it into Apache's web root:
```bash
sudo mv phpMyAdmin-5.2.3-all-languages /var/www/html/phpmyadmin
```
Set permissions
```bash
sudo chown -R www-data:www-data /var/www/html/phpmyadmin
sudo chmod -R 755 /var/www/html/phpmyadmin
```
### Create the configuration
```bash
cd /var/www/html/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
```
- Generate a secure **blowfish secret**:
```bash
openssl rand -base64 32
```
Copy the generated string.
Edit the configuration:
```bash
sudo nano /var/www/html/phpmyadmin/config.inc.php
```
Find:
```sh
$cfg['blowfish_secret'] = '';
```
Replace it with:
```sh
$cfg['blowfish_secret'] = 'PASTE_THE_RANDOM_STRING_HERE';
```
Save the file.

- Restart Apache
```bash
sudo systemctl restart apache2
```
Test

Open: *http://YOUR_PUBLIC_IP/phpmyadmin*

It shows phpmyadmin portal running.....
Now login
```bash
sudo mysql
```
On MySQL ->
```bash
SELECT user,host,plugin FROM mysql.user;
```
If it shows *root* user uses *auth_socket* then phpmyadmin will not login, then create a new mysql user
```bash
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'Password@123';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
At phpmyadmin write

Username: *admin*

Password: *Password@123*
