## Install MySQL Server

- Install MySQL:
```bash
sudo apt install mysql-server -y
```
- Check if it's running:
```bash
sudo systemctl status mysql
```
It should show:
Active: active (running)

- Enable it to start on boot:
```bash
sudo systemctl enable mysql
```
For secure MySQL run:
```bash
sudo mysql_secure_installation
```
If prompted about the password validation component, you can usually answer *No*.
For the remaining prompts:

1. Remove anonymous users → *Yes*
2. Disallow remote root login → *Yes*
3. Remove test database → *Yes*
4. Reload privilege tables → *Yes*

- Verify MySQL
Login:
```bash
sudo mysql
```
You should see:
Welcome to the MySQL monitor.

- Check the version:
```sh
SELECT VERSION();
```
Exit:
```sh
exit;
```