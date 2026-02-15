# Monitoring & Alerting Architecture

This document explains how automated health monitoring works.

---

## Design Goal

Detect hardware or service issues early and notify the administrator automatically.

No manual checking required.

---

## 🛠 Components Used

| Tool      | Purpose |
|-----------|----------|
| cron      | Scheduling automation |
| smartctl  | Disk SMART health checks |
| mdadm     | RAID status monitoring |
| msmtp     | Secure email sending |
| mail      | Simple CLI email wrapper |

---

## Monitoring Flow

1. cron triggers script at 7:00 AM
2. Script gathers:
   - RAID status (`/proc/mdstat`)
   - Detailed RAID info (`mdadm --detail`)
   - SMART disk health (`smartctl`)
   - Disk temperature
   - Docker container status
3. Script formats output
4. Output piped to `mail`
5. `mail` uses `msmtp`
6. Email delivered via Gmail SMTP (TLS)

---

## Script Location

```
/usr/local/bin/arthpi-health.sh
```

Permissions:

```
chmod +x /usr/local/bin/arthpi-health.sh
```

---

## Cron Configuration

Check cron jobs:

```
sudo crontab -l
```

Scheduled job:

```
0 7 * * * /usr/local/bin/arthpi-health.sh | mail -s "ArthPi Daily Health Report" arthrraval@gmail.com
```

---

## Email Configuration

Using `msmtp` with:

- TLS enabled
- Gmail SMTP (smtp.gmail.com:587)
- App Password authentication
- Config files permissioned 600

Security:

- No plain-text root passwords exposed
- App password used instead of account password

---

## Example Output

Daily report includes:

- RAID health `[UU]`
- SMART pass/fail
- Disk temperatures
- Mount confirmation
- Docker container health

---

## Alert Conditions

Immediate action required if:

- RAID shows `[U_]`
- SMART reports `FAILED`
- Spin_Retry_Count failing
- Temperature consistently > 55°C
- Docker containers not running

---

## Manual Test Procedure

To test the system manually:

```
/usr/local/bin/arthpi-health.sh | mail -s "Manual Test" arthrraval@gmail.com
```

---

## Future Improvements

- Add temperature threshold alerts
- Add Telegram/Discord notifications
- Add log file archiving
- Add weekly long SMART tests
- Add uptime monitoring

---

End of Monitoring Documentation
