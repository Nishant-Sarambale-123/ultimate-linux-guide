Got it ✅
I’ll prepare **detailed Linux Fundamentals notes** on the mentioned topics and at the end, I’ll add **interview Q\&A + scenario-based questions** so you can use this directly for preparation.

---

# 🐧 Basic Linux Fundamentals – Detailed Notes

---

## 1. Linux Architecture

Linux follows a layered architecture:

1. **Kernel**

   * Core of Linux OS.
   * Manages hardware resources (CPU, memory, devices).
   * Provides system calls to user space.
   * Types: Monolithic kernel (Linux), Microkernel (Minix).

2. **Shell**

   * Command interpreter between user and kernel.
   * Accepts user commands → converts them into system calls.
   * Types: Bash, Zsh, Ksh, Fish.

3. **File System**

   * Hierarchical structure (tree-like).
   * Everything in Linux is a file (devices, processes, directories).
   * Common types: ext4, xfs, btrfs.

4. **User Space**

   * Area where user applications run.
   * Includes libraries, applications, and daemons.

---

## 2. File System Hierarchy (Important Directories)

* `/bin` → Essential user commands (ls, cp, mv, rm, cat, echo).
* `/etc` → Configuration files (passwd, fstab, ssh/sshd\_config).
* `/var` → Variable data (logs, mail, spool, temporary files).
* `/opt` → Optional 3rd party software.
* `/tmp` → Temporary files (cleared after reboot).
* `/home` → User home directories.
* `/usr` → User programs and libraries.

  * `/usr/bin` → Additional user binaries.
  * `/usr/lib` → Shared libraries.

---

## 3. File Permissions

Every file in Linux has **owner, group, others** access.

* **Symbols:** `r (4) = read`, `w (2) = write`, `x (1) = execute`.
* **chmod** → Change permissions.

  * Example: `chmod 755 file` → Owner rwx, group r-x, others r-x.
* **chown** → Change file owner.

  * Example: `chown user:group file`
* **umask** → Default permission mask for new files.

  * Example: `umask 022` → new files = 644, new dirs = 755.

---

## 4. File Operations

* `ls` → List files.
* `cp` → Copy files (`cp file1 file2`).
* `mv` → Move/rename files.
* `rm` → Remove files.
* `cat` → Display file contents.
* `less` → Scroll through file content (q to quit).
* `head -n 10 file` → Show first 10 lines.
* `tail -n 10 file` → Show last 10 lines.
* `tail -f logfile` → Follow log in real-time.

---

## 5. Process Management

* `ps aux` → Show running processes.
* `top` → Interactive process viewer.
* `htop` → Enhanced top with colors.
* `kill PID` → Kill process by PID.
* `kill -9 PID` → Force kill.
* `nice` → Start process with priority (`nice -n 10 command`).
* `renice` → Change priority of running process (`renice -n 5 -p PID`).

---

## 6. User & Group Management

* `useradd username` → Add user.
* `usermod -aG group user` → Add user to group.
* `passwd user` → Set/change password.
* `groups user` → Show groups of a user.
* `sudo` → Run commands as root.

---

Got it 👍 Let’s go through each **Disk Usage & Monitoring** command with **real examples** so it’s crystal clear:

