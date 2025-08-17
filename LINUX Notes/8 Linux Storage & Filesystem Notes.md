Perfect 👌 Storage & Filesystem is another **core Linux area** often tested in **DevOps & SRE interviews**. Let’s break it down step by step with commands, examples, and interview + scenario Q\&A.

---

# 💾 Linux Storage & Filesystem Notes

---

## 1. Partitioning

### 🔹 fdisk (MBR/GPT partitioning tool)

* List disks:

  ```bash
  fdisk -l
  ```
* Create new partition:

  ```bash
  fdisk /dev/sdb
  ```

  Inside fdisk prompt:

  * `n` → new partition
  * `p` → primary partition
  * `w` → write changes

---

### 🔹 parted (advanced, works with >2TB disks, GPT)

* Start parted:

  ```bash
  parted /dev/sdb
  ```
* Example create partition:

  ```bash
  mklabel gpt
  mkpart primary ext4 1MiB 5GiB
  quit
  ```

---

## 2. Filesystem Creation

### 🔹 mkfs (make filesystem)

* Format partition with ext4:

  ```bash
  mkfs.ext4 /dev/sdb1
  ```
* Format with XFS:

  ```bash
  mkfs.xfs /dev/sdb1
  ```

### 🔹 tune2fs (modify/ext filesystem parameters)

* Check filesystem info:

  ```bash
  tune2fs -l /dev/sdb1
  ```
* Change reserved blocks percentage:

  ```bash
  tune2fs -m 1 /dev/sdb1
  ```

---

## 3. Mounting & Unmounting

### 🔹 Temporary Mount (lost after reboot)

```bash
mount /dev/sdb1 /mnt
umount /mnt
```

### 🔹 Permanent Mount → `/etc/fstab`

Example entry:

```
/dev/sdb1   /data   ext4   defaults   0 2
```

* `0` → dump backup (usually 0)
* `2` → fsck order (1=root fs, others=2)

Reload fstab without reboot:

```bash
mount -a
```

---

## 4. Swap Management

### 🔹 Check existing swap

```bash
swapon --show
free -h
```

### 🔹 Create swap file

```bash
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### 🔹 Permanent entry in `/etc/fstab`

```
/swapfile none swap sw 0 0
```

### 🔹 Disable swap (temporary)

```bash
swapoff -a
```

---

# 🎯 Interview Questions

### Q1: Difference between fdisk and parted?

**A:**

* `fdisk` → works with MBR and GPT but limited with very large disks (>2TB).
* `parted` → supports GPT, large disks, scripting, advanced partitioning.

---

### Q2: What is the use of `/etc/fstab`?

**A:**
It contains persistent filesystem mount configurations. On boot, system reads it and mounts partitions automatically.

---

### Q3: What happens if `/etc/fstab` is misconfigured?

**A:**
System may fail to boot properly. Common recovery: boot into rescue mode, fix `/etc/fstab`, remount root.

---

### Q4: Difference between mkfs and tune2fs?

**A:**

* `mkfs` → formats and creates a new filesystem.
* `tune2fs` → modifies parameters of an existing ext filesystem (reserved blocks, labels, etc.).

---

### Q5: How does Linux use swap?

**A:**
Swap is disk space used as an extension of RAM. It helps when RAM is full, but is slower. Can be partition-based or file-based.

---

# ⚡ Scenario-Based Questions

### Scenario 1: You added a new 100GB disk `/dev/sdc` and want to mount it at `/data`. How?

**Answer:**

```bash
fdisk /dev/sdc          # create partition
mkfs.ext4 /dev/sdc1     # format
mkdir /data
mount /dev/sdc1 /data
echo "/dev/sdc1 /data ext4 defaults 0 2" >> /etc/fstab
mount -a
```

---

### Scenario 2: Your application reports "read-only filesystem". What could be the issue?

**Answer:**

* Disk errors → system remounted FS read-only.
* Check with:

  ```bash
  dmesg | grep -i error
  mount | grep ro
  ```

---

### Scenario 3: Root filesystem is 100% full. How do you fix?

**Answer:**

* Check usage:

  ```bash
  du -sh /*
  ```
* Clear logs (`/var/log`).
* Move data to another partition.
* Use `tune2fs -m 1 /dev/sda1` to reduce reserved space (for root).

---

### Scenario 4: Swap is full and system is slow. What do you do?

**Answer:**

* Check processes using memory:

  ```bash
  top
  ```
* Increase swap:

  ```bash
  fallocate -l 2G /swap2
  mkswap /swap2
  swapon /swap2
  ```
* Or add more RAM.

---

✅ That covers **Partitioning, Filesystem, Mounting, Swap**, with **commands, interview & scenario Q\&A**.

Do you want me to also include **LVM (Logical Volume Management)** here? It often comes up in DevOps/SRE interviews along with storage.
