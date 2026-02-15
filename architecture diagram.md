## 🧱 Layered Architecture (High-Level)

```mermaid
flowchart TB

  %% ===== Client Layer =====
  subgraph L1[Client Devices]
    Phone[📱 Phone<br>(File Upload)]
    Laptop[💻 Windows Laptop<br>(SMB Share)]
    TV[📺 Smart TV<br>(Jellyfin App)]
  end

  %% ===== Network Layer =====
  subgraph L2[Network Layer]
    Router[🌐 Home Router / Wi-Fi]
  end

  %% ===== Compute Layer =====
  subgraph L3[Compute Layer]
    Pi[🖥 Raspberry Pi 4<br>Ubuntu Server 25.10]
  end

  %% ===== Service Layer =====
  subgraph L4[Service Layer]
    Docker[🐳 Docker Engine]
    Jellyfin[🎬 Jellyfin<br>:8096]
    Filebrowser[📁 Filebrowser<br>:8081]
    Samba[🗂 Samba (SMB)<br>\\10.0.0.50\Media]
    Cockpit[🔧 Cockpit<br>:9090 (LAN only)]
  end

  %% ===== Storage Layer =====
  subgraph L5[Storage Layer]
    RAID[💾 RAID1 (mdadm)<br>/dev/md0 → /mnt/raid]
    DiskA[HDD A]
    DiskB[HDD B]
  end

  %% ===== Flows =====
  Phone --> Router
  Laptop --> Router
  TV --> Router

  Router --> Pi

  Pi --> Docker
  Docker --> Jellyfin
  Docker --> Filebrowser

  Pi --> Samba
  Pi --> Cockpit

  Jellyfin --> RAID
  Filebrowser --> RAID
  Samba --> RAID

  RAID --> DiskA
  RAID --> DiskB
```
