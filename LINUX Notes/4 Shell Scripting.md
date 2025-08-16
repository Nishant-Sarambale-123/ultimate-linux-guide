Perfect 🚀 Shell scripting is indeed **core for DevOps interviews**. Let’s make a **detailed structured notes** covering all the topics you mentioned, with examples, best practices, and then add **interview + scenario Q\&A** at the end.

---

# 🐚 Shell Scripting – Detailed Notes

---

## 1. Variables & Environment

### 🔹 Defining Variables

```bash
NAME="Nishant"
AGE=25
echo "My name is $NAME and age is $AGE"
```

* No spaces around `=`
* Variables are case-sensitive

### 🔹 Environment Variables

* View: `env` or `printenv`
* Export:

  ```bash
  export PATH=$PATH:/opt/bin
  ```
* Persistent variables → Add in `~/.bashrc`, `~/.bash_profile` or `/etc/profile`

---

## 2. Conditional Statements

### 🔹 if-else

```bash
if [ $AGE -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi
```

### 🔹 elif

```bash
if [ $AGE -lt 13 ]; then
    echo "Child"
elif [ $AGE -lt 20 ]; then
    echo "Teen"
else
    echo "Adult"
fi
```

### 🔹 case statement

```bash
case $1 in
  start) echo "Service Starting...";;
  stop)  echo "Service Stopping...";;
  restart) echo "Restarting...";;
  *) echo "Usage: $0 {start|stop|restart}";;
esac
```

---

## 3. Loops

### 🔹 for loop

```bash
for i in 1 2 3 4 5
do
  echo "Number: $i"
done
```

With range:

```bash
for i in {1..5}; do echo $i; done
```

### 🔹 while loop

```bash
count=1
while [ $count -le 5 ]
do
  echo "Count: $count"
  count=$((count+1))
done
```

### 🔹 until loop

```bash
num=1
until [ $num -gt 5 ]
do
  echo "Num: $num"
  num=$((num+1))
done
```

---

## 4. Functions in Shell Scripts

```bash
greet() {
   echo "Hello, $1"
}

greet Nishant
```

* `$1, $2, …` → Positional parameters
* `return <code>` → Return exit code (not values)

---

## 5. Command Substitution

Two ways:

```bash
DATE=$(date)
echo "Today is $DATE"

HOST=`hostname`
echo "Host: $HOST"
```

✅ Prefer `$(command)` (modern, supports nesting).

---

## 6. Exit Codes & Error Handling

* `$?` → Stores exit status of last command.

  * `0` → success
  * Non-zero → failure

Example:

```bash
ls /tmp
if [ $? -eq 0 ]; then
   echo "Command successful"
else
   echo "Command failed"
fi
```

### Better error handling with `set`

```bash
set -e   # exit on error
set -u   # error on undefined vars
set -o pipefail   # fail if any command in pipeline fails
```

---

## 7. Scheduling Jobs

### 🔹 Cron Jobs (recurring tasks)

* Edit cron table:

  ```bash
  crontab -e
  ```
* Example: Run script daily at 2 AM

  ```
  0 2 * * * /path/to/script.sh
  ```
* View jobs: `crontab -l`

Cron Syntax:

```
min hour day month weekday command
```

### 🔹 `at` (one-time tasks)

```bash
echo "/path/to/script.sh" | at 23:00
```

→ Runs script at 11 PM today.

---

# 🎯 Interview Questions & Answers

### Q1: What is the difference between `sh` and `bash`?

**A:**

* `sh` → Bourne shell (old, minimal features).
* `bash` → Bourne Again Shell (improved, supports arrays, functions, scripting features).

---

### Q2: How do you debug a shell script?

**A:**
Run script with:

```bash
bash -x script.sh
```

or add `set -x` inside script to trace execution.

---

### Q3: How do you handle errors in a script?

**A:**

* Check exit code with `$?`.
* Use `set -e` to exit on first error.
* Log errors to a file:

  ```bash
  command || echo "command failed" >> error.log
  ```

---

### Q4: How do you pass arguments to a shell script?

**A:**

* `$0` = script name
* `$1, $2, …` = arguments
* `$#` = number of args
* `$@` = all arguments

Example:

```bash
./backup.sh /etc /var/log
```

Inside script:

```bash
SRC=$1
DEST=$2
```

---

### Q5: What is the difference between `cron` and `at`?

**A:**

* `cron` → Schedules recurring jobs.
* `at` → Schedules one-time jobs.

---

# ⚡ Scenario-Based Questions

### Scenario 1: You wrote a script, but it works manually and fails in cron. Why?

**Answer:**

* Cron runs with minimal environment → missing PATH or variables.
* Fix: use absolute paths (`/usr/bin/python` not just `python`) or export PATH in script.

---

### Scenario 2: You want to write a script that checks if a service is running, and restart if not.

**Answer:**

```bash
#!/bin/bash
SERVICE=nginx

if ! systemctl is-active --quiet $SERVICE; then
   echo "$SERVICE is down, restarting..."
   systemctl restart $SERVICE
fi
```

---

### Scenario 3: Script needs to process 100 files, but if one fails, it should continue with others.

**Answer:**

```bash
for file in *.txt
do
   process_file "$file" || echo "Failed: $file"
done
```

(`||` ensures failure doesn’t break loop)

---

### Scenario 4: Write a cron job to run backup.sh every Monday at 3 AM.

**Answer:**

```
0 3 * * 1 /opt/scripts/backup.sh
```

---

✅ This is a **full Shell Scripting DevOps guide** with variables, conditionals, loops, functions, substitution, exit codes, cron/at, interview Q\&A, and scenarios.

Do you want me to also create a **ready-to-use sample DevOps shell script (like backup + monitoring + automation)** for practice?
