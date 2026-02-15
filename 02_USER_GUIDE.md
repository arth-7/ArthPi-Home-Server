# ArthPi Home Server – User Guide

This guide explains how to use the server in simple everyday language.

No Linux knowledge required.

---

# 📥 How to Upload Files

You have two main ways to upload files:

1. From your phone (easiest)
2. From your laptop (fastest)

---

## 📱 Option 1 – Upload From Phone (Recommended)

### Step 1 – Connect to Home Wi-Fi

Make sure your phone is connected to the same Wi-Fi network as the server.

---

### Step 2 – Open File Browser

In your phone browser, go to:

http://10.0.0.50:8081


Login if required.

---

### Step 3 – Choose Folder

Upload movies to:

/movies


Upload TV shows to:

/series


---

### Step 4 – Upload

Tap the upload button and select your video files.

Wait for upload to complete.

That’s it.

---

# 💻 Option 2 – Upload From Windows Laptop (Drag & Drop)

### Step 1 – Open File Explorer

In the address bar, type:

``` If that doesn’t work, try: ``` \\arthpi\Media ``` --- ### Step 2 – Login Use: - Username: `arth` - Password: (your Samba password) --- ### Step 3 – Drag and Drop Copy files into: - `movies` - `series` Done. --- # 🎬 How to Watch Movies Open Jellyfin in your browser or TV app: ``` http://10.0.0.50:8096 ``` Select your library and press play. --- # 📺 Watching on TV If using a smart TV: - Install Jellyfin app - Add server: - Address: `10.0.0.50` - Port: `8096` Login and stream. --- # 📁 File Naming Tips (Keeps Library Clean) Movies: ``` Movie Name (2024).mp4 ``` TV Shows: ``` Show Name └── Season 01 └── Show Name - S01E01.mp4 ``` Clean naming = better organization. --- # ⚠ Important Rules - Do not unplug the drives while server is running. - Do not power off by pulling the plug. - Avoid unnecessary reboots. - RAID protects against one disk failure, not total data loss. - Important files should still be backed up elsewhere. --- # 🔄 If Something Stops Working First try: 1. Refresh browser 2. Restart Jellyfin app 3. Reboot server only if necessary If needed, contact the administrator. --- End of User Guide ``` --- Reply: **Done** Then we move to: 📄 `03_ADMIN_GUIDE.md` (proper maintenance, RAID replacement steps, safe reboot procedure, monitoring commands, etc.)
