Great 👍 You’re covering all the **important Linux tools** that are frequently used in **DevOps, SRE, and Linux admin interviews**. Let’s go point by point with explanations, examples, and then wrap up with **interview + scenario Q\&A**.

---

# 🛠️ Important Linux Tools Notes

---

## 1. Text Processing Tools

### 🔹 grep → search for patterns in files

* Search word in a file:

  ```bash
  grep "error" /var/log/syslog
  ```
* Case-insensitive search:

  ```bash
  grep -i "failed" auth.log
  ```
* Recursive search in directory:

  ```bash
  grep -r "TODO" /project
  ```

---

### 🔹 sed → stream editor (modify text on the fly)

* Replace text:

  ```bash
  sed 's/error/issue/g' file.txt
  ```
* Delete lines containing a pattern:

  ```bash
  sed '/DEBUG/d' app.log
  ```
* Replace only on line 3:

  ```bash
  sed '3s/old/new/' file.txt
  ```

---

### 🔹 awk → powerful text processing & field manipulation

* Print 2nd column:

  ```bash
  awk '{print $2}' file.txt
  ```
* Print lines where 3rd column > 100:

  ```bash
  awk '$3 > 100 {print $0}' data.txt
  ```
* Sum values in 2nd column:

  ```bash
  awk '{sum += $2} END {print sum}' data.txt
  ```

---

### 🔹 cut → extract fields from text

* Get first 10 characters:

  ```bash
  cut -c1-10 file.txt
  ```
* Extract 2nd column (comma-delimited):

  ```bash
  cut -d',' -f2 data.csv
  ```

---

### 🔹 sort → sort lines in a file

* Sort alphabetically:

  ```bash
  sort names.txt
  ```
* Sort numerically (descending):

  ```bash
  sort -nr numbers.txt
  ```

---

### 🔹 uniq → remove duplicates

* Remove duplicates (must use `sort` before):

  ```bash
  sort names.txt | uniq
  ```
* Count occurrences:

  ```bash
  sort names.txt | uniq -c
  ```

---

## 2. Find Files

### 🔹 find → search files by name, size, type, etc.

* Find by name:

  ```bash
  find / -name "app.log"
  ```
* Find files >100MB:

  ```bash
  find /var/log -size +100M
  ```
* Execute action (delete logs older than 7 days):

  ```bash
  find /var/log -name "*.log" -mtime +7 -exec rm {} \;
  ```

---

### 🔹 locate → fast file search (uses index database)

* Update DB:

  ```bash
  updatedb
  ```
* Search file:

  ```bash
  locate nginx.conf
  ```

---

### 🔹 which → locate executable in PATH

```bash
which python
```

### 🔹 whereis → locate binary, source, and man pages

```bash
whereis nginx
```

---

## 3. Editors

### 🔹 vi / vim → powerful modal editor

* Open file: `vi file.txt`
* Insert mode: `i`
* Save & exit: `:wq`
* Exit without saving: `:q!`
* Search: `/word`

### 🔹 nano → simple editor

* Open: `nano file.txt`
* Save: `Ctrl+O`
* Exit: `Ctrl+X`

---

## 4. Process Inspection

### 🔹 lsof → list open files

* Show files opened by a process:

  ```bash
  lsof -p <pid>
  ```
* Show processes using a port:

  ```bash
  lsof -i :8080
  ```

### 🔹 strace → trace system calls made by a process

* Run a program and trace:

  ```bash
  strace ls
  ```
* Attach to running process:

  ```bash
  strace -p <pid>
  ```
* Debug network/system issues, permission denied errors, etc.

---

# 🎯 Interview Questions

### Q1: Difference between `grep`, `sed`, and `awk`?

**A:**

* `grep` → search/filter lines.
* `sed` → stream editor, modify text.
* `awk` → field/column-based processing, advanced text handling.

---

### Q2: When would you use `find` vs `locate`?

**A:**

* `find` → real-time search, slower, but accurate.
* `locate` → uses prebuilt database, faster but may be outdated.

---

### Q3: How do you check which process is using port 80?

**A:**

```bash
lsof -i :80
# or
netstat -tulnp | grep :80
```

---

### Q4: Difference between `which` and `whereis`?

**A:**

* `which` → shows binary path from `$PATH`.
* `whereis` → shows binary, source, and man page locations.

---

### Q5: How to debug why a program fails with "Permission denied"?

**A:**
Use `strace` to trace system calls:

```bash
strace -e open,access ./myapp
```

It will show which file is failing.

---

# ⚡ Scenario-Based Questions

### Scenario 1: Your server’s `/var/log` is filling up. How do you find the largest files?

**Answer:**

```bash
find /var/log -type f -exec du -sh {} + | sort -hr | head -10
```

---

### Scenario 2: You need to extract the 2nd column from a CSV and count unique values.

**Answer:**

```bash
cut -d',' -f2 file.csv | sort | uniq -c
```

---

### Scenario 3: Application fails to start. You suspect missing library. What do you do?

**Answer:**
Run:

```bash
strace ./app 2>&1 | grep "ENOENT"
```

This shows missing files.

---

### Scenario 4: How to check which config file `nginx` is using?

**Answer:**

```bash
strace -e open nginx -t 2>&1 | grep conf
```

---

✅ These tools (`grep, sed, awk, find, lsof, strace…`) are **daily drivers for DevOps engineers**.

Do you want me to also make a **cheatsheet table with command + example + use case** for quick interview revision?
