# ArthPi Home Server – Admin Guide

This document covers maintenance, monitoring, recovery procedures, and safe operations.

This guide assumes SSH access to the server.

---

# Accessing the Server

Connect via SSH:

```
ssh arth@10.0.0.50
```

---

# Safe Reboot Procedure

Never pull the power cable.

To reboot safely:

```
sudo reboot
```

To shut down safely:

```
sudo shutdown -h now
```

Wait until all drive activity stops before disconnecting power.

---

# RAID Health Monitoring

## Check RAID Status

```
cat /proc/mdstat
```

Healthy output should show:

```
[UU]
```

If it shows:

```
[U_]
```

One disk has failed.

---

## Detailed RAID Info

```
sudo mdadm --detail /dev/md0
```

Check for:

- State: clean
- Active Devices: 2
- Failed Devices: 0

---

# SMART Disk Health Checks

## Quick Health Check

```
sudo smartctl -H /dev/sda
sudo smartctl -H /dev/sdb
```

Look for:

```
SMART overall-health self-assessment test result: PASSED
```

If it says FAILED → replace disk.

---

## Check Important SMART Attributes

```
sudo smartctl -A /dev/sda | egrep 'Reallocated|Pending|Spin|Temperature'
sudo smartctl -A /dev/sdb | egrep 'Reallocated|Pending|Spin|Temperature'
```

Warning signs:

- Spin_Retry_Count failing
- Reallocated sectors increasing
- Pending sectors > 0
- Temperature consistently above 50°C

---

# Replacing a Failed RAID Disk

When a disk fails:

## Step 1 – Mark Disk as Failed

```
sudo mdadm --manage /dev/md0 --fail /dev/sdb1
sudo mdadm --manage /dev/md0 --remove /dev/sdb1
```

## Step 2 – Insert New Disk

Partition it:

```
sudo parted /dev/sdb -- mklabel gpt
sudo parted /dev/sdb -- mkpart primary 0% 100%
sudo parted /dev/sdb -- set 1 raid on
```

## Step 3 – Add to RAID

```
sudo mdadm --manage /dev/md0 --add /dev/sdb1
```

## Step 4 – Monitor Rebuild

```
watch cat /proc/mdstat
```

Wait until `[UU]` appears.

---

# Docker Maintenance

## Check Running Containers

```
docker ps
```

## Restart a Container

```
docker restart jellyfin
docker restart filebrowser
```

## Update Containers

```
docker compose pull
docker compose up -d
```

---

# Daily Health Script

Location:

```
/usr/local/bin/arthpi-health.sh
```

Runs automatically via cron at 7:00 AM.

To test manually:

```
/usr/local/bin/arthpi-health.sh | mail -s "Manual Health Test" your-email@gmail.com
```

---

# Firewall (UFW)

Check firewall status:

```
sudo ufw status verbose
```

Allowed ports:

- 22 (SSH)
- 8096 (Jellyfin)
- 8081 (Filebrowser)
- 9000 (Portainer)
- 9090 (Cockpit – LAN only)
- 137-139, 445 (Samba)

---

# Emergency Checklist

If server is unreachable:

1. Check power.
2. Check network connection.
3. Try SSH.
4. Check RAID status.
5. Check disk SMART health.
6. Reboot safely if necessary.

---

# Important Operational Rules

- Never unplug drives while running.
- Never power off without shutdown.
- Keep backups of important data.
- Monitor disk temperature.
- Replace failing drives promptly.

---

End of Admin Guide
