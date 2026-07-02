# AWS LAMP Server Setup

[![AWS](https://img.shields.io/badge/AWS-EC2-orange)](https://aws.amazon.com/ec2/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420)](https://ubuntu.com/)
[![PHP](https://img.shields.io/badge/PHP-7.4_%7C_8.1-777BB4)](https://www.php.net/)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

Production-ready LAMP (Linux, Apache, MySQL, PHP) web server deployed and configured on AWS EC2, with Node.js added for modern app support.

## Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Setup Guide](#setup-guide)
- [Lessons Learned](#lessons-learned)
- [Challenges & Solutions](#challenges--solutions)
- [Author](#author)

## Project Overview

This project demonstrates deploying and configuring a production-ready Ubuntu web server on AWS EC2 using the LAMP stack.

**Stack:**
```bash
AWS EC2
   | 
Ubuntu 24.04 LTS
   |
Apache
   | 
PHP 7.4 & 8.1
   |
MySQL
   | 
phpMyAdmin
   | 
Node.js LTS 
   |
FTP User
```
**Use cases:** 
WordPress sites, Laravel/CodeIgniter applications, CRMs, admin panels, school management systems, insurance portals, and general company websites.

**Request flow:**
Developer → Upload Code → Apache → PHP Executes → MySQL Stores Data → User Opens Website

## Architecture
```bash
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
┌─┴─┐
▼   ▼
PHP  Node.js
│
▼
MySQL
│
▼
phpMyAdmin
```

## Technologies Used

| Category | Tools |
|---|---|
| Cloud | AWS EC2, Security Groups |
| OS | Ubuntu 24.04 LTS |
| Web Server | Apache2 |
| Backend | PHP 7.4 & 8.1, Node.js LTS |
| Database | MySQL, phpMyAdmin |
| File Transfer | vsftpd |

## Setup Guide

Full step-by-step installation docs are in [`/doc`](./doc):

1. [EC2 Instance Setup](./doc/01-ec2-setup.md)
2. [Apache Installation](./doc/02-apache-installation.md)
3. [PHP Installation](./doc/03-php-installation.md)
4. [MySQL Installation](./doc/04-mysql-installation.md)
5. [Node.js Installation](./doc/05-nodejs-installation.md)
6. [phpMyAdmin Installation](./doc/06-phpmyadmin-installation.md)
7. [FTP User Setup](./doc/07-ftp-user-setup.md)

Quick reference commands: [`commands/setup-commands.md`](./commands/setup-commands.md)
Apache config files: [`configs/apache`](./configs/apache)

## Lessons Learned

- Linux system administration
- Apache configuration and virtual hosts
- Managing multiple PHP versions on one server
- MySQL administration
- phpMyAdmin installation and troubleshooting
- AWS security groups and EC2 networking
- FTP user management
- Debugging package/version compatibility issues

## Challenges & Solutions

**1. PHP version compatibility on Ubuntu 26.04**

The project required PHP 7.4 and 8.1, but Ubuntu 26.04 LTS ships PHP 8.5 by default — creating a version mismatch.

*Fix:* Checked available versions via APT, confirmed 26.04 didn't natively support the required PHP versions, and re-provisioned the instance on Ubuntu 24.04 LTS instead.

**2. phpMyAdmin vs. PHP 8.1 compatibility**

On Ubuntu 24.04, the default phpMyAdmin package required PHP 8.2+, but the project needed PHP 8.1.

*Fix:* Verified dependencies with `apt policy` / `apt search`, checked repo availability, and used a manual phpMyAdmin installation instead of the default Ubuntu package.

## Author

**[Ambika Joshi]**
[GitHub](https://github.com/AjIndia77) · [LinkedIn](https://www.linkedin.com/in/ambika-joshi-aj/) · [Email](https://mail.com/indiaaj282@gmail.com)