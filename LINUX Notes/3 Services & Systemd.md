Great 👍 Let’s prepare **detailed notes on Services & systemd** (service management, logs, runlevels/targets) and also include **apt (package management link)** with **OS flavour–wise differences (Ubuntu/Debian vs RHEL/CentOS)**.

---

# ⚙️ Services & Systemd – Detailed Notes

---

## 1. Service Management with `systemctl`

`systemctl` is the command-line tool to interact with **systemd** (the init system used by most modern Linux distros).

### 🔹 Basic Commands

* **Start a service**

  ```bash
  sudo systemctl start nginx
  ```
* **Stop a service**

  ```bash
  sudo systemctl stop nginx
  ```
* **Restart a service**

  ```bash
  sudo systemctl restart nginx
  ```
* **Check status of a service**

  ```bash
  systemctl status nginx
  ```
* **Enable service at boot**

  ```bash
  sudo systemctl enable nginx
  ```
* **Disable service at boot**

  ```bash
  sudo systemctl disable nginx
  ```
* **Check if enabled**

  ```bash
  systemctl is-enabled nginx
  ```

---

## 2. Logs

### 🔹 Using `journalctl` (systemd log viewer)

* Show logs for a service:

  ```bash
  journalctl -u nginx
  ```
* Show logs since boot:

  ```bash
  journalctl -b
  ```
* Follow logs (like `tail -f`):

  ```bash
  journalctl -u nginx -f
  ```
* Show logs in last 1 hour:

  ```bash
  journalctl --since "1 hour ago"
  ```

### 🔹 Log Files (non-systemd distros or additional logs)

* General logs: `/var/log/syslog` (Debian/Ubuntu)
* System messages: `/var/log/messages` (RHEL/CentOS)
* Service-specific logs (if configured): `/var/log/nginx/error.log`

---

## 3. Runlevels & Targets

### 🔹 Runlevels (SysVinit – older systems)

* **0** → Halt (shutdown)
* **1** → Single user (rescue mode)
* **3** → Multi-user (no GUI)
* **5** → Multi-user with GUI
* **6** → Reboot

### 🔹 Systemd Targets (replacement for runlevels)

* `graphical.target` → GUI (like runlevel 5)
* `multi-user.target` → CLI (like runlevel 3)
* `rescue.target` → Rescue mode (like runlevel 1)
* `emergency.target` → Emergency shell

### 🔹 Commands

* Check current default target:

  ```bash
  systemctl get-default
  ```
* Set default target:

  ```bash
  sudo systemctl set-default multi-user.target
  ```
* Switch to another target immediately:

  ```bash
  sudo systemctl isolate graphical.target
  ```

---

## 4. Package Management (Link with Services)

Different OS families use different package managers, but services are still managed with `systemctl`.

### 🔹 Debian/Ubuntu (APT + Systemd)

* Update repo: `sudo apt update`
* Install service: `sudo apt install nginx`
* Start service: `sudo systemctl start nginx`

### 🔹 RHEL/CentOS (YUM/DNF + Systemd)

* Update repo: `sudo yum update` OR `sudo dnf upgrade`
* Install service: `sudo yum install httpd`
* Start service: `sudo systemctl start httpd`

---

## 5. OS Flavour–wise Differences

| Feature              | Debian/Ubuntu                 | RHEL/CentOS                     |
| -------------------- | ----------------------------- | ------------------------------- |
| Package Manager      | `apt`, `dpkg`                 | `yum`, `dnf`, `rpm`             |
| Default Service Tool | `systemctl`                   | `systemctl`                     |
| Logs                 | `/var/log/syslog`, journalctl | `/var/log/messages`, journalctl |
| Example Service      | `nginx` (installed via apt)   | `httpd` (installed via yum/dnf) |
| Enable Service       | `systemctl enable nginx`      | `systemctl enable httpd`        |
| Restart Service      | `systemctl restart nginx`     | `systemctl restart httpd`       |

---

# 🎯 Interview Questions & Answers

### Q1: What is the difference between `service` and `systemctl`?

**A:**

* `service` → Legacy command (SysVinit).
* `systemctl` → Modern systemd tool (supports units, targets, dependencies).

---

### Q2: How do you check if a service is enabled at boot?

**A:**

```bash
systemctl is-enabled service_name
```

---

### Q3: What is the difference between runlevels and systemd targets?

**A:**

* Runlevels → Numeric (0–6), used in SysVinit.
* Targets → Named (graphical.target, multi-user.target), used in systemd.

---

### Q4: How do you check logs for a failed service startup?

**A:**

```bash
journalctl -u service_name -xe
```

---

### Q5: A service is installed but not starting. What do you check?

**A:**

1. `systemctl status service`
2. Logs → `journalctl -u service`
3. Config errors in `/etc/` files
4. Port conflicts with `ss -tulnp`

---

# ⚡ Scenario-Based Questions

### Scenario 1: After reboot, your web server (nginx/httpd) is not running. How do you fix it?

**Answer:**

* Enable at boot → `sudo systemctl enable nginx`
* Start manually → `sudo systemctl start nginx`
* Check logs → `journalctl -u nginx -xe`

---

### Scenario 2: You want a server to boot into CLI mode (no GUI).

**Answer:**

```bash
sudo systemctl set-default multi-user.target
sudo systemctl isolate multi-user.target
```

---

### Scenario 3: A service is crashing repeatedly. What steps will you take?

**Answer:**

1. Check service logs → `journalctl -u service -xe`
2. Verify configs → syntax check (`nginx -t`, `apachectl configtest`).
3. Restart with debug → `systemctl restart service`
4. Check dependencies or missing packages (apt/yum).

---

✅ That’s a **complete guide on Services & systemd** with commands, logs, runlevels/targets, apt/yum differences, interview Q\&A, and scenarios.

Would you like me to also make a **troubleshooting checklist (step-by-step flow)** for when a service fails to start?
