## ✅ Scenario 1: Disk Usage Monitoring & Alert

**📌 Problem**  
Prevent servers from running out of disk space.

**🧠 Real Use**  
- Cron jobs  
- Production monitoring  
- Common interview scenario  

**📜 Script**
```bash
#!/bin/bash

THRESHOLD=80
LOG_FILE="/var/log/disk_monitor.log"

df -h | grep -E '^/dev/' | while read fs size used avail percent mount; do
    usage=${percent%\%}

    if [ "$usage" -ge "$THRESHOLD" ]; then
        echo "$(date) WARNING: $fs mounted on $mount is ${usage}% full" >> "$LOG_FILE"
    fi
done
```
**🎯 Interview Explanation**  
This script monitors disk usage and logs warnings when usage exceeds the defined threshold.

## ✅ Scenario 2: Service Monitoring & Auto-Restart (Nginx)

**📌 Problem**  
Ensure critical services are always running.

**📜 Script**
```bash
#!/bin/bash

SERVICE="nginx"
LOG_FILE="/var/log/service_monitor.log"

if ! systemctl is-active --quiet "$SERVICE"; then
    echo "$(date) $SERVICE is down. Restarting..." >> "$LOG_FILE"
    systemctl restart "$SERVICE"
fi
```
**🎯 Interview Line**  
Used for self-healing services in production.

## ✅ Scenario 3: Backup Automation with Error Handling

**📌 Problem**  
Daily backups with logging and failure detection.

**📜 Script**
```bash
#!/bin/bash
set -euo pipefail

SOURCE="/var/www/html"
BACKUP_DIR="/backup"
LOG_FILE="/var/log/backup.log"

mkdir -p "$BACKUP_DIR"

tar -czf "$BACKUP_DIR/backup_$(date +%F).tar.gz" "$SOURCE" >> "$LOG_FILE" 2>&1

echo "$(date) Backup successful" >> "$LOG_FILE"
```
**🎯 Interview Highlight**  
Uses strict mode and logging to ensure reliability.

## ✅ Scenario 4: Log Cleanup (Retention Policy)

**📌 Problem**  
Prevent logs from consuming disk space.

**📜 Script**
```bash
#!/bin/bash

LOG_PATH="/var/log/myapp"
DAYS=7

find "$LOG_PATH" -type f -mtime +$DAYS -exec rm -f {} \;
```
**🎯 Interview Line**  
Implements log retention policy.

## ✅ Scenario 5: User Creation Automation

**📌 Problem**  
Automate user onboarding.

**📜 Script**
```bash
#!/bin/bash

USER=$1

if id "$USER" &>/dev/null; then
    echo "User already exists"
else
    useradd "$USER"
    echo "$USER created successfully"
fi
```
**🎯 Interview Note**  
Common in system administration automation.

## ✅ Scenario 6: CPU & Memory Monitoring

**📌 Problem**  
Detect high resource usage.

**📜 Script**
```bash
#!/bin/bash

CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print 100 - $8}')
MEM=$(free | awk '/Mem/ {printf("%.2f"), $3/$2*100}')

echo "CPU Usage: $CPU%"
echo "Memory Usage: $MEM%"
```
**🎯 Interview Line**  
Used for lightweight monitoring scripts.

## ✅ Scenario 7: Application Deployment Script

**📌 Problem**  
Automate deployment steps.

**📜 Script**
```bash
#!/bin/bash
set -e

APP_DIR="/opt/app"
GIT_REPO="https://github.com/example/app.git"

if [ ! -d "$APP_DIR" ]; then
    git clone "$GIT_REPO" "$APP_DIR"
else
    cd "$APP_DIR" && git pull
fi

systemctl restart app
```
**🎯 Interview Talking Point**  
Automates deployment with rollback potential.

## ✅ Scenario 8: Error Handling + Logging Template (BEST PRACTICE)

**📜 Production-Ready Template**
```bash
#!/bin/bash
set -euo pipefail

LOG_FILE="/var/log/script.log"

trap 'echo "$(date) ERROR on line $LINENO" >> "$LOG_FILE"' ERR

echo "$(date) Script started" >> "$LOG_FILE"

# your commands here

echo "$(date) Script completed" >> "$LOG_FILE"
```
**🎯 Interview GOLD**  
Demonstrates professional scripting standards.

## 🚀 Common Interview Follow-ups

| Question | How to Answer |
|----------|---------------|
| How to schedule this? | Using cron |
| How to monitor failures? | Logging & exit codes |
| How to improve? | Alerts, email, Slack |
| Why shell over Python? | Lightweight, OS-level |
