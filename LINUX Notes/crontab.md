## **Crontab Notes in Linux (Complete & Interview-Ready)**

![Image](https://tecadmin.net/wp-content/uploads/2013/03/crontab-2.png?utm_source=chatgpt.com)

![Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1684341382013/4407b2ac-a6f0-4d17-bbd6-4624467f1fc0.png?utm_source=chatgpt.com)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1200/0%2AQKaDRRZW-8ViuDig.png?utm_source=chatgpt.com)

---

## **What is Crontab?**

`crontab` is a Linux **job scheduler** used to run commands or scripts **automatically at fixed time intervals** (minutes, hours, days, weeks, months).

👉 Used mainly for **automation**, **backups**, **log rotation**, **monitoring**, and **maintenance tasks**.

---

## **Cron Components**

* **cron** → background daemon (service)
* **crontab** → configuration file for scheduled jobs
* **cron job** → scheduled task

---

## **Crontab Syntax**

```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └── Day of week (0–7) (Sunday=0 or 7)
│ │ │ └──── Month (1–12)
│ │ └────── Day of month (1–31)
│ └──────── Hour (0–23)
└────────── Minute (0–59)
```

---

## **Basic Crontab Commands**

| Command                 | Description            |
| ----------------------- | ---------------------- |
| `crontab -e`            | Edit crontab           |
| `crontab -l`            | List cron jobs         |
| `crontab -r`            | Remove all cron jobs   |
| `crontab -u user -e`    | Edit other user’s cron |
| `systemctl status cron` | Check cron service     |

---

## **Common Cron Examples**

### Run script every day at 2 AM

```bash
0 2 * * * /home/user/backup.sh
```

### Run every 5 minutes

```bash
*/5 * * * * /script.sh
```

### Run every Sunday at 6 PM

```bash
0 18 * * 0 /cleanup.sh
```

### Run at reboot

```bash
@reboot /start_app.sh
```

---

## **Special Cron Keywords**

| Keyword    | Meaning            |
| ---------- | ------------------ |
| `@reboot`  | Run at system boot |
| `@hourly`  | Every hour         |
| `@daily`   | Once per day       |
| `@weekly`  | Once per week      |
| `@monthly` | Once per month     |

---

## **System Crontabs**

| File/Dir             | Purpose          |
| -------------------- | ---------------- |
| `/etc/crontab`       | System-wide cron |
| `/etc/cron.hourly/`  | Hourly jobs      |
| `/etc/cron.daily/`   | Daily jobs       |
| `/etc/cron.weekly/`  | Weekly jobs      |
| `/etc/cron.monthly/` | Monthly jobs     |

---

## **Crontab Best Practices**

✅ Use **absolute paths**

```bash
/usr/bin/python /scripts/app.py
```

✅ Redirect output to logs

```bash
* * * * * /job.sh >> /var/log/job.log 2>&1
```

✅ Set environment variables

```bash
PATH=/usr/local/bin:/usr/bin
```

✅ Test scripts manually before scheduling

---

## **Cron Troubleshooting**

| Issue                                 | Fix                         |
| ------------------------------------- | --------------------------- |
| Job not running                       | Check `cron` service        |
| Script works manually but not in cron | Use full paths              |
| No logs                               | Redirect stdout/stderr      |
| Permission denied                     | Check executable permission |

Check logs:

```bash
/var/log/cron
/var/log/syslog
```

---

## **Crontab vs Anacron**

| Feature                 | Crontab | Anacron   |
| ----------------------- | ------- | --------- |
| Requires system running | Yes     | No        |
| Missed jobs             | Lost    | Run later |
| Use case                | Servers | Laptops   |

---

## **Interview One-Liner**

> Crontab is a Linux scheduler that automates recurring tasks using time-based rules, commonly used for backups, monitoring, and maintenance.

---

If you want, I can also provide:

* **Crontab interview Q&A**
* **Real production examples**
* **Cron vs systemd timers**
* **DevOps use cases (AWS, Kubernetes cron jobs)**
