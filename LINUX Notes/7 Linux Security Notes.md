Great 👍 Security is a **very important topic in Linux & DevOps interviews**, because you’ll deal with access control, SSH, privilege management, and SELinux. Let’s go step by step with **commands, concepts, interview & scenario Q\&A**.

---

# 🔐 Linux Security Notes

---

## 1. File Permissions & ACL

### 🔹 Standard Permissions (rwx)

* **3 types of permissions**:

  * `r` (read), `w` (write), `x` (execute).

* **3 entities**:

  * Owner, Group, Others.

* Example:

  ```bash
  chmod 755 script.sh
  ```

  * Owner → `rwx`
  * Group → `r-x`
  * Others → `r-x`

* **Change ownership**:

  ```bash
  chown user:group file.txt
  ```

---

### 🔹 ACL (Access Control List)

* More granular than chmod (assign different permissions to multiple users/groups).

* Enable ACLs:

  ```bash
  mount -o remount,acl /
  ```

* Add ACL:

  ```bash
  setfacl -m u:john:r file.txt
  ```

  (Give user `john` read permission)

* View ACL:

  ```bash
  getfacl file.txt
  ```

---

## 2. SSH Key-Based Authentication

* **Generate key pair**:

  ```bash
  ssh-keygen -t rsa -b 4096
  ```

  (Generates `~/.ssh/id_rsa` & `~/.ssh/id_rsa.pub`)

* **Copy public key to server**:

  ```bash
  ssh-copy-id user@server
  ```

  → Adds key to `~/.ssh/authorized_keys`.

* **Login without password**:

  ```bash
  ssh user@server
  ```

* Disable password login (more secure):
  Edit `/etc/ssh/sshd_config`:

  ```
  PasswordAuthentication no
  ```

  Then restart SSH:

  ```bash
  systemctl restart sshd
  ```

---

## 3. /etc/passwd & /etc/shadow

* **/etc/passwd**

  * Stores user account details.
  * Format:

    ```
    username:x:UID:GID:comment:home:shell
    ```
  * Password field is `x` → actual password stored in `/etc/shadow`.

* **/etc/shadow**

  * Stores encrypted passwords + aging info.
  * Format:

    ```
    username:hashed_password:last_change:min:max:warn:inactive:expire
    ```
  * Only root can read it.

---

## 4. sudo & Least Privilege Principle

* **sudo** → run commands with elevated privileges.

  ```bash
  sudo systemctl restart nginx
  ```

* Add user to sudoers:

  ```bash
  visudo
  ```

  Example:

  ```
  john ALL=(ALL) NOPASSWD:ALL
  ```

* **Least Privilege Principle** →

  * Give only required permissions, not full root.
  * Example: allow restarting nginx only:

    ```
    john ALL=(ALL) /bin/systemctl restart nginx
    ```

---

## 5. SELinux Basics

* SELinux = Security-Enhanced Linux (mandatory access control).

* Modes:

  * `getenforce` → shows status (`Enforcing`, `Permissive`, `Disabled`)
  * `setenforce 0` → switch to permissive
  * `setenforce 1` → enforcing mode

* Config file: `/etc/selinux/config`

* Example: Allow httpd to access home dirs:

  ```bash
  setsebool -P httpd_enable_homedirs on
  ```

---

# 🎯 Interview Questions

### Q1: Difference between chmod and ACL?

**A:**

* `chmod` → basic rwx permissions (owner, group, others).
* `ACL` → fine-grained, multiple users/groups permissions.

---

### Q2: How does SSH key authentication work?

**A:**

* Client generates key pair.
* Public key stored in server’s `authorized_keys`.
* During login, server sends challenge encrypted with public key → client proves identity with private key.

---

### Q3: Difference between /etc/passwd and /etc/shadow?

**A:**

* `/etc/passwd` → user info, world-readable.
* `/etc/shadow` → encrypted passwords + expiration, root-only.

---

### Q4: What is least privilege principle?

**A:**
Give users only the minimum access they need. Example: Instead of full root, grant ability to restart only specific service.

---

### Q5: What are SELinux modes?

**A:**

* **Enforcing** → actively enforces rules.
* **Permissive** → logs violations, doesn’t enforce.
* **Disabled** → off.

---

# ⚡ Scenario-Based Questions

### Scenario 1: You need to give `john` user access to read `/etc/httpd.conf` but not modify it.

**Answer:**

```bash
setfacl -m u:john:r /etc/httpd.conf
```

---

### Scenario 2: A user is locked out because of password expiry. How to check?

**Answer:**
Check `/etc/shadow` or use:

```bash
chage -l username
```

---

### Scenario 3: An app is failing due to "Permission denied", but chmod looks fine. What could be the issue?

**Answer:** Likely SELinux is blocking. Confirm with:

```bash
getenforce
ausearch -m avc -ts recent
```

---

### Scenario 4: Your organization wants passwordless login for servers. How will you implement?

**Answer:**

* Generate SSH keys.
* Copy public key to server `~/.ssh/authorized_keys`.
* Disable password authentication in `/etc/ssh/sshd_config`.

---

✅ That covers **file permissions, ACL, SSH security, passwd/shadow, sudo, SELinux, with examples, interview Q\&A, and scenarios**.

Would you like me to also make you a **one-page Security Cheatsheet** (commands + purpose) so you can revise quickly before interviews?
