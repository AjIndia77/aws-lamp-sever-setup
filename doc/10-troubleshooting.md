# 10. Troubleshooting

Real issues encountered during this project, along with root causes and fixes. Documented for future reference and to help others hitting the same problems.

---

## 1. PHP Version Mismatch on Ubuntu 26.04

**Symptom:** Project required PHP 7.4 and PHP 8.1, but the instance's default repository only offered PHP 8.5.

**Cause:** The EC2 instance was initially provisioned on Ubuntu 26.04 LTS, which ships PHP 8.5 by default — incompatible with the project's version requirements.

**Diagnosis:**
```bash
apt policy php
lsb_release -a
```

**Fix:** Re-provisioned the EC2 instance using Ubuntu 24.04 LTS instead, which supports adding the Ondřej Surý PPA for multiple PHP versions. See [03. PHP Installation](./03-php-installation.md).

**Lesson:** Always verify OS/package compatibility with target software versions *before* building on top of an instance — catching this early avoids re-doing later setup steps.

---

## 2. phpMyAdmin Requires a Newer PHP Version Than Available

**Symptom:** `sudo apt install phpmyadmin` pulled a package requiring PHP 8.2+, but the project used PHP 8.1.

**Cause:** Ubuntu 24.04's repository version of phpMyAdmin (5.2.1) has a packaging dependency on PHP 8.2+.

**Diagnosis:**
```bash
apt policy phpmyadmin
apt-cache madison phpmyadmin
apt search phpmyadmin
```

**Fix:** Installed phpMyAdmin 5.2.3 manually from the official phpMyAdmin website, which officially supports PHP 7.2+. See [06. phpMyAdmin Installation](./06-phpmyadmin-installation.md).

**Lesson:** A package being in the default OS repository doesn't mean it's the right version for your stack — always cross-check dependency requirements against installed versions.

---

## 3. phpMyAdmin Login Fails with Root User

**Symptom:** After installing phpMyAdmin, logging in with MySQL's `root` user fails.

**Cause:** On Ubuntu, MySQL's `root` user authenticates via the `auth_socket` plugin by default — it validates the OS-level Linux user, not a password. phpMyAdmin logs in over HTTP using a username/password, which `auth_socket` doesn't support.

**Diagnosis:**
```sql
SELECT user, host, plugin FROM mysql.user;
```

**Fix:** Created a separate MySQL user with password-based authentication (`mysql_native_password`) specifically for phpMyAdmin access, instead of modifying root's auth method.

**Lesson:** Don't change a secure default (root's auth_socket) just to make a tool work — create a scoped, purpose-specific user instead.

---

## 4. Apache Not Serving the Expected PHP Version

**Symptom:** Unsure which PHP version (7.4 or 8.1) Apache was actively using after installing both.

**Fix:** Created a temporary `phpinfo()` test file to confirm the active version:
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Checked `http://YOUR_PUBLIC_IP/info.php`, then deleted the file immediately after confirming.

**Lesson:** When multiple versions of a runtime are installed side by side, always verify which one is actually active in the web-facing environment — don't assume based on `update-alternatives` alone. Also: never leave a `phpinfo()` file publicly accessible, since it exposes server configuration details.

---

## 5. [Add your own]

**Symptom:**

**Cause:**

**Fix:**

**Lesson:**

---

## General Debugging Commands Used Throughout This Project

| Purpose | Command |
|---|---|
| Check service status | `sudo systemctl status <service>` |
| Check listening ports | `sudo ss -tlnp` |
| Check package version/availability | `apt policy <package>` |
| Search available packages | `apt search <package>` |
| Check installed packages matching a pattern | `dpkg -l \| grep <name>` |
| Tail service logs | `sudo journalctl -u <service> -f` |
| Check Apache error log | `sudo tail -f /var/log/apache2/error.log` |

---
**Previous:** [← 09. SSL Installation](./09-ssl-installation.md)