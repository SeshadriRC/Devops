**Question :** We have a on-premises linux server which
needs some monitoring enabled. Being a DevOps
engineer only you have access to this server. How would
you setup a basic monitoring script on this on-premises
server ?

Here’s the answer **in layman terms + interview perspective**, using **Python + psutil** for monitoring on an on-prem Linux server.

---

# **Interview Perspective Answer (with psutil)**

If I want to set up basic monitoring on an on-premises Linux server and I have access only to that server, I can create a **Python monitoring script using the `psutil` module`**.

`psutil` is a powerful Python library that allows me to collect system metrics like CPU, memory, disk, process info, etc.

---

## **Steps I would follow**

---

## **1. Install psutil**

```bash
pip install psutil
```

---

## **2. Create a monitoring script (monitor.py)**

```python
import psutil
import datetime

def log(message):
    with open("/var/log/basic_monitor.log", "a") as f:
        f.write(f"{datetime.datetime.now()} - {message}\n")

# CPU usage
cpu_usage = psutil.cpu_percent(interval=1)

# Memory usage
memory_usage = psutil.virtual_memory().percent

# Disk usage
disk_usage = psutil.disk_usage('/').percent

log(f"CPU: {cpu_usage}% | Memory: {memory_usage}% | Disk: {disk_usage}%")

# Alerts
if cpu_usage > 85:
    log(f"ALERT: High CPU usage: {cpu_usage}%")

if memory_usage > 90:
    log(f"ALERT: High Memory usage: {memory_usage}%")

if disk_usage > 80:
    log(f"ALERT: High Disk usage: {disk_usage}%")
```

This script does three things:

* Checks CPU, RAM, and Disk percentage
* Logs the output to a file
* Logs alerts if thresholds are crossed

---

## **3. Run the script automatically using cron**

```bash
crontab -e
*/5 * * * * /usr/bin/python3 /opt/monitor.py
```

This means the script runs every 5 minutes.

---

# **Layman Explanation**

Think of psutil as a “thermometer” for your server.

The Python script:

* Checks how hot the CPU is
* Checks how much memory is used
* Checks how full the disk is
* Writes everything into a log file
* If something looks bad, it writes an ALERT message

Then cron automatically runs the script like a reminder every few minutes.

This gives basic monitoring without installing big monitoring tools.

---

## If you want, I can also add:

✔ Email alert
✔ Slack / Teams alert
✔ Service monitoring (using psutil.process_iter)
✔ Log rotation setup

Just tell me!


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c97f7bd-5f9e-41a2-8a57-841125b96bf1" />

