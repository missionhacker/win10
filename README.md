# 🪟 Windows 10 Virtual Machine — Docker + KVM + RDP

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square\&logo=docker\&logoColor=white)
![Windows 10](https://img.shields.io/badge/Windows%2010-0078D6?style=flat-square\&logo=windows\&logoColor=white)
![KVM](https://img.shields.io/badge/KVM-Virtualization-FF6600?style=flat-square\&logo=linux\&logoColor=white)
![RDP](https://img.shields.io/badge/RDP-Remote%20Desktop-0078D4?style=flat-square\&logo=windows\&logoColor=white)
![Codespaces](https://img.shields.io/badge/GitHub-Codespaces-181717?style=flat-square\&logo=github)

> A lightweight Windows 10 virtualization setup using **Docker, KVM, and Remote Desktop Protocol (RDP)**.

This project allows you to deploy a Windows 10 virtual machine inside a Docker-based environment and access it remotely through **RDP** or a browser-based **noVNC** interface.

---

## ✨ What This Project Does

The setup uses a Linux-based environment as the host and runs Windows 10 as a virtual machine through QEMU/KVM.

```text
┌───────────────────────────────┐
│        GitHub Codespace       │
│                               │
│  ┌─────────────────────────┐  │
│  │     Docker Container    │  │
│  │                         │  │
│  │   ┌─────────────────┐   │  │
│  │   │   QEMU + KVM    │   │  │
│  │   │                 │   │  │
│  │   │    Windows 10   │   │  │
│  │   └─────────────────┘   │  │
│  │                         │  │
│  │    RDP / noVNC Access   │  │
│  └─────────────────────────┘  │
└───────────────────────────────┘
```

---

## 🚀 Key Features

* 🪟 Windows 10 virtual machine
* 🐳 Docker-based deployment
* ⚡ QEMU/KVM virtualization
* 🖥️ Remote access through RDP
* 🌐 Browser access through noVNC
* 💾 Persistent Windows storage
* 📦 Windows ISO mounted separately
* ☁️ Suitable for cloud development environments
* 🔧 Easy to rebuild and redeploy

---

## 📋 Requirements

### Host Environment

You need a Linux environment with:

* Docker
* QEMU
* KVM support
* `/dev/kvm` access
* Sufficient CPU and RAM
* Administrative/root privileges where required

### Additional Files

You will also need:

* Windows 10 ISO
* Dockerfile
* Persistent Docker volume for Windows data

> **Note:** Hardware virtualization support may not be available in every GitHub Codespaces configuration. If `/dev/kvm` is unavailable, KVM acceleration cannot be used.

---

## 📁 Project Structure

```text
win10/
│
├── docker
├── README.md
├
│
└── data/
    └── Windows VM storage
```

---

## 🔧 Deployment

### 1. Clone the Repository

```bash
git clone https://github.com/missionhacker/win10
cd win10
```

### 2. Build the Docker Image

```bash
docker build -t wind10 .
```

### 3. Start the Windows 10 VM

```bash
docker run -it --rm \
  --device /dev/kvm \
  -p 6080:6080 \
  -p 3389:3389 \
  -v windows_data:/data \
  -v windows_iso:/iso \
  win10
```

---

## 🖥️ Remote Desktop Access

Once Windows 10 has been configured and the RDP service is running, connect using:

```text
localhost:3389
```

For a remote environment such as GitHub Codespaces, expose/forward port **3389** through the Codespaces port forwarding system and connect using an appropriate RDP client.

### Windows RDP Client

Press:

```text
Win + R
```

Then run:

```text
mstsc
```

Enter the forwarded RDP address and connect.

---

## 🌐 noVNC Access

The container also exposes port:

```text
6080
```

Open the forwarded port in your browser:

```text
http://localhost:6080
```

This provides browser-based graphical access without requiring a traditional RDP client.

---

## 💾 Persistent Storage

Windows VM data is stored in a Docker volume:

```bash
-v windows_data:/data
```

This allows the Windows installation and configuration to persist even when the container is recreated.

List available volumes:

```bash
docker volume ls
```

---

## 🔍 Check KVM Availability

Before starting the VM, verify that KVM is available:

```bash
ls -la /dev/kvm
```

You can also check virtualization support with:

```bash
lsmod | grep kvm
```

If `/dev/kvm` does not exist, the host environment may not provide hardware virtualization.

---

## 🛠️ Useful Docker Commands

### Stop the container

```bash
docker stop <container_id>
```

### View running containers

```bash
docker ps
```

### View logs

```bash
docker logs <container_id>
```

### Remove the image

```bash
docker rmi windows10-vm
```

### List volumes

```bash
docker volume ls
```

---

## ⚠️ Important Notes

This project is intended for **testing, development, learning, and laboratory environments**.

Performance depends heavily on the underlying host system and whether KVM hardware acceleration is available.

Running Windows inside Docker does **not** mean Windows itself is a native Docker container. Docker is being used as the environment for launching the virtualization stack.

---

## 📌 Architecture

```text
User
 │
 ├─────────────── RDP :3389 ───────────────┐
 │                                         │
 └────────────── noVNC :6080 ──────────────┤
                                           ▼
                              ┌─────────────────────┐
                              │   Docker Container  │
                              │                     │
                              │   QEMU / KVM        │
                              │          │          │
                              │          ▼          │
                              │     Windows 10      │
                              │                     │
                              └─────────────────────┘
```

---

## 📄 License

This project is provided for educational and experimental purposes.

Use Windows installation media and software according to their respective licenses.

---

## ⭐ Credits

Built with:

* Docker
* QEMU
* KVM
* Windows 10
* noVNC
* RDP
* GitHub Codespaces
