# Linux User Management 

## 1. What is a User?
- Identity in Linux system
- Each user has UID (User ID)
- Controls access, processes, permissions

---

## 2. Important Files

| File | Purpose |
|------|--------|
| /etc/passwd | user info |
| /etc/shadow | passwords |
| /etc/group | group info |

---

## 3. User Types
- Root (UID 0) → full access
- System users → services
- Normal users → human users

---

## 4. Create User

Basic:
  sudo useradd username

With home:
  sudo useradd -m username

Set password:
  sudo passwd username

---

## 5. Modify User

Change name:
  sudo usermod -l new old

Change shell:
  sudo usermod -s /bin/bash username

Add to group:
  sudo usermod -aG group username

---

## 6. Delete User

Delete user:
  sudo userdel username

Delete with home:
  sudo userdel -r username

---

## 7. Groups

Create group:
  sudo groupadd groupname

Add user:
  sudo usermod -aG group user

Remove user:
  sudo gpasswd -d user group

---

## 8. View Users

Check user:
  id username

List users:
  cat /etc/passwd

---

## 9. Switch User

Switch:
  su username

Root:
  sudo -i

---

## 10. Password Management

Check expiry:
  chage -l username

Set expiry:
  chage -M 90 username

---

## 11. Key Commands

useradd

usermod

userdel

groupadd

passwd

id

groups

---







# Linux Permission Management 

## 1. Permission Levels

| Level | Meaning |
|------|--------|
| User (u) | Owner |
| Group (g) | Group members |
| Others (o) | Everyone else |

---

## 2. Permission Types

| Symbol | Value | Meaning |
|--------|------|--------|
| r | 4 | Read |
| w | 2 | Write |
| x | 1 | Execute |

---

## 3. View Permissions

ls -l

Example:
-rwxr-xr--

---

## 4. File vs Directory

File:
- r → read
- w → modify
- x → execute

Directory:
- r → list files
- w → create/delete
- x → enter

---

## 5. chmod (Change Permissions)

Symbolic:
  chmod u+x file
  chmod g-w file
  chmod a+r file

Numeric:
  chmod 755 file
  chmod 644 file

---

## 6. Numeric Values

| Number | Permission |
|--------|-----------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |

---

## 7. Ownership

Change owner:
  chown user file

Change group:
  chgrp group file

Both:
  chown user:group file

---

## 8. Recursive Changes

chmod -R 755 folder
chown -R user folder

---

## 9. Default Permissions (umask)

Check:
  umask

Formula:
  file → 666 - umask
  dir → 777 - umask

---

## 10. Special Permissions

SUID:
  chmod u+s file

SGID:
  chmod g+s folder

Sticky bit:
  chmod +t folder

---

## 11. Examples

755 → rwxr-xr-x
644 → rw-r--r--
700 → rwx------

---













# Linux Process Management 

## 1. What is a Process?
- A process = program in execution
- Identified by PID (Process ID)
- Contains code, memory, CPU state, open files

---

## 2. Process Creation
- Created using:
  - fork() → creates child process
  - exec() → loads new program
- Example flow:
  bash → fork → exec → program runs

---

## 3. Process Hierarchy
- Every process has a parent
- Root process = systemd (PID 1)
- Structure:
  systemd → services → user processes

---

## 4. Viewing Processes
- ps → snapshot
- ps aux → detailed list
- top → real-time monitoring
- htop → interactive UI

---

## 5. Process States

| State | Meaning |
|------|--------|
| R | Running |
| S | Sleeping (waiting) |
| D | Waiting (I/O) |
| T | Stopped |
| Z | Zombie |

---

## 6. Foreground vs Background
- Foreground → runs in terminal
- Background → runs independently

Run in background:
  command &

Check jobs:
  jobs

Bring back:
  fg / bg

---

## 7. Signals (Process Control)

| Signal | Use |
|-------|-----|
| SIGTERM (15) | Graceful stop |
| SIGKILL (9) | Force kill |
| SIGSTOP | Pause |
| SIGCONT | Resume |

---

## 8. Process Termination

Kill process:
  kill PID

Force kill:
  kill -9 PID

Kill by name:
  pkill name
  killall name

---

## 9. Process Priority
- Range: -20 (high) to 19 (low)
- Default: 0

Run with priority:
  nice -n 10 command

Change priority:
  renice -n 5 -p PID

---

## 10. Zombie Process
- Child finished but parent didn’t clean it
- State = Z
- Fix: kill parent process

---

## 11. Key Commands Summary

