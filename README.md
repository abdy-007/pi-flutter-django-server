# 🚀 Pi-Server: Flutter Web + Django API 

Welcome to the central repository for the Pi-Server! This project is a fully containerized, decoupled web architecture designed to be cross-compiled on a standard desktop and deployed onto a Raspberry Pi (ARM64). 

It features a **Flutter Web** frontend served by an ultra-lightweight **Nginx** reverse proxy, communicating seamlessly with a **Django REST API** backend powered by **Gunicorn**. 

---

## 🏗️ Architecture Overview

* **Frontend:** Flutter (compiled to static HTML/JS/CSS) hosted on `nginx:alpine`.
* **Backend:** Django hosted on `python:3.11-slim` running via Gunicorn.
* **Routing:** Nginx intercepts all `/api/` traffic and funnels it to Django, completely eliminating CORS headaches.
* **Target Hardware:** Raspberry Pi (ARM64).

---

## 🛠️ Phase 1: Installing Dependencies

Because we are building ARM64 images on standard x86_64 computers, we need specific tools to emulate the Raspberry Pi's architecture during the build process. Choose your operating system below.

### 🐧 Linux (Arch / Ubuntu)
Linux requires a manual setup of QEMU to emulate the ARM architecture.

**For Arch/EndeavourOS:**
```bash
# Install Docker, Buildx, and QEMU
sudo pacman -Syu git docker docker-buildx qemu-user-static qemu-user-static-binfmt

# Restart the bin format service so the kernel recognizes ARM binaries
sudo systemctl restart systemd-binfmt

# Start and enable the Docker daemon
sudo systemctl enable --now docker
For Ubuntu/Debian:

Bash
# Install Docker and Git
sudo apt update
sudo apt install git docker.io docker-buildx docker-compose

# Install QEMU for ARM emulation
sudo apt install qemu-user-static binfmt-support

# Start and enable the Docker daemon
sudo systemctl enable --now docker
🪟 Windows 10/11
Windows makes cross-compilation surprisingly easy thanks to WSL2 (Windows Subsystem for Linux).

Install Git: Download and install Git for Windows.

Enable WSL2: Open PowerShell as Administrator and run: wsl --install. Restart your PC if prompted.

Install Docker Desktop: Download and install Docker Desktop.

Configure Docker: Open Docker Desktop, go to Settings > General, and ensure "Use the WSL 2 based engine" is checked. QEMU and cross-architecture support are built into Docker Desktop automatically!

🚀 Phase 2: Cloning the Repository
Once your system is prepped, pull the codebase down to your local machine:

Bash
# Clone the repository
git clone [https://github.com/YOUR-USERNAME/pi-flutter-django-server.git](https://github.com/YOUR-USERNAME/pi-flutter-django-server.git)

# Enter the project directory
cd pi-flutter-django-server
(Note: Replace YOUR-USERNAME with your actual GitHub username!)

⚙️ Phase 3: Building and Running
Before we spin up the server, we need to create a dedicated Docker builder that understands how to compile code for the Raspberry Pi (linux/arm64).

1. Initialize the Multi-Arch Builder (All OS)
Run this command once to set up your builder:

Bash
docker buildx create --name pi-builder --use
docker buildx inspect --bootstrap
Look for linux/arm64 in the output to confirm it worked!

2. Build the Backend (Django)
Navigate to the backend folder and compile the ARM64 image:

Bash
cd backend
docker buildx build --platform linux/arm64 -t pi-django-api:latest . --load
cd ..
3. Build the Frontend (Flutter)
(Prerequisite: You must have the Flutter SDK installed on your host machine to generate the static web files first. Run flutter build web inside the frontend/ directory before running this step).

Navigate to the frontend folder and compile the ARM64 Nginx proxy:

Bash
cd frontend
docker buildx build --platform linux/arm64 -t pi-flutter-web:latest . --load
cd ..
4. Ignite the Server
With both images built and loaded, use Docker Compose from the root directory to link them together and start the server:

Bash
docker-compose up
Boom! 💥 Your server is now alive.
Open your browser and navigate to http://localhost. Nginx will serve you the Flutter app, and any calls to http://localhost/api/ will hit your Django backend.

🚨 Troubleshooting & Support
Cross-compiling infrastructure can sometimes throw weird errors depending on your exact hardware and operating system updates.

If you will encounter any troubles, ask AI. Drop the error logs directly into your preferred AI assistant—especially if it's a YAML formatting complaint or a QEMU emulation warning—and it will help you debug the pipeline instantly. Happy building!