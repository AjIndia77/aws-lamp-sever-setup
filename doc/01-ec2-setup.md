## Setup webserver on AWS free tier account 
AWS EC2 instance service
Created instance on ap-south-1a with:
- Name - LAMP-setup-server
- OS - Ubuntu 24.04 LTS *(free tier eligible)*
- instance type - t2.micro *(free tier eligible)*
- key pair - create and download the key pair 
- Network settings - allow SSH on MyIP - port 22
                     allow HTTPS on Aywhere - port 443
                     allow HTTP on Anywhere - port 80
- luanch the instance
## After Succesfully creating EC2 instance 
1. open system *Terminal* to give commands
2.  Use ssh client login credential given by AWS or use 
```bash
ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP
```
- Update Ubuntu:
```bash
sudo apt update
sudo apt upgrade -y
```

- Check Ubuntu version
```bash
lsb_release -a
```
- Install Required Utilities
```bash
sudo apt install -y \
software-properties-common \
ca-certificates \
curl \
wget \
unzip \
zip \
git
```