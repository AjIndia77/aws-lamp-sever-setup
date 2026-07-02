# AWS-LAMP-Sever-Setup
## Project Overview

This project demonstrates how to deploy and configure a production-ready Ubuntu web server on AWS EC2 using the LAMP stack.

### Built an AWS Ubuntu Web Server with the following stack:
```bash
AWS EC2
     │
Ubuntu 24.04 LTS
     │
Apache Web Server
     │
PHP 7.4 & PHP 8.1
     │
MySQL Database
     │
phpMyAdmin
     │
Node.js LTS
     │
FTP User
     │
SSL 
```
This is called a LAMP Stack (Linux + Apache + MySQL + PHP) with Node.js added.

### What is it used for?

This server can host PHP applications such as:

1. WordPress websites
2. Insurance portals 
3. Laravel applications
4. CodeIgniter applications
5. CRM systems
6. Admin Panels
7. School Management Systems
8. Company websites

### The flow is:
```sh
Developer
      │
      │ Upload Code
      ▼
Apache
      │
PHP Executes Code
      │
MySQL Stores Data
      │
User Opens Website
```

### Architecture
```sh
Internet
      │
      ▼
AWS Security Group
      │
      ▼
Ubuntu EC2 Instance
      │
      ▼
Apache Web Server
      │
 ┌────┴────┐
 ▼         ▼
PHP      Node.js
 │
 ▼
MySQL
 │
 ▼
phpMyAdmin
```
### Technologies Used
1. AWS EC2
2. Ubuntu 24.04 LTS
3. Apache2
4. PHP
5. MySQL
6. Node.js
7. phpMyAdmin
8. vsftpd
9. Linux
10. Installation Guide  See [*doc*](https://github.com/AjIndia77/aws-lamp-sever-setup/tree/main/doc) for detailed installation steps.

### Lessons Learned

During this project I learned:

- Linux System Administration
- Apache Configuration
- Managing Multiple PHP Versions
- MySQL Administration
- phpMyAdmin Installation
- AWS Security Groups
- EC2 Networking
- FTP User Management
- SSL Deployment
- Troubleshooting Package Compatibility

### Challenges

*Challenge 1: PHP Version Compatibility on Ubuntu 26.04*

Initially, an AWS EC2 instance was created using Ubuntu 26.04 LTS. However, the project required PHP 7.4 and PHP 8.1, while Ubuntu 26.04 provided PHP 8.5 by default. This caused a version mismatch and made it difficult to meet the project requirements.

*Solution*

- Verified the available PHP versions using APT package repositories.
- Identified that Ubuntu 26.04 did not natively support the required PHP versions.
- Re-created the server using Ubuntu 24.04 LTS for better compatibility.

*Challenge 2: phpMyAdmin and PHP 8.1 Compatibility*

After switching to Ubuntu 24.04 LTS, another issue was encountered: the default phpMyAdmin package available in the Ubuntu repository required PHP 8.2 or later, while the project requirement was PHP 8.1.

*Solution*

- Checked package dependencies using apt policy and apt search.
- Verified repository availability and version compatibility.
- Explored manual phpMyAdmin installation methods instead of relying solely on the default Ubuntu package.
- Documented the compatibility issue and the troubleshooting approach.