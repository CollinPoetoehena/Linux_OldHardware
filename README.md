# Linux on Old Hardware: Reviving a Windows Laptop

This repository documents my experiment to install Linux on an old Windows laptop, giving it a new lease on life and deepening my understanding of Linux systems.

This experiment focuses on Windows machines only, as installing Linux on Macs involves different procedures and considerations.

## Table of Contents

1. [Project Goals](#project-goals)
2. [Hardware Specifications](#hardware-specifications)
3. [Prerequisites](#prerequisites)
4. [Choosing a Linux Distribution](#choosing-a-linux-distribution)
5. [Preparation Steps](#preparation-steps)
   - [Backing Up Data](#backing-up-data)
   - [Creating a Bootable USB](#creating-a-bootable-usb)
   - [BIOS Configuration](#bios-configuration)
6. [Installation Process](#installation-process)
   - [Booting from USB](#booting-from-usb)
   - [Live Environment Testing](#live-environment-testing)
   - [Disk Partitioning](#disk-partitioning)
   - [Installing the System](#installing-the-system)
7. [Post-Installation Setup](#post-installation-setup)
   - [System Updates](#system-updates)
   - [Driver Installation](#driver-installation)
   - [Essential Software](#essential-software)
8. [Troubleshooting](#troubleshooting)
9. [Performance Optimization](#performance-optimization)
10. [Lessons Learned](#lessons-learned)
11. [Resources](#resources)

---

## Project Goals

The primary objectives of this experiment are:

- **Extend Hardware Lifespan**: Give new purpose to an old Windows laptop that would otherwise sit unused or be discarded
- **Reduce E-Waste**: Contribute to environmental sustainability by repurposing existing hardware
- **Learn Linux**: Gain hands-on experience with Linux installation, configuration, and system administration
- **Create a Secondary Machine**: Establish a functional secondary computer for lightweight tasks, learning, or experimentation
- **Cost-Effective Computing**: Demonstrate that not all computing needs require new hardware

---

## Hardware Specifications

This experiment was conducted on the following hardware (find on Windows machine by running `msinfo32` or checking system properties):
- Model: Acer Aspire A715-75G
- CPU: Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz, 2592 Mhz, 6 Core(s), 12 Logical Processor(s)
- RAM: 16GB
- Storage: 512GB SSD
- Original OS: Windows 11 Home

---

## Prerequisites

Before beginning the installation, gather the following:

### Hardware Requirements
- [ ] USB flash drive (8GB minimum, 16GB recommended)
- [ ] External backup drive (if preserving Windows data, otherwise, optional)
- [ ] Stable internet connection
- [ ] Power adapter (ensure laptop stays plugged in during installation)

### Software Requirements
- [ ] Linux distribution ISO file
- [ ] USB creation tool (Rufus, balenaEtcher, or dd)
- [ ] (Optional) Backup software for Windows data

### Knowledge Requirements
- Basic understanding of BIOS/UEFI settings
- Familiarity with disk partitioning concepts
- Awareness that this process may erase existing data

---

## Choosing a Linux Distribution

For older hardware, consider these lightweight distributions:

### Recommended for Old Hardware

| Distribution | RAM Requirement | Desktop Environment | Best For |
|-------------|-----------------|---------------------|----------|
| **Ubuntu** | 4GB+ | GNOME | Modern hardware, full-featured, great support |
| **Lubuntu** | 1GB+ | LXQt | Very old hardware, Windows-like interface |
| **Linux Mint Xfce** | 2GB+ | Xfce | Beginners, familiar interface |
| **Xubuntu** | 2GB+ | Xfce | Balance of features and performance |
| **Manjaro Xfce** | 2GB+ | Xfce | Rolling release, AUR access |
| **AntiX** | 256MB+ | IceWM/Fluxbox | Extremely old systems |
| **Peppermint OS** | 2GB+ | Xfce | Cloud-focused, lightweight |
| **MX Linux** | 2GB+ | Xfce | Stable, beginner-friendly |

### My Choice
- **Distribution**: [Ubuntu (Desktop)](https://ubuntu.com/download/desktop)
- **Version**: 24.04.3 LTS
- **Reason**: Good balance of user-friendliness, community support, and performance, combined with my familiarity from previous experiences. Furthermore, Ubuntu's extensive documentation and large user base make troubleshooting easier. It is also a modern distribution that supports a wide range of hardware, ensuring compatibility with the laptop's components.

---

## Preparation Steps

### Backing Up Data

If the laptop has any important data on Windows:

1. **Boot into Windows**
2. **Identify Important Files**:
   - Documents, photos, videos
   - Browser bookmarks and passwords
   - Application settings
3. **Copy to External Drive** or cloud storage
4. **Verify Backup** by checking file integrity

### Creating a Bootable USB

1. **Download the Linux ISO**:
   - Visit the official distribution website
   - Download the appropriate ISO file (usually ~2-4GB)

2. **Download Rufus** or **balenaEtcher**:
   - Rufus: https://rufus.ie/
   - balenaEtcher: https://www.balena.io/etcher/

3. **Create Bootable USB**:
   - Insert USB drive (will be erased!)
   - Launch Rufus/Etcher (from laptop, not necessary to run from the USB itself!)
   - Select the ISO file
   - Select the USB drive as the device (probably listed as "NO LABEL (D:)" or similar)
   - Check other settings (default usually fine)
   - Click "Start" and wait for completion (click "OK" on any prompts)

### BIOS Configuration

1. **Enter BIOS/UEFI**:
   - Restart the laptop
   - Press the BIOS key during startup (usually F2, F12, Del, or Esc)
   - Check the screen during boot for the correct key

2. **Adjust Settings**:
   - **Boot Order**: Move USB to first position
   - **Secure Boot**: Disable (if present, may prevent Linux from booting)
   - **Fast Boot**: Disable (optional, helps with boot issues)
   - **Legacy/UEFI Mode**: Note your current setting
     - Modern systems: Use UEFI
     - Older systems: May require Legacy/CSM mode

3. **Save and Exit**

---

## Installation Process

### Booting from USB

1. **Insert the bootable USB** into the laptop
2. **Restart** the laptop
3. **Select USB as boot device**:
   - Either from BIOS boot menu (F12, F9, or similar)
   - Or automatically if USB is first in boot order
4. **Select "Try Linux" or "Install"** from the boot menu

### Live Environment Testing

Before installing, test the live environment:

1. **Check Hardware Compatibility**:
   - Does the display work correctly?
   - Is Wi-Fi detected?
   - Do touchpad and keyboard work?
   - Does audio work?

2. **Test Performance**:
   - Open a web browser
   - Launch a few applications
   - Check overall responsiveness

3. **Wi-Fi Setup** (if needed):
   - Connect to your network
   - This allows downloading updates during installation

### Disk Partitioning

Choose your installation strategy:

#### Option 1: Erase Disk (Simplest)
- **Use Case**: No need to keep Windows
- **Result**: Entire disk used for Linux
- Select "Erase disk and install Linux" in the installer

#### Option 2: Dual Boot (Keep Windows)
- **Use Case**: Want both Windows and Linux
- **Steps**:
  1. Shrink Windows partition from Windows Disk Management first
  2. Create free space (50GB+ recommended for Linux)
  3. During Linux installation, select "Install alongside Windows"
  4. Or manually partition:
     - `/` (root): 25GB+ (ext4)
     - `/home`: Remaining space (ext4)
     - `swap`: 2GB or equal to RAM (swap)
     - `/boot/efi`: 512MB (if UEFI, FAT32)

#### My Choice: Option 1 - Erase Disk
I opted to erase the entire disk for a clean Linux installation, as I did not need to retain Windows (the laptop was an old laptop with no important data, all data was either in the cloud and/or on my new laptop).

#### Option 3: Manual Partitioning
```
Recommended partition scheme for standalone Linux:
/boot/efi   512MB   FAT32   (UEFI systems only)
/           30GB    ext4    (root filesystem)
swap        4GB     swap    (equal to RAM for hibernation)
/home       Rest    ext4    (user data)
```

### Installing the System

1. **Launch Installer**:
   - Double-click "Install" icon on desktop
   - Or select "Install" from the application menu

2. **Language Selection**:
   - Choose your preferred language

3. **Keyboard Layout**:
   - Select your keyboard layout
   - Test in the provided text box

4. **Installation Type**:
   - Select your chosen partitioning option
   - Review carefully before proceeding

5. **Location & Timezone**:
   - Select your location or timezone

6. **User Account**:
   - Enter your name
   - Create username (lowercase, no spaces)
   - Set a strong password
   - Choose whether to encrypt home folder (recommended)

7. **Begin Installation**:
   - Review all settings
   - Click "Install Now"
   - Confirm partition changes
   - Wait for installation (10-30 minutes)

8. **Restart**:
   - Remove USB when prompted
   - System will reboot into Linux

---

## Post-Installation Setup

### System Updates

First thing after installation:

```bash
# For Debian/Ubuntu-based distributions
sudo apt update
sudo apt upgrade -y

# For Arch-based distributions
sudo pacman -Syu

# For Fedora
sudo dnf update -y
```

### Driver Installation

Check for missing drivers:

```bash
# Check system information
inxi -Fxz

# Ubuntu: Check for additional drivers
sudo ubuntu-drivers autoinstall

# Or use GUI: Software & Updates > Additional Drivers
```

**Common Driver Issues**:
- **Wi-Fi**: May need proprietary drivers
- **Graphics**: Install appropriate drivers (NVIDIA, AMD)
- **Touchpad**: Usually works out of the box

### Essential Software

Install commonly needed applications:

```bash
# Development tools
sudo apt install git build-essential curl wget

# Multimedia
sudo apt install vlc gimp

# Office suite
sudo apt install libreoffice

# Additional useful tools
sudo apt install htop neofetch gnome-tweaks
```

**Via GUI**:
- Use Software Center/App Store to browse and install applications
- Look for: Browser (Firefox/Chrome), text editor, file manager extras

---

## Troubleshooting

### Common Issues and Solutions

#### Boot Issues
- **Problem**: System won't boot from USB
  - **Solution**: Check BIOS boot order, disable Secure Boot
  
- **Problem**: GRUB error after installation
  - **Solution**: Reinstall GRUB from live USB

#### Hardware Issues
- **Problem**: Wi-Fi not working
  - **Solution**: Install proprietary drivers, use USB Wi-Fi adapter temporarily
  
- **Problem**: Screen resolution incorrect
  - **Solution**: Install graphics drivers, edit `/etc/X11/xorg.conf`

#### Performance Issues
- **Problem**: System slow/laggy
  - **Solution**: Check running processes with `htop`, disable unnecessary startup programs

#### Sound Issues
- **Problem**: No audio output
  - **Solution**: 
    ```bash
    # Check audio devices
    aplay -l
    
    # Restart audio service
    pulseaudio -k
    pulseaudio --start
    ```
---

## Performance Optimization

### Reduce Startup Programs

```bash
# Check startup applications
# GUI: Search for "Startup Applications"
# Or edit autostart files in ~/.config/autostart/
```

### Enable ZRAM (Compressed RAM)

```bash
sudo apt install zram-config
```

### Use Lightweight Alternatives

Replace heavy applications with lighter ones:
- **Browser**: Firefox → Falkon/Midori
- **Office**: LibreOffice → AbiWord/Gnumeric
- **File Manager**: Nautilus → Thunar/PCManFM
- **Image Viewer**: GIMP → Mirage/Viewnior

### Disable Animations

```bash
# Xfce: Settings > Window Manager Tweaks > Compositor
# Reduce compositor settings or disable animations
```

### Monitor System Resources

```bash
# Install monitoring tools
sudo apt install htop iotop

# Check resource usage
htop
```

---

## Lessons Learned

_Document your personal experiences here as you progress:_

- What worked well?
- What challenges did you face?
- What would you do differently?
- Performance observations?
- Favorite features discovered?

TODO

---

## Resources

- [Article about how to install Linux on old hardware](https://www.linuxoperatingsystem.net/how-to-install-linux-os-on-your-old-pc/) - Full install guide on how to install Linux on old hardware
- [Linux Journey](https://linuxjourney.com/) - Interactive Linux learning   
- [The Linux Command Line](http://linuxcommand.org/) - Command line tutorial
- [DistroWatch](https://distrowatch.com/) - Linux distribution information

### Useful Commands Reference

```bash
# System information
uname -a                    # Kernel information
lsb_release -a             # Distribution information
df -h                      # Disk usage
free -h                    # Memory usage
lscpu                      # CPU information

# Package management (Debian/Ubuntu)
sudo apt update            # Update package list
sudo apt upgrade           # Upgrade packages
sudo apt install [package] # Install package
sudo apt remove [package]  # Remove package
sudo apt search [keyword]  # Search for package

# File operations
ls -lah                    # List files with details
cd [directory]             # Change directory
cp [source] [dest]         # Copy files
mv [source] [dest]         # Move/rename files
rm [file]                  # Remove file
mkdir [directory]          # Create directory

# System management
sudo systemctl status [service]  # Check service status
sudo systemctl start [service]   # Start service
sudo systemctl enable [service]  # Enable service at boot
journalctl -xe                   # View system logs
```

---
