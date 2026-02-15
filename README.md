![Ubuntu](https://img.shields.io/badge/Ubuntu-25.10-E95420?logo=ubuntu&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED?logo=docker&logoColor=white)
![RAID](https://img.shields.io/badge/RAID-1-blue)
![ARM64](https://img.shields.io/badge/Architecture-ARM64-green)
![Status](https://img.shields.io/badge/Status-Active-success)

# ArthPi Home Server

A Raspberry Pi 4–based home server designed for reliable storage, media streaming, and automated health monitoring.

This project turns a Raspberry Pi into:

-  RAID1 mirrored storage (data redundancy)
-  Jellyfin media server
-  Samba network storage (Windows compatible)
-  Web-based file uploads (Filebrowser)
-  Docker-managed services
-  Automated disk & RAID health monitoring with email alerts

---

# Hardware

- Raspberry Pi 4B
- 2 × 750GB Seagate HDDs
- Blueendless HD05 dual-drive dock
- MicroSD card (OS only)

---

# System Architecture

## Storage Layer

- RAID Level: **RAID1 (Mirroring)**
- Device: `/dev/md0`
- Filesystem: `ext4`
- Mount point: `/mnt/raid`
- Auto-mount configured via UUID in `/etc/fstab`

RAID1 mirrors both disks.  
If one fails, the system continues running.

> RAID is redundancy, not backup.

---

## Directory Structure

```
/mnt/raid
├── media
│   ├── movies
│   └── series
│
└── docker
    ├── jellyfin
    │   ├── config
    │   └── cache
    └── filebrowser
        ├── config
        └── database
```

All important data lives on RAID — not on the SD card.

---

# Services

| Service      | Purpose                  | Port |
|-------------|--------------------------|------|
| Jellyfin    | Media streaming          | 8096 |
| Filebrowser | Web-based file manager   | 8081 |
| Samba       | Windows file sharing     | 445  |
| Portainer   | Docker management        | 9000 |
| Cockpit     | System admin panel       | 9090 |

---

# Access

- Jellyfin:  
  `http://10.0.0.50:8096`

- Filebrowser:  
  `http://10.0.0.50:8081`

- Samba (Windows):  
  `\\10.0.0.50\Media`

- SSH:  
  `ssh arth@10.0.0.50`

---

# Monitoring & Automation

The system includes automated monitoring:

- Daily RAID health check
- SMART disk health monitoring
- Disk temperature reporting
- Docker container status check
- Email alerts on failure
- Daily summary email at 7:00 AM

Health script location:

```
/usr/local/bin/arthpi-health.sh
```

---

# Documentation

Detailed guides:

- `01_PROJECT_OVERVIEW.md`
- `02_USER_GUIDE.md`
- `03_ADMIN_GUIDE.md`

---

# Current Status

- RAID healthy
- Monitoring active
- Email alerts working
- Media streaming functional
- One disk scheduled for replacement

---
##  Architecture Diagram

```mermaid
flowchart TD

    %% Clients
    Phone[ Phone]
    Laptop[ Windows Laptop]
    TV[ Smart TV]

    %% Network
    Router[ Home Router / WiFi]

    %% Server
    Pi[🖥 Raspberry Pi 4<br>Ubuntu Server]

    %% Services
    Jellyfin[ Jellyfin :8096]
    Filebrowser[ Filebrowser :8081]
    Samba[ Samba Share]
    Docker[ Docker Engine]

    %% Storage
    RAID[ RAID1 /dev/md0]
    DiskA[HDD A]
    DiskB[HDD B]

    %% Connections
    Phone --> Router
    Laptop --> Router
    TV --> Router

    Router --> Pi

    Pi --> Docker
    Docker --> Jellyfin
    Docker --> Filebrowser
    Pi --> Samba

    Jellyfin --> RAID
    Filebrowser --> RAID
    Samba --> RAID

    RAID --> DiskA
    RAID --> DiskB
```

## 🔁 Data Flow Diagram (Upload + Streaming)

```mermaid
flowchart LR

  %% Clients
  Phone["Phone\nUploads"]
  Laptop["Windows Laptop\nDrag and Drop"]
  TV["Smart TV\nPlayback"]

  %% Network
  Router["Home Router / WiFi"]

  %% Server + Services
  Pi["Raspberry Pi 4\nUbuntu Server"]
  Filebrowser["Filebrowser\n8081"]
  Samba["Samba SMB\nMedia Share"]
  Jellyfin["Jellyfin\n8096"]

  %% Storage
  RAID["RAID1 md0\n/mnt/raid"]
  Movies["media/movies"]
  Series["media/series"]

  %% Network connectivity
  Phone -->|WiFi| Router
  Laptop -->|WiFi or Ethernet| Router
  TV -->|WiFi| Router
  Router -->|LAN| Pi

  %% Upload paths
  Phone -->|HTTP upload| Filebrowser
  Laptop -->|SMB copy| Samba

  %% Service hosting
  Pi -->|hosts| Filebrowser
  Pi -->|hosts| Samba
  Pi -->|docker runs| Jellyfin

  %% Storage writes
  Filebrowser -->|writes| RAID
  Samba -->|writes| RAID

  %% Media organization
  RAID -->|stores| Movies
  RAID -->|stores| Series

  %% Streaming
  Jellyfin -->|reads| RAID
  TV -->|streams| Jellyfin
  Phone -->|optional stream| Jellyfin
  Laptop -->|optional stream| Jellyfin
```


# Future Improvements

- Replace aging HDDs
- Improve cooling
- Upgrade to larger capacity drives
- Implement automated off-site backups

---

# License

Personal project. For educational and home use.

---

**ArthPi Home Server**  
Built for reliability, simplicity, and control.
