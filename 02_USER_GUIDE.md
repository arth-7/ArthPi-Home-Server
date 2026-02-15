# ArthPi Home Server – User Guide

This guide explains how to use the server in simple everyday language.

No Linux knowledge required.

---

# How to Upload Files

You have two main ways to upload files:

1. From your phone 
2. From your laptop

---

##  Option 1 – Upload From Phone

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
## Option 2 – Upload From Windows Laptop (Drag & Drop)

This is the fastest way to upload large files from your computer.

---

### Step 1 – Open the Server Folder

1. Press **Windows + E** to open File Explorer.  
2. Click inside the address bar.  
3. Type:

```
\\10.0.0.50\Media
```

4. Press **Enter**.

If that address does not open, try:

```
\\arthpi\Media
```

---

### Step 2 – Enter Login Details

If Windows asks for credentials, enter:

- **Username:** `arth`  
- **Password:** (your Samba password)

If this is your personal laptop, you may check **Remember my credentials**.

---

### Step 3 – Copy Your Files

You will see folders such as:

- `movies`
- `series`

Open the correct folder and drag your files into it.

Wait until the transfer finishes completely before closing the window.

Once the copy completes, your files are available on the server.
