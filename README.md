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
   - [Live Environment Testing](#live-environment-testing)
6. [Installation Process](#installation-process)
   - [Creating a Bootable USB](#creating-a-bootable-usb)
   - [BIOS Configuration](#bios-configuration)
   - [Booting from USB](#booting-from-usb)
   - [Disk Partitioning](#disk-partitioning)
7. [Post-Installation Setup](#post-installation-setup)
   - [System Updates](#system-updates)
   - [Driver Installation](#driver-installation)
   - [Essential Software](#essential-software)
8. [Fun Experiments & Learning Projects](#fun-experiments--learning-projects)
   - [Container Technologies](#1-container-technologies)
   - [Kubernetes & Orchestration](#2-kubernetes--orchestration)
   - [Programming Languages](#3-programming-languages)
   - [Infrastructure as Code](#4-infrastructure-as-code)
   - [CI/CD & DevOps Tools](#5-cicd--devops-tools)
   - [Monitoring & Observability](#6-monitoring--observability)
   - [System Administration](#7-system-administration-practice)
9. [Troubleshooting](#troubleshooting)
10. [Performance Optimization](#performance-optimization)
11. [Lessons Learned](#lessons-learned)
12. [Resources](#resources)

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

If the laptop has any important data on Windows (in this experiment I skipped this step since it was just an old laptop with no important data):

1. **Boot into Windows**
2. **Identify Important Files**:
   - Documents, photos, videos
   - Browser bookmarks and passwords
   - Application settings
3. **Copy to External Drive** or cloud storage
4. **Verify Backup** by checking file integrity

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

## Installation Process

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
The BIOS Configuration section explains how to access your laptop's firmware settings (BIOS/UEFI) to configure it for booting from USB. This is not a Windows application, it's a low-level system interface that exists before Windows even loads.

To configure these settings:

1. **Access BIOS/UEFI**:
   - Shut down your laptop completely (not restart)
   - Press the power button to turn it on
   - Immediately and repeatedly press one of these keys:
      - F2 (most common for Windows laptops, in this experiment it was F2)
      - F12 (opens boot menu)
      - Del or Esc (alternative keys)
      - Watch the screen during boot, the laptop usually shows a splash screen with the correct key

2. **Adjust Settings**:
   - **Boot Order**: Move USB to first position
   - **Secure Boot**: Disable (if present, may prevent Linux from booting)
     - **Important**: Security settings are often locked by default
     - If you cannot edit settings, you must first set a Supervisor Password:
       1. Select "Set Supervisor Password"
       2. Enter and confirm a new password
       3. **Write this password down** - you'll need it to access BIOS later (if you forget it, you may be locked out of BIOS and need to contact the manufacturer for a reset)
       4. Settings will now become editable
     - **Alternative Approach (I selected this option during the experiment)**: Skip this step and try installing Ubuntu with Secure Boot enabled (Ubuntu 24.04 LTS supports Secure Boot). Only return to disable Secure Boot if installation fails.
   - **Fast Boot**: Disable (helps with boot issues)
   - **Intel RST/Optane**: Change to AHCI mode (will otherwise cause installation issues on Ubuntu). 
      - If you're **erasing Windows** (Option 1 of Disk Partitioning), this change is completely safe and you do not need to back up any data
      - On some laptops the option may be hidden. In this experiment (Acer laptop), pressing Ctrl+s revealed the option under Main > SATA Mode, where you could select AHCI.
      - On Ubuntu it may prompt you to disable it if you skip this step: [Intel RST (Rapid Storage Technology)](https://documentation.ubuntu.com/desktop/en/latest/reference/intel-rst-during-ubuntu-installation/)
   - You'll see a message: "Turn off RST" or "Change storage controller to AHCI"
   - **Legacy/UEFI Mode**: Note your current setting
     - Modern systems: Use UEFI
     - Older systems: May require Legacy/CSM mode

3. **Save and Exit** (usually F10)
---

### Booting from USB

It may already start from the USB if you set it as the first boot device in the previous step (skip the first steps). If not, you can manually follow the first steps to boot from the USB:

1. **Insert the bootable USB (if not done already)** into the laptop
2. **Restart** the laptop
3. **Select USB as boot device**:
   - Either from BIOS boot menu (F12, F9, or similar)
   - Or automatically if USB is first in boot order
4. **Select "Try or Install Linux Ubuntu" (the option to install Linux from the USB)** from the boot menu
5. **Follow on-screen instructions** to load the live environment or start installation. Follow the official installation guide for your chosen distribution for detailed steps, such as [Ubuntu Installation Guide](https://ubuntu.com/tutorials/install-ubuntu-desktop).

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

## Fun Experiments & Learning Exercises Executed on Linux

Now that your Linux system is up and running, here are some hands-on experiments to deepen your DevOps and systems knowledge:

### 1. Container Technologies

- **Docker**: Industry-standard container platform - [Get Started Guide](https://docs.docker.com/get-started/) | [Installation](https://docs.docker.com/engine/install/ubuntu/)
- **Podman**: Rootless Docker alternative - [Getting Started](https://podman.io/get-started)
- **Docker Compose**: Multi-container orchestration - [Overview](https://docs.docker.com/compose/)

### 2. Kubernetes & Orchestration

- **Minikube**: Local Kubernetes cluster - [Getting Started](https://minikube.sigs.k8s.io/docs/start/) | [Kubernetes Tutorials](https://kubernetes.io/docs/tutorials/)
- **K3s**: Lightweight Kubernetes distribution - [Quick-Start Guide](https://docs.k3s.io/quick-start) | [Documentation](https://docs.k3s.io/)
- **Kind**: Kubernetes in Docker - [Quick Start](https://kind.sigs.k8s.io/docs/user/quick-start/)

### 3. Programming Languages

- **Go**: Systems programming language - [Go Tour](https://go.dev/tour/) | [Go by Example](https://gobyexample.com/) | [Installation](https://go.dev/doc/install)
- **Rust**: Memory-safe systems language - [The Rust Book](https://doc.rust-lang.org/book/) | [Rustlings Exercises](https://github.com/rust-lang/rustlings)
- **Python**: Scripting and automation - [Python Tutorial](https://docs.python.org/3/tutorial/) | [Real Python](https://realpython.com/)
- **Node.js**: JavaScript runtime - [Getting Started](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs) | [Node School](https://nodeschool.io/)
- **Java**: Enterprise programming language - [Java Tutorials](https://docs.oracle.com/javase/tutorial/) | [Learn Java](https://dev.java/learn/) | [Installation](https://openjdk.org/install/)
- **TypeScript**: Typed superset of JavaScript - [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) | [TypeScript Deep Dive](https://basarat.gitbook.io/typescript)

### 4. Infrastructure as Code

- **Terraform**: Infrastructure provisioning - [Tutorials](https://developer.hashicorp.com/terraform/tutorials) | [Get Started](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)
- **Ansible**: Configuration management - [Getting Started](https://docs.ansible.com/ansible/latest/getting_started/index.html)
- **Pulumi**: Infrastructure as actual code - [Get Started](https://www.pulumi.com/docs/get-started/) | [Examples](https://github.com/pulumi/examples)

### 5. CI/CD & DevOps Tools

- **Jenkins**: Automation server - [Tutorial](https://www.jenkins.io/doc/tutorials/) | [Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)
- **GitLab CI/CD**: Built-in CI/CD platform - [Quick Start](https://docs.gitlab.com/ee/ci/quick_start/) | [Examples](https://docs.gitlab.com/ee/ci/examples/)
- **GitHub Actions**: Workflow automation - [Quickstart](https://docs.github.com/en/actions/quickstart) | [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)
- **ArgoCD**: GitOps continuous delivery - [Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)

### 6. Monitoring & Observability

- **Prometheus**: Metrics and alerting - [Getting Started](https://prometheus.io/docs/prometheus/latest/getting_started/) | [Query Basics](https://prometheus.io/docs/prometheus/latest/querying/basics/)
- **Grafana**: Visualization and dashboards - [Tutorials](https://grafana.com/tutorials/) | [Getting Started](https://grafana.com/docs/grafana/latest/getting-started/)
- **ELK Stack**: Log management (Elasticsearch, Logstash, Kibana) - [Getting Started](https://www.elastic.co/elastic-stack/)
- **Loki**: Log aggregation system - [Overview](https://grafana.com/docs/loki/latest/get-started/overview/)

### 7. System Administration Practice

- **Linux Command Line**: [The Linux Command Line Book](https://linuxcommand.org/tlcl.php) | [Linux Journey](https://linuxjourney.com/)
- **Shell Scripting**: [Bash Guide](https://mywiki.wooledge.org/BashGuide) | [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/)
- **Command Practice**: [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) - Interactive command line game
- **SSH & Networking**: [SSH Essentials](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)

---

## Troubleshooting

### Common Issues and Solutions encountered during this Experiment:

#### Boot Issues
- **Problem**: System won't boot from USB
  - **Solution**: Check BIOS boot order, disable Secure Boot
  
- **Problem**: GRUB error after installation
  - **Solution**: Reinstall GRUB from live USB

- **Problem**: System freezes during boot, such as "Your device ran into a problem and needs to restart. We'll restart for you." 
  - **Solution**: This often happens if Intel RST driver was removed from Storage Controllers but BIOS is still in RST/Optane mode. Boot into Windows Safe Mode to restore:
    1. Power off the laptop completely
    2. Remove the USB drive
    3. Turn on and immediately start tapping **F8** or **Shift+F8** repeatedly to enter Windwos Recovery/Automatic Repair Mode. This can be tricky on modern laptops due to fast boot times, so you may need to try a few times. In this experiment, it went to a mode where it did not have a network connection and prompted to press Enter to see other recovery options. Here I selected Quick Repair and enabled Hotspot on my mobile phone for network connection.
    4. If that doesn't work, go back into the BIOS/UEFI settings and set the SATA Mode to AHCI and boot with Ubuntu on USB:
       - Press power button
       - Enter BIOS/UEFI (see steps above for explanation)
       - Change SATA Mode to AHCI as explained in the earlier steps for BIOS/UEFI configuration
       - Save and Exit
       - Boot from the USB with Linux, such as Ubuntu, and continue the installation
       - Repeat this 3 times - Windows will boot into Recovery mode

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

### What was the goal of this project, and what did I want to learn/explore in short?
As a DevOps Engineer, I undertook this project to deepen my practical Linux knowledge by performing a bare-metal installation on older hardware. The goal was to gain hands-on experience with system-level operations, troubleshooting, and configuration that directly applies to infrastructure management and cloud deployments.

### What worked well, and what didn’t?
- **Ubuntu 24.04 LTS** proved to be an excellent choice with strong hardware compatibility and comprehensive documentation
- The **live environment testing** allowed validation of hardware support before committing to installation
- **Community resources** (Ubuntu forums, documentation) were valuable for troubleshooting
- Modern Linux distributions handle most driver installations automatically, making the process smoother than expected
- **Intel RST/Optane to AHCI conversion** was the most significant hurdle. Some laptops hide these BIOS options, requiring keyboard shortcuts (Ctrl+S) to reveal advanced settings. Furthermore, I removed the Intel RST driver from Storage Controllers before changing the SATA mode, which caused Windows to break and required to set the SATA Mode in BIOS options. In hindsight, I should have changed the SATA mode to AHCI and not remove the driver to avoid this issue.
- **BIOS/UEFI navigation** varies significantly between manufacturers; took several attempts to find the right settings
- Troubleshooting hardware-specific issues requires patience and systematic debugging

### What did I learn from doing this project, and how can I apply these learnings in my work?
- **Low-level system understanding**: Gained deeper appreciation for boot processes, storage controllers, and firmware, directly applicable when troubleshooting cloud instances or containerized environments
- **Troubleshooting methodology**: Systematic problem-solving (checking logs, trying alternatives, consulting documentation) mirrors production incident response
- **Hardware abstraction insights**: Experiencing bare-metal installation enhances understanding of virtualization and containerization abstractions used in DevOps
- **Foundation for further learning**: Having a dedicated Linux environment opens up opportunities to experiment with Docker, Kubernetes, infrastructure automation tools, and other DevOps technologies in a safe, low-stakes environment
- **Hands-on DevOps experimentation**: The Fun Experiments & Learning Exercises Executed on Linux section provided additional practical learning opportunities, such as setting up containerization tools (Docker, Podman), orchestration platforms (Kubernetes, Minikube), IaC tools (Terraform, Ansible), and CI/CD pipelines, deepening my understanding of DevOps workflows and reinforcing system administration fundamentals in a real Linux environment

### Would I do anything differently next time?
- **Disable Fast Boot earlier** in BIOS to avoid boot timing issues
- **Change SATA mode to AHCI first** before attempting any other troubleshooting to avoid Windows breakage
- **Document BIOS paths immediately** when finding hidden settings, since it is easy to forget specific navigation steps
- Consider using **USB keyboard/mouse** if laptop peripherals have compatibility issues during installation

Applications to DevOps Work:
This hands-on experience strengthens my ability to:
- Debug Linux systems at a fundamental level
- Understand cloud instance boot processes and storage configurations
- Troubleshoot container runtime and orchestration issues
- Make informed decisions about OS selection for different workloads
- Communicate effectively with infrastructure teams about system-level concerns

---

## Resources

- [Article on how to install Linux on old hardware](https://www.linuxoperatingsystem.net/how-to-install-linux-os-on-your-old-pc/) - Full install guide on how to install Linux on old hardware
- [Article on installing Linux on Windows laptops](https://linuxvox.com/blog/how-to-install-ubuntu-from-windows/) - Guide on how to install Linux on Windows laptops
- [Ubuntu Documentation](https://documentation.ubuntu.com/) - Official Ubuntu documentation
- [Ubuntu Installation Guide](https://ubuntu.com/tutorials/install-ubuntu-desktop) - Official Ubuntu installation instructions
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
