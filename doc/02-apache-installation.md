## Apache Installion 
Go through to documention of Apche installation on Ubuntu
- Check Apache service 
```bash
apt policy apache2
```
- Install Apache service
```bash 
sudo apt install apache2 -y
```
- Start and enable service
```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```
- Check the activation by
```bash
sudo systemctl status apache2
sudo ss -tinp
```
- Check - http://your-public-ip

You should see
```sh
Apache2 Ubuntu Default Page
```
