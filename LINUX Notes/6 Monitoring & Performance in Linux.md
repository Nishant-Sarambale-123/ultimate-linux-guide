Perfect 👍 Monitoring & performance is **core for Linux + DevOps interviews** because you’ll often troubleshoot resource issues, bottlenecks, and logs. Let’s break it down in detail with **commands, usage, interview + scenario Q\&A**.

---

# 📊 Monitoring & Performance in Linux

---

## 1. CPU & Memory Monitoring

### 🔹 `top`

* Real-time view of CPU, memory, process usage.
* Key fields:

  * `%CPU` → CPU usage
  * `%MEM` → memory usage
  * `load average` → avg system load (1, 5, 15 min)
* Commands inside `top`:

  * `k` → kill a process
  * `r` → renice a process
  * `M` → sort by memory
  * `P` → sort by CPU

---

### 🔹 `htop` (advanced top)

* Interactive, color UI version of `top`.
* Easier to navigate with arrow keys.
* Shows process tree & CPU cores separately.

---

### 🔹 `vmstat`

* Shows overall **CPU, memory, IO, swap statistics**.
* Example:

  ```bash
  vmstat 2 5
  ```

  (updates every 2s, 5 times)

Key columns:

* `r` → processes waiting for CPU
* `b` → processes waiting for I/O
* `si/so` → swap in/out
* `wa` → time CPU waits for I/O

---

### 🔹 `free`

* Shows memory usage (RAM & swap).
* Example:

  ```bash
  free -m
  ```

  * `used` = total used memory
  * `free` = unused memory
  * `available` = usable memory (better indicator)

---

## 2. Disk I/O Monitoring

### 🔹 `iostat` (from `sysstat` package)

* CPU and I/O stats per disk/device.
* Example:

  ```bash
  iostat -x 2 5
  ```

  * `r/s`, `w/s` → reads/writes per second
  * `%util` → % time device was busy

---

### 🔹 `iotop`

* Like `top` but for disk I/O per process.
* Shows which process is consuming I/O.

---

### 🔹 `df`

* Shows **disk space usage** per filesystem.
* Example:

  ```bash
  df -h
  ```

  → human readable (GB, MB).

---

### 🔹 `du`

* Shows disk space used by files & directories.
* Example:

  ```bash
  du -sh /var/log/*
  ```

  (size of each log file in `/var/log/`)

---

## 3. Logs Analysis

### 🔹 `tail`

* View last lines of a file.
* Follow logs in real time:

  ```bash
  tail -f /var/log/syslog
  ```

---

### 🔹 `grep`

* Search inside logs.
* Example:

  ```bash
  grep "ERROR" /var/log/app.log
  ```
* With `-i` → case insensitive
* With `-r` → recursive

---

### 🔹 `awk`

* Text processing tool, works with columns.
* Example: Extract IP from logs:

  ```bash
  awk '{print $1}' access.log
  ```

---

### 🔹 `sed`

* Stream editor (replace, filter text).
* Example: Replace "ERROR" with "ISSUE":

  ```bash
  sed 's/ERROR/ISSUE/g' app.log
  ```

---

### 🔹 `cut`

* Extract specific columns/fields.
* Example: Get 2nd column (space-delimited):

  ```bash
  cut -d' ' -f2 access.log
  ```

---

### 🔹 `sort` & `uniq`

* `sort` → sort log entries.
* `uniq -c` → count unique occurrences.
* Example: Top IP addresses:

  ```bash
  awk '{print $1}' access.log | sort | uniq -c | sort -nr | head
  ```

---

# 🎯 Interview Questions

### Q1: How do you check which process is consuming the most CPU?

**A:** Use `top` or `htop`. Sort by `%CPU`.

---

### Q2: Difference between `df` and `du`?

**A:**

* `df` → reports free space on filesystem level.
* `du` → reports actual file/directory size.

---

### Q3: How do you check disk I/O bottleneck?

**A:** Use `iostat -x`. If `%util` is \~100%, disk is overloaded.

---

### Q4: How do you analyze logs for specific errors in last 100 lines?

**A:**

```bash
tail -n 100 app.log | grep "ERROR"
```

---

### Q5: Server is slow, how do you troubleshoot?

**A:**

1. Check CPU load → `top`, `uptime`.
2. Check memory → `free -m`, `vmstat`.
3. Check disk usage → `df -h`.
4. Check I/O → `iostat`, `iotop`.
5. Check logs → `tail -f /var/log/syslog`.

---

# ⚡ Scenario-Based Questions

### Scenario 1: A Java app keeps crashing due to "Out of Memory". How do you confirm?

**Answer:**

```bash
free -m
dmesg | grep -i oom
```

Check swap, memory usage.

---

### Scenario 2: Disk is 100% full, but `df -h` shows free space. Why?

**Answer:** Deleted files still held by running processes. Check with:

```bash
lsof | grep deleted
```

---

### Scenario 3: Website is slow; CPU usage is low, but I/O is high. What’s wrong?

**Answer:** Likely disk bottleneck. Confirm with `iostat -x` and `iotop`.

---

### Scenario 4: Find top 5 most common error messages from logs.

**Answer:**

```bash
grep "ERROR" app.log | sort | uniq -c | sort -nr | head -5
```

---

✅ That covers **CPU, memory, disk I/O, and logs analysis with commands, interview Q\&A, and scenarios.**

Would you like me to also create a **ready-made troubleshooting checklist** (step-by-step order: CPU → Memory → Disk → Logs) so you can use it in interviews?
