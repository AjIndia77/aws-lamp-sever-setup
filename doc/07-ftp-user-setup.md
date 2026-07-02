## Install vsftpd
```bash
sudo apt update
sudo apt install vsftpd -y
```
Enable service:
```bash
sudo systemctl enable vsftpd
sudo systemctl start vsftpd
```
Check status:
```bash
sudo systemctl status vsftpd
```
Create FTP User
```bash
sudo adduser ftpuser
```
Set *Password*.

Home directory created automatically.

Give Web Directory Access

If user want to manage website
```bash
sudo usermod -aG www-data ftpuser
```
Check permissions according to requirement.