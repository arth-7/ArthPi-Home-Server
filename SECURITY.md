# Security Policy (ArthPi Home Server)

This document describes the security posture of the ArthPi home server and the expected operational rules.

> Scope: local home network only (LAN). No public internet exposure by default.

---

## Security Goals

- Keep services accessible **only on the home network**
- Minimize open ports and limit access where possible
- Ensure storage integrity (RAID + monitoring)
- Use least privilege and avoid unsafe defaults
- Maintain a clear audit trail for changes

---

## Network Exposure

### Intended Access Model

- ✅ LAN-only access (home Wi-Fi / Ethernet)
- ❌ No port forwarding / no WAN access
- ❌ No public DNS exposure

---

## Firewall (UFW)

Firewall is enabled with default deny inbound.

### Allowed Services (LAN)

- SSH: `22/tcp`
- Jellyfin: `8096/tcp`
- Filebrowser: `8081/tcp`
- Portainer: `9000/tcp`
- Cockpit: `9090/tcp` (**restricted to LAN subnet**)
- Samba: `137-139/tcp+udp`, `445/tcp+udp`

> Recommendation: do not expose these ports outside the LAN.

---

## Accounts & Authentication

### SSH
- Use a non-root user (`arth`)
- Root login over SSH should remain disabled
- Prefer key-based SSH in future

### Samba
- Share is restricted to authenticated user(s)
- No guest access to shared media folder

### Email Alerts
- Uses an app password stored in msmtp config
- Keep msmtp configs permissioned `600`

Files:
- `/home/arth/.msmtprc` (mode `600`)
- `/root/.msmtprc` (mode `600`)

---

## Docker Security

- Containers use bind mounts to `/mnt/raid` (persistent storage)
- Only required ports are published
- Avoid running containers in privileged mode
- Keep Docker images updated regularly

---

## Operational Rules

- Never unplug drives while running
- Always shutdown cleanly (`sudo shutdown -h now`)
- Avoid unnecessary reboots (reduces disk stress)
- Replace failing disks promptly
- RAID is not a backup — keep an external copy of important data

---

## Reporting a Security Issue

This is a personal system. If a critical issue is found:

- Disable the exposed service immediately (UFW + stop service)
- Review logs:
  - `journalctl -xe`
  - `/var/log/samba/`
  - `~/.msmtp.log`
- Rotate credentials (Samba password / app password)

---

## Quick Security Checks (Admin)

Run these commands:

```bash
sudo ufw status verbose
ss -tulpn
sudo systemctl --type=service --state=running | head -n 80
docker ps
```

---

End of Security Policy