---
7. Disk Usage & Monitoring
8.  df -h → Disk space usage (human-readable).
9.  du -sh /path → Directory size summary. l
10.  sblk → List block devices.
11.   mount /dev/sdb1 /mnt → Mount partition.
12.   umount /mnt → Unmount partition.
13.   /etc/fstab → Defines auto-mount settings. 
### **1. df -h** → Disk space usage (human-readable)

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
tmpfs           798M     0  798M   0% /dev/shm
/dev/sdb1       100G   60G   35G  64% /mnt/data
```

✅ Shows disk partitions, total size, used space, available space, and mount points.

---

### **2. du -sh /path** → Directory size summary

```bash
$ du -sh /var/log
2.3G    /var/log
```

✅ Displays total size of `/var/log` (logs often grow large).
Use `du -sh *` inside a directory to see size of each subfolder.

---

### **3. lsblk** → List block devices

```bash
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk
└─sda1   8:1    0   50G  0 part /
sdb      8:16   0  100G  0 disk
└─sdb1   8:17   0  100G  0 part /mnt/data
```

✅ Shows all disks (`sda`, `sdb`) and their partitions (`sda1`, `sdb1`) with mount points.

---

### **4. mount /dev/sdb1 /mnt** → Mount partition

```bash
$ sudo mount /dev/sdb1 /mnt
$ df -h | grep /mnt
/dev/sdb1       100G   1G   99G   2% /mnt
```

✅ Mounted `/dev/sdb1` partition to `/mnt` directory.

---

### **5. umount /mnt** → Unmount partition

```bash
$ sudo umount /mnt
$ df -h | grep /mnt
# (No output since /mnt is unmounted)
```

✅ `/dev/sdb1` is now unmounted and no longer accessible at `/mnt`.

---

### **6. /etc/fstab** → Defines auto-mount settings

File content example (`/etc/fstab`):

```bash
UUID=1234-5678   /mnt/data   ext4   defaults   0 0
```

* `UUID=1234-5678` → Unique identifier of disk/partition (use `blkid` to find).
* `/mnt/data` → Mount point.
* `ext4` → Filesystem type.
* `defaults` → Default options.
* `0 0` → Dump & fsck options.

Now the partition auto-mounts at boot:

```bash
$ df -h | grep /mnt/data
/dev/sdb1       100G   5G   95G   5% /mnt/data
```

---

# 🎯 Interview Questions & Answers

### Q1: What is the difference between Kernel and Shell?

**A:**

* Kernel → Core part of OS, manages hardware & resources.
* Shell → Interface between user & kernel, executes commands.

---

### Q2: Explain difference between `chmod 777` and `chmod 755`.

**A:**

* `777` → Full permissions (rwx for owner, group, others).
* `755` → Full for owner (rwx), read/execute for group & others.

---

### Q3: What happens when you type a command in Linux?

**A:**

1. Shell takes command.
2. Searches in `$PATH` for binary.
3. Converts to system call.
4. Kernel executes it.

---

### Q4: How to find which process is using a port?

**A:**
`lsof -i :8080` OR `netstat -tulnp | grep 8080`.

---

### Q5: What is the difference between hard link and soft link?

**A:**

* Hard link → Points directly to inode, survives even if original file is deleted.
* Soft link (symlink) → Points to file path, breaks if original file is deleted.

---

### Q6: How to check disk usage of all users?

**A:**
`du -sh /home/*` → Shows per-user usage.

---

# ⚡ Scenario-Based Questions

### Scenario 1: A critical process is consuming 100% CPU. What will you do?

**Answer:**

* Run `top` or `htop` to identify PID.
* If safe, lower priority → `renice +10 -p PID`.
* If harmful, kill → `kill -9 PID`.
* Check logs & root cause.

---

### Scenario 2: A user cannot create files in `/opt/app/`. How do you troubleshoot?

**Answer:**

* Check directory permissions → `ls -ld /opt/app/`.
* If owned by root, assign group → `chown -R user:group /opt/app/`.
* Give proper permissions → `chmod 755 /opt/app/`.

---

### Scenario 3: Root partition is full, but `/var/log` is huge. What will you do?

**Answer:**

* Check space → `du -sh /var/log/*`.
* Compress old logs → `gzip old.log`.
* Clear rotated logs → `logrotate`.
* Mount `/var/log` on separate partition if recurring.

---

### Scenario 4: After reboot, extra disk is not auto-mounted. What’s the fix?

**Answer:**

* Add entry in `/etc/fstab`:

  ```
  /dev/sdb1   /mnt/data   ext4   defaults   0 0
  ```
* Run `mount -a` to apply.

---

✅ That’s a complete **Linux Fundamentals interview prep notes** with theory + commands + interview Q\&A + scenarios.

Do you want me to also create a **one-page cheatsheet (summary with only commands)** for quick revision?
