


⸻


# 🍏 macOS on Chromebook via Crostini + QEMU

Emulate Mac OS X or macOS inside the Linux container (Crostini) on a Chromebook. ⚠️ This is for educational purposes only. Performance will be slow (Crostini does **not** support KVM acceleration), but older versions of OS X are usable.

---

## 📋 Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Running macOS in QEMU](#running-macos-in-qemu)
- [Best macOS Version by Chromebook Board](#best-macos-version-by-chromebook-board)
- [Troubleshooting](#troubleshooting)
- [Credits](#credits)

---

## ✅ Requirements

- Chromebook with **Crostini (Linux Beta)** enabled
- Intel CPU (not ARM-based)
- At least **4 GB RAM** (8 GB+ preferred)
- 30–50 GB free storage in Linux container
- Debian 10/11 container (default on most Chromebooks)

---

## 🧱 Installation

### 1. Enable Linux (Crostini)

- Go to **Settings → Advanced → Developers**
- Turn on **Linux development environment**

### 2. Update Your Linux Container

```bash
sudo apt update && sudo apt upgrade -y

3. Install QEMU and Required Tools

sudo apt install qemu-system-x86 qemu-utils curl git -y

4. Create a macOS Working Directory

mkdir ~/macos && cd ~/macos

5. Download a macOS or OS X ISO (example: Mavericks)

curl -O https://archive.org/download/Install_OS_X_Mavericks/Install_OS_X_Mavericks.iso

You can replace this with El Capitan, Yosemite, Sierra, etc. from Archive.org or other sources.

⸻

🚀 Running macOS in QEMU

1. Create a Virtual Hard Drive

qemu-img create -f qcow2 macos_disk.qcow2 40G

2. Create a Launch Script

Create a new file called start.sh:

nano start.sh

Paste the following:

#!/bin/bash
qemu-system-x86_64 \
  -m 4096 \
  -cpu Penryn,vendor=GenuineIntel \
  -machine q35 \
  -smp 2 \
  -usb -device usb-kbd -device usb-mouse \
  -vga vmware \
  -drive file=macos_disk.qcow2,format=qcow2 \
  -cdrom Install_OS_X_Mavericks.iso \
  -boot d \
  -net nic -net user

Then make it executable:

chmod +x start.sh

3. Start macOS

./start.sh


⸻

🧠 Best macOS Version by Chromebook Board

Here’s a list of Chromebook board names and which OS X/macOS version runs best:

Board Name	CPU Gen	RAM	Best macOS Version	Notes
mario, zgb	Intel Atom	2GB	Snow Leopard (10.6)	Very old and light
lumpy, alex	Sandy Bridge	2–4GB	Lion (10.7)	Lightest usable GUI version
parrot, butterfly	Ivy Bridge	2–4GB	Mountain Lion (10.8)	Works okay
peppy, auron	Haswell	4GB	Yosemite (10.10)	Best blend of visuals/performance
glimmer, samus	Broadwell	4–8GB	El Capitan (10.11)	Stable and relatively smooth
octopus, hatch	8th Gen Intel	8GB+	Mojave (10.14)	Slow but usable
nami, nocturne	8th Gen i5/i7	8GB+	Catalina (10.15)	Bootable, laggy GUI
brya, volteer	11th–13th Gen	8GB+	Big Sur (11.0)	Painfully slow without KVM
ARM boards (kevin, coral, etc)	ARM/Rockchip	❌	Not supported	QEMU requires Intel x86 CPU


⸻

❗ Troubleshooting
	•	Stuck at Apple logo → Your ISO might be corrupted or missing boot files
	•	Mouse/keyboard don’t work → Add -usb -device usb-kbd -device usb-mouse
	•	QEMU says KVM not found → Crostini doesn’t support KVM. Just remove -enable-kvm
	•	Graphics slow → Try older OS X (like Mavericks or Yosemite)

⸻






⸻

❤️ Credits
	•	kholia/OSX-KVM
	•	myspaghetti/macOS-Simple-KVM
	•	QEMU.org
	•	mrchromebox.tech
	•	cros.tech (Chromebook board name reference)

---

