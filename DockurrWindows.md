# 🚀 Dockurr Windows 101

**Run Windows (Desktop or Server) inside Docker — with full GUI support.**

This guide explains how to run **Windows**, **Windows Server**, and **Windows 11/10/7** using the amazing project **dockurr/windows**.

---

## 📌 1. Install Dockurr Windows Image

```bash
docker pull dockurr/windows
```

---

## 📌 2. Run Full Windows Desktop With GUI (Windows 11)

```bash
docker run -it --rm \
  -p 3389:3389 \
  -p 8006:8006 \
  --device /dev/kvm \
  -e RAM_SIZE=4G \
  -e CPU_CORES=4 \
  -e DISK_SIZE=30G \
  --shm-size=2g \
  dockurr/windows
```

### ✔ What happens:

* Docker starts a **QEMU VM**
* It downloads Windows ISO automatically
* Installs Windows automatically
* GUI is accessed via:

  * Web GUI: `http://localhost:8006`
  * RDP Client (recommended): connect to `localhost:3389`

Default credentials:

```
Username: Docker
Password: admin
```

---

## 📌 3. Run Windows Server (2022, 2025, etc.)

### ▶ Example: Windows Server 2022

```bash
docker run -it --rm \
  -p 3389:3389 \
  --device /dev/kvm \
  -e VERSION=2022 \
  dockurr/windows
```

### ▶ Windows Server 2025

```bash
docker run -it --rm \
  -p 3389:3389 \
  --device /dev/kvm \
  -e VERSION=2025 \
  dockurr/windows
```

### Available Versions

| VERSION     | Windows Edition     |
| ----------- | ------------------- |
| 2025        | Windows Server 2025 |
| 2022        | Windows Server 2022 |
| 2019        | Windows Server 2019 |
| 2016        | Windows Server 2016 |
| 11          | Windows 11          |
| 10          | Windows 10          |
| 8e          | 8.1 Enterprise      |
| 7u          | Windows 7 Ultimate  |
| xp          | Windows XP          |
| 2003        | Windows Server 2003 |
| … many more |                     |

---

## 📌 4. Docker Compose Example (Recommended)

Create `docker-compose.yml`:

```yaml
services:
  windows:
    image: dockurr/windows
    container_name: windows
    environment:
      VERSION: "2022"
      RAM_SIZE: "8G"
      CPU_CORES: "4"
    devices:
      - /dev/kvm
      - /dev/net/tun
    cap_add:
      - NET_ADMIN
    ports:
      - 8006:8006
      - 3389:3389/tcp
      - 3389:3389/udp
    volumes:
      - ./windows:/storage
    restart: always
    stop_grace_period: 2m
```

Start:

```bash
docker compose up -d
```

GUI:

* Browser → `http://localhost:8006`
* RDP → `localhost:3389`

---

## 📌 5. Change CPU, RAM, and Disk

```yaml
environment:
  RAM_SIZE: "8G"
  CPU_CORES: "4"
  DISK_SIZE: "128G"
```

---

## 📌 6. Choose Windows Language

```yaml
environment:
  LANGUAGE: "Arabic"
```

Available: Arabic, English, French, German, Japanese, Turkish, etc.

---

## 📌 7. File Sharing

Host ↔ Windows shared folder:

```yaml
volumes:
  - ./share:/shared
```

In Windows you will see: **Desktop → Shared**

---

## 📌 8. Run Custom ISO

### 1️⃣ Using URL:

```yaml
environment:
  VERSION: "https://example.com/custom.iso"
```

### 2️⃣ Using local ISO:

```yaml
volumes:
  - ./my.iso:/boot.iso
```

---

## 📌 9. GUI Access

### Web GUI (installer only)

```
http://localhost:8006
```

### RDP (recommended)

Linux:

```bash
xfreerdp /v:localhost /u:Docker /p:admin
```

Windows:

```
mstsc → type localhost
```

Android: Microsoft Remote Desktop
iPhone/iPad: Microsoft Remote Desktop

---

## 📌 10. Hardware Acceleration (KVM)

Check if VM support is enabled:

```bash
sudo pacman -S cpu-checker
sudo kvm-ok
```

If missing → enable VT-x / AMD-V in BIOS.

---

# 🎉 Summary

With `dockurr/windows`, you can run **any Windows version** inside Docker with GUI and KVM acceleration.
It is perfect for:

* Testing Windows apps
* Running Windows Server
* RDP lab
* Malware analysis
* Windows sandbox
* CI/CD automation

---
