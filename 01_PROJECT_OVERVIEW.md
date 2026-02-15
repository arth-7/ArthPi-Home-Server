# ArthPi Home Server – Project Overview

## 🎯 Purpose of This Project

This project converts a Raspberry Pi 4 into a structured and reliable home server that functions as:

- A NAS (Network Attached Storage)
- A RAID1 mirrored storage system
- A media streaming server (Jellyfin)
- A simple web-based upload system (Filebrowser)
- A Windows-compatible network share (Samba)
- A monitored server with automated disk and RAID alerts

The goal is to build a stable, organized, and professional-grade home server that is easy to manage and reliable long term.

---

## 🖥 Hardware Used

- Raspberry Pi 4B  
- 2 × 750GB Seagate HDDs  
- Blueendless HD05 dual-drive dock  
- MicroSD card (OS only)

---

## 💿 Operating System

- Ubuntu Server 25.10 (64-bit ARM)
- Headless setup (managed via SSH)
- Static IP configured using Netplan

**Hostname:**  
`arthpi`

**Static IP:**  
`10.0.0.50`

---

## 💾 Storage Architecture

### RAID Configuration

- **RAID Level:** RAID1 (Mirroring)
- **RAID Device:** `/dev/md0`
- **Filesystem:** ext4
- **Mount Point:** `/mnt/raid`
- **Status:** clean `[UU]`
- **Persistence:** Configured in `/etc/fstab` using UUID

### What RAID1 Means

RAID1 mirrors both drives:

- Data is written to both disks simultaneously.
- If one disk fails, the other continues running.
- Total usable storage equals the size of one drive.

> Important: RAID provides redundancy, not backup.


## 📂 Folder Structure

All real data lives under:
/mnt/raid

Directory layout:
/mnt/raid
├── media
│ ├── movies
│ └── series
│
└── docker
├── jellyfin
│ ├── config
│ └── cache
└── filebrowser
├── config
└── database

Nothing critical is stored on the SD card.  
The SD card contains only the operating system.

## 🧩 Services Running

### 🎬 Jellyfin
- Media streaming server
- Port: `8096`
- URL: `http://arthpi:8096`
- Container path: `/media`
- Host path: `/mnt/raid/media`

### 📁 Filebrowser
- Web-based file manager
- Port: `8081`
- URL: `http://10.0.0.50:8081`
- Root directory: `/mnt/raid/media`

### 🖥 Samba (SMB)
- Windows network share
- Share name: `Media`
- Path: `/mnt/raid/media`
- Access: `\\arthpi\Media`

### 🐳 Portainer
- Docker management interface
- Port: `9000`

### 🔧 Cockpit
- Web-based server administration panel
- Port: `9090` (LAN access only)

---

## 📊 Monitoring & Automation

The server includes automated monitoring to reduce manual checks.

### SMART Monitoring
- Daily disk health checks
- Email alerts on disk failure
- Scheduled SMART tests

### RAID Monitoring
- Email alerts if RAID becomes degraded

### Daily Health Report

Every day at **7:00 AM**, the system sends an email summary including:

- RAID status
- SMART health summary
- Disk temperatures
- Mount status
- Docker container health

This allows early detection of hardware issues.

---

## ⚠ Current Known Issue

One disk is reporting a SMART failure (`Spin_Retry_Count`).

- RAID is still healthy.
- Redundancy is intact.
- Disk replacement is planned.

---

## ✅ Project Status

The system is currently:

- Stable  
- RAID healthy  
- Automated monitoring active  
- Email alerts working  
- Media streaming functional  
- Network access working  

### Next Planned Improvements

- Replace failing HDD  
- Improve drive cooling  
- Consider larger capacity drives  

---

**End of Document**


