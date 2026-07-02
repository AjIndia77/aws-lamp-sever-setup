# 01. EC2 Instance Setup

Set up the web server foundation on an AWS Free Tier account using EC2.

## 1. Launch EC2 Instance

Go to **AWS Console → EC2 → Launch Instance** and configure:

| Setting | Value |
|---|---|
| Name | LAMP-setup-server |
| Region | ap-south-1a |
| OS | Ubuntu 24.04 LTS *(Free Tier eligible)* |
| Instance type | t2.micro *(Free Tier eligible)* |
| Key pair | Create new, download the `.pem` file, and store it safely |

### Network Settings (Security Group)

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | My IP |
| HTTP | TCP | 80 | Anywhere |
| HTTPS | TCP | 443 | Anywhere |

Click **Launch Instance**.

## 2. Connect to the Instance

Open your terminal and connect using SSH with the downloaded key pair:

```bash
ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP
```

> Replace `your-key.pem` with your actual key file name and `YOUR_PUBLIC_IP` with the instance's public IP from the EC2 dashboard.

## 3. Update the System

```bash
sudo apt update
sudo apt upgrade -y
```

Check the Ubuntu version to confirm it's correct:

```bash
lsb_release -a
```

## 4. Install Required Utilities

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

---
**Next:** [02. Apache Installation →](./02-apache-installation.md)