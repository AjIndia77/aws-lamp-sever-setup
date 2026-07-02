# 05. Node.js Installation

**Node.js LTS** *(Long Term Support)* — used here for frontend build tools and running Node-based APIs alongside the PHP stack.

## 1. Add the NodeSource Repository

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
```

This adds the official NodeSource repository so `apt` can install the latest LTS release directly (Ubuntu's default repos often carry outdated Node versions).

## 2. Install Node.js

```bash
sudo apt install nodejs -y
```

This also installs `npm` (Node's package manager) automatically.

## 3. Verify the Installation

```bash
node -v
npm -v
```

## 4. Quick Test (Optional)

Confirm Node.js runs correctly:

```bash
node -e "console.log('Node.js is working')"
```

---
**Previous:** [← 04. MySQL Installation](./04-mysql-installation.md) | **Next:** [06. phpMyAdmin Installation →](./06-phpmyadmin-installation.md)