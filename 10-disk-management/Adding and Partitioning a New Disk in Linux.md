Perfect 👌 — let’s go step by step with **detailed notes on how to add a new disk, partition it, and make it usable in Linux** (common in DevOps & Sysadmin interviews and real-world troubleshooting).

---

# 🖴 Adding and Partitioning a New Disk in Linux

---

## **1. Attach the New Disk**

* If on **cloud VM (AWS, Azure, GCP, VMware, VirtualBox, etc.)**, attach an extra volume/disk first.
* Example: `/dev/sdb` appears after adding a 20 GB new disk.

---

## **2. Verify the New Disk**

Check if the OS recognizes the new disk.

```bash
lsblk
```

**Output (example):**

```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   48G  0 part /
├─sda2   8:2    0    2G  0 part [SWAP]
sdb      8:16   0   20G  0 disk   # New unpartitioned disk
```

👉 `sdb` = new disk, not yet partitioned.

---

## **3. Create a Partition**

Use `fdisk` (MBR) or `parted` (GPT) — `fdisk` is simpler.

```bash
sudo fdisk /dev/sdb
```

**Steps inside `fdisk`:**

* `n` → new partition
* `p` → primary
* `1` → partition number (sdb1)
* Press Enter for **default first sector**
* Press Enter for **default last sector** (uses full disk)
* `w` → write changes and exit

Now `/dev/sdb1` is created.

---

## **4. Format the Partition with Filesystem**

Example: format as **ext4** (common Linux filesystem).

```bash
sudo mkfs.ext4 /dev/sdb1
```

👉 This creates an ext4 filesystem on the partition.

---

## **5. Create Mount Point & Mount Disk**

Choose a directory to mount (e.g., `/mnt/data`):

```bash
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
```

**Verify mount:**

```bash
df -h | grep sdb1
```

---

## **6. Persistent Mount (Auto-mount at Reboot)**

Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

Add entry:

```
/dev/sdb1   /mnt/data   ext4   defaults   0 0
```

👉 This ensures `/mnt/data` mounts automatically at reboot.

**Test without reboot:**

```bash
sudo mount -a
```

(if no errors → fstab entry is valid).

---

## **7. Set Permissions (Optional)**

By default, only root can write. Give permissions:

```bash
sudo chown -R $USER:$USER /mnt/data
sudo chmod 755 /mnt/data
```

---

## **8. Verify Final Setup**

```bash
lsblk
df -h
mount | grep sdb1
```

Example output:

```
/dev/sdb1   20G   24M   19.9G   1% /mnt/data
```

---

# ✅ Quick Summary

1. Attach new disk → `/dev/sdb`
2. Check → `lsblk`
3. Partition → `fdisk /dev/sdb`
4. Format → `mkfs.ext4 /dev/sdb1`
5. Mount → `mount /dev/sdb1 /mnt/data`
6. Permanent → Add to `/etc/fstab`
7. Permissions → `chown` & `chmod`

---

⚡ Tip: If disk > 2 TB → prefer **GPT partition table (`parted` or `gdisk`)**, since MBR has size limits.

---

Do you want me to also include **steps with screenshots-like command outputs** (as if done on a real Linux system), so you can visualize better for interviews and practice?
