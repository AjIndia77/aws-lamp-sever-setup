# 04. MySQL Installation

## 1. Install MySQL Server

```bash
sudo apt install mysql-server -y
```

## 2. Verify the Service

```bash
sudo systemctl status mysql
```

It should show:
Active: active (running)

Enable it to start automatically on boot:

```bash
sudo systemctl enable mysql
```

## 3. Secure the Installation

```bash
sudo mysql_secure_installation
```

This removes insecure defaults left over from a fresh install (anonymous users, test databases, remote root access).

If prompted about the password validation component, you can usually answer **No**.

For the remaining prompts:

| Prompt | Answer |
|---|---|
| Remove anonymous users | Yes |
| Disallow remote root login | Yes |
| Remove test database | Yes |
| Reload privilege tables | Yes |

> **Note:** On Ubuntu, MySQL's root user authenticates via `auth_socket` by default — meaning `sudo mysql` logs in using your Linux system credentials rather than a MySQL password. This is why no root password is set here. If an application needs a password-based MySQL user, create one separately with `CREATE USER` and `GRANT` rather than changing root's auth method.

## 4. Verify MySQL Login

```bash
sudo mysql
```

You should see:
Welcome to the MySQL monitor.

Check the version:

```sql
SELECT VERSION();
```

Exit:

```sql
exit;
```

---
**Previous:** [← 03. PHP Installation](./03-php-installation.md) | **Next:** [05. Node.js Installation →](./05-nodejs-installation.md)