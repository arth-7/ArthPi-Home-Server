ArthPi Home Server – Project Overview
1. Purpose of This Project

This project converts a Raspberry Pi 4 into a home server that functions as:

A NAS (Network Attached Storage)

A RAID1 mirrored storage system

A media streaming server (Jellyfin)

A simple web-based file upload system (Filebrowser)

A Samba network share for drag-and-drop access

A monitored server with automated disk and RAID health alerts

The goal is to build a stable, organized, and professional home server that is easy to manage and reliable.

2. Hardware Used

Raspberry Pi 4B

2 × 750GB Seagate HDDs

Blueendless HD05 dual drive dock

MicroSD card (OS only)

3. Operating System

Ubuntu Server 25.10 (64-bit ARM)

Headless setup (managed via SSH)

Static IP configured using Netplan

Hostname:
arthpi

Static IP:
10.0.0.50

4. Storage Architecture
RAID Configuration

RAID Level: RAID1 (Mirroring)

RAID Device: /dev/md0

Filesystem: ext4

Mount Point: /mnt/raid

Status: clean [UU]

Persistence: Configured in /etc/fstab using UUID

What RAID1 Means

RAID1 mirrors both drives:

Data is written to both disks simultaneously.

If one disk fails, the other keeps the system running.

Total usable space equals one disk.

Important:
RAID is redundancy, NOT backup.

5. Folder Structure

All real data is stored under:

/mnt/raid


Structure:

/mnt/raid
  ├── media
  │     ├── movies
  │     └── series
  │
  └── docker
        ├── jellyfin
        │     ├── config
        │     └── cache
        └── filebrowser
              ├── config
              └── database


Nothing important is stored on the SD card.

6. Services Running
Jellyfin

Media streaming server

Port: 8096

URL: http://arthpi:8096

Media path inside container: /media

Maps to host path: /mnt/raid/media

Filebrowser

Simple web file manager

Port: 8081

URL: http://10.0.0.50:8081

Root directory: /mnt/raid/media

Samba (SMB)

Windows network share

Share name: Media

Path: /mnt/raid/media

Access: \\arthpi\Media

Portainer

Docker management interface

Port: 9000

Cockpit

Web server administration panel

Port: 9090 (LAN only)

7. Monitoring & Automation

The server includes automated health monitoring:

SMART Monitoring

Daily disk health checks

Email alerts on disk failure

Weekly SMART tests

RAID Monitoring

Email alerts if RAID becomes degraded

Daily Health Report

Every day at 7:00 AM:

RAID status

SMART health summary

Disk temperature

Mount status

Docker container health

Email summary sent automatically

8. Current Known Issue

One disk is reporting SMART failure (Spin_Retry_Count).

RAID is still healthy.

Disk replacement is planned.

Data redundancy still intact.

9. Project Status

System is currently:

Stable

Fully automated

Email alerts working

RAID healthy

Media streaming functional

Network access working

Monitoring active

Next planned improvement:

Replace failing HDD

Improve cooling

Possibly upgrade drives