ps aux
top
kill PID
kill -9 PID
jobs
bg / fg

---







# Linux Logging - Quick Notes (KN Method)

## 1. What is Logging?

Logs are system records that store: - What happened - When it happened -
Which service caused it - Success or failure

Used for: - Debugging - Monitoring - Security auditing

------------------------------------------------------------------------

## 2. Logging Flow (How it Works)

Event → Logging Service → Stored in Files → Admin Reads

Components: - Applications / Kernel generate logs - systemd-journald /
rsyslog collect logs - Stored in `/var/log`

------------------------------------------------------------------------

## 3. Important Log Directory

`/var/log` = Main log storage

Common files: - syslog → General system logs - auth.log → Login &
security logs - kern.log → Kernel logs - boot.log → Boot process logs -
dmesg → Hardware logs

------------------------------------------------------------------------

## 4. Application Logs

Stored inside: - `/var/log/nginx/` - `/var/log/apache2/` -
`/var/log/mysql/`

------------------------------------------------------------------------

## 5. journalctl (Modern Logs)

Commands: - View all logs: `journalctl` - Last logs:
`journalctl -n 50` - Service logs: `journalctl -u nginx` - Current boot:
`journalctl -b` - Live logs: `journalctl -f`

------------------------------------------------------------------------

## 6. Log Reading Commands

-   View file: `less /var/log/syslog`
-   Show last logs: `tail /var/log/syslog`
-   Live logs: `tail -f /var/log/syslog`
-   Search logs: `grep error /var/log/syslog`

------------------------------------------------------------------------

## 7. Log Format

Example: Mar 13 11:15:01 server CRON\[1234\]: Job started

Parts: - Timestamp - Hostname - Service - Process ID - Message

------------------------------------------------------------------------

## 8. Troubleshooting Steps

1.  Identify issue (service failure)
2.  Check service status: `systemctl status <service>`
3.  Check logs: `journalctl -u <service>`
4.  Search errors: `grep error /var/log/syslog`
5.  Check app logs

------------------------------------------------------------------------

## 9. Common Issues & Logs

-   Login failure → `/var/log/auth.log`
-   Server crash → `dmesg` or kern.log
-   Web errors → `/var/log/nginx/error.log`

------------------------------------------------------------------------

## 10. Log Rotation

Tool: `logrotate`

Config: `/etc/logrotate.conf`

Purpose: - Prevent logs from filling disk - Compress old logs

------------------------------------------------------------------------







# Cron Jobs in Linux – Notes

## What is Cron?

Cron is a Linux scheduler used to automate repetitive tasks.
It runs commands or scripts at specific times automatically.
It is managed by a background service called crond.

---

## Key Components

crond → Background service that executes scheduled jobs
crontab → File that stores scheduled tasks
cron job → Command or script scheduled to run

---

## Cron Workflow

User adds job → stored in crontab → crond checks every minute → runs task if time matches

---

## Crontab Syntax

* * * * * command
          │ │ │ │ │
          │ │ │ │ └── Day of Week (0–7, Sunday = 0/7)
          │ │ │ └──── Month (1–12)
          │ │ └────── Day of Month (1–31)
          │ └──────── Hour (0–23)
          └────────── Minute (0–59)

---

## Time Field Meaning

Minute: 0–59
Hour: 0–23
Day of Month: 1–31
Month: 1–12
Day of Week: 0–7

---

## Special Symbols

* → Every value
  ,  → Multiple values

- → Range
  /  → Interval

Examples:

* * * * * → every minute
          0 9,17 * * * → 9 AM and 5 PM
          0 9-17 * * * → every hour from 9 to 5
          */5 * * * * → every 5 minutes

---

## Common Cron Examples

Every minute → * * * * *
Every 5 minutes → */5 * * * *
Every hour → 0 * * * *
Daily at midnight → 0 0 * * *
Sunday at 3 AM → 0 3 * * 0
Monday to Friday at 9 AM → 0 9 * * 1-5

---

## Cron Commands

View cron jobs:
crontab -l

Edit cron jobs:
crontab -e

Delete all cron jobs:
crontab -r

View another user's cron:
crontab -u username -l

---

## Cron File Locations

/var/spool/cron/ → User cron jobs
/etc/crontab → System cron file
/etc/cron.hourly → Hourly jobs
/etc/cron.daily → Daily jobs
/etc/cron.weekly → Weekly jobs
/etc/cron.monthly → Monthly jobs

---
## Logs and Monitoring

Check cron logs:
grep CRON /var/log/syslog

---










