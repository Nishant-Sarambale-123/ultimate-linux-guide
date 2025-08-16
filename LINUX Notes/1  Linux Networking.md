Perfect 👍 Let’s build **detailed Networking notes for Linux/DevOps interview prep**.
I’ll explain each command with examples, concepts, and then add **interview + scenario-based Q\&A** at the end.

---

# 🌐 Linux Networking – Detailed Notes

---

## 1. Check IP & Routing

### 🔹 Commands:

* `ip a` → Show all network interfaces & IP addresses.
  Example:

  ```bash
  ip a show eth0
  ```

  Shows IP, MAC, state (UP/DOWN).

* `ip route` → Show kernel routing table.
  Example:

  ```
  default via 192.168.1.1 dev eth0
  ```

  → default gateway is `192.168.1.1`.

* `ifconfig` → Legacy tool (replaced by `ip`). Shows interface details.

* `netstat -rn` → Show routing table.

* `ss -tulnp` → Show listening ports (faster replacement for netstat).

---

## 2. DNS Lookup & Connectivity

### 🔹 Commands:

* `ping <hostname/IP>` → Test reachability.
  Example: `ping google.com`

* `traceroute <host>` → Show path (hops) packets take.
  Example: `traceroute 8.8.8.8`

* `nslookup <domain>` → Basic DNS lookup.
  Example: `nslookup openai.com`

* `dig <domain>` → Detailed DNS query (better than nslookup).
  Example: `dig A google.com`

* `curl <url>` → Fetch data from web (REST API testing).
  Example: `curl -I https://example.com`

* `wget <url>` → Download file from web.
  Example: `wget https://example.com/file.zip`

---

## 3. Firewall & Ports

### 🔹 Commands:

* **iptables** → Kernel firewall tool.

  * Example:

    ```bash
    iptables -L -n -v
    iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    iptables -A INPUT -j DROP
    ```

* **firewalld** (service wrapper for iptables).

  * Example:

    ```bash
    firewall-cmd --list-all
    firewall-cmd --add-port=8080/tcp --permanent
    firewall-cmd --reload
    ```

* **ufw (Uncomplicated Firewall)** → Ubuntu/Debian.

  * Example:

    ```bash
    ufw allow 22/tcp
    ufw deny 80/tcp
    ufw status
    ```

* **Port Testing Tools**:

  * `telnet <IP> <PORT>` → Check connectivity (deprecated in some distros).
  * `nc -zv <IP> <PORT>` → Test open ports.
    Example: `nc -zv 192.168.1.10 443`

---

## 4. SSH Basics

### 🔹 Commands:

* **ssh** → Connect to remote server.
  Example:

  ```bash
  ssh user@192.168.1.10
  ```

* **scp** → Securely copy files.
  Example:

  ```bash
  scp file.txt user@192.168.1.10:/home/user/
  ```

* **sftp** → Secure FTP (interactive).
  Example:

  ```bash
  sftp user@192.168.1.10
  ```

* **SSH Key Management**

  * Generate key: `ssh-keygen -t rsa -b 4096`
  * Copy key: `ssh-copy-id user@server`
  * Keys stored in `~/.ssh/id_rsa` (private), `~/.ssh/id_rsa.pub` (public).
  * Config file: `~/.ssh/config` for aliases.

---

## 5. Network Troubleshooting

### 🔹 Commands:

* **tcpdump** → Capture packets.
  Example:

  ```bash
  tcpdump -i eth0 port 80
  ```

  → Capture HTTP traffic on eth0.

* **nmap** → Network scanner (port scanning, service detection).
  Examples:

  * `nmap 192.168.1.10` → Scan host.
  * `nmap -p 1-1000 192.168.1.10` → Scan first 1000 ports.
  * `nmap -sV 192.168.1.10` → Detect services & versions.

---

# 🎯 Interview Questions & Answers

### Q1: Difference between `netstat` and `ss`?

**A:**

* `netstat` → Old, slower, less maintained.
* `ss` → Modern, faster, provides more socket info.

---

### Q2: Difference between `curl` and `wget`?

**A:**

* `curl` → Used for APIs, sending HTTP requests, testing endpoints.
* `wget` → Used for downloading files.

---

### Q3: How does SSH authentication work?

**A:**

* Client sends public key to server.
* Server checks against `~/.ssh/authorized_keys`.
* If matched, allows access without password.

---

### Q4: What is the difference between iptables and firewalld?

**A:**

* iptables → Direct firewall rules, manual.
* firewalld → Wrapper for iptables, zone-based, dynamic updates.

---

### Q5: How do you troubleshoot if you can’t reach a server?

**Steps:**

1. `ping <IP>` → Check basic connectivity.
2. `traceroute <IP>` → Check route/hops.
3. `ssh user@host` → Test SSH connection.
4. `nc -zv host 22` → Check if port open.
5. Check firewall (`iptables -L`).
6. Capture traffic (`tcpdump`).

---

# ⚡ Scenario-Based Questions

### Scenario 1: You can ping a server but cannot SSH. What do you do?

**Answer:**

* Check if port 22 is open → `nc -zv server 22`.
* Verify firewall rules (`ufw status` / `iptables -L`).
* Ensure `sshd` service is running → `systemctl status sshd`.

---

### Scenario 2: A website is slow. How do you troubleshoot?

**Answer:**

* `ping` → Check latency.
* `traceroute` → Identify network hops issues.
* `curl -I` → Check response headers (server-side delays).
* `tcpdump` → Capture traffic.
* `nmap` → Scan for open/extra services.

---

### Scenario 3: Port 8080 is open locally but not accessible externally. What will you do?

**Answer:**

* Verify service is listening → `ss -tulnp | grep 8080`.
* Check firewall → `iptables -L` / `ufw status`.
* Check external routing (security group / NAT / gateway).

---

### Scenario 4: You need to allow only SSH from a specific IP. How?

**Answer:**
Using `iptables`:

```bash
iptables -A INPUT -p tcp -s 192.168.1.100 --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j DROP
```

---

✅ These notes cover **IP, routing, DNS, firewall, SSH, troubleshooting** with theory + commands + interview + scenarios.

Would you like me to also make a **network troubleshooting flowchart (step-by-step diagram)** for quick recall during interviews?
