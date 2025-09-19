# Debian Journey

## System Information

* **OS:** Debian GNU/Linux 13 (trixie) x86\_64
* **Kernel:** Linux 6.12.43+deb13-amd64
* **Packages:** 2517 (dpkg), 17 (flatpak)
* **Shell:** bash 5.2.37
* **Display (HDMI-0):** 1600x900 @ 60 Hz \[External]
* **DE:** GNOME 48.4
* **WM:** Mutter (X11)
* **Terminal:** GNOME Terminal 3.56.2

**Hardware:**

* **CPU:** Intel(R) Core(TM) i7-7700K (8) @ 4.50 GHz
* **GPU:** NVIDIA GeForce GTX 1050 Ti \[Discrete]
* **Memory:** 9.59 GiB / 15.58 GiB (62%)
* **Swap:** 0 B / 7.45 GiB (0%)

**Storage:**

* **Disk (/):** 128.13 GiB / 211.22 GiB (61%) - ext4
* **Disk (/mnt/HDDBLACK):** 463.76 GiB / 465.76 GiB (100%) - fuseblk \[Read-only]
* **Disk (/mnt/WINDOWS):** 107.50 GiB / 111.03 GiB (97%) - fuseblk \[Read-only]

---

## Installation Process

* Chose **manual installation**
* Selected SSD for installation
* Created boot partition `/boot` (ext4, boot flag, 512MB)
* Created swap partition (swap type, swap flag, 8GB)
* Created `/` partition (ext4, all remaining space)
* Installed: Debian desktop environment + GNOME + basic system tools

  * (*Next time*: exclude GNOME, try Hyprland instead)

---

## First Steps & Configuration

* Enabled **night light** & **performance mode**
* Keyboard: English (US) + Spanish (LatAm)
* Timezone: UTC-3
* Nautilus preferences: “Sort folders before files”
* Disabled mouse acceleration

### Custom Shortcuts

* `Super + Shift + S` → interactive screenshot
* `Ctrl + Alt + T` → GNOME Terminal
* `Alt + WASD` → window resizing
* `Super + D` → hide all windows

### Personalization

* Changed background & profile picture
* Added user to sudoers via `visudo`
* Updated system with `apt update && apt upgrade`

---

## Disk Management

* Used **Disks** app for automatic mounts
* Created shortcuts for mounted drives
* Added recognizable mount points

---

## Accounts

* Netflix (enabled DRM)
* Google (unc.edu.ar)

---

## Firefox Setup

* Synced bookmarks via Firefox account
* Enabled: *Ctrl+Tab cycles tabs by recent order*
* Showed bookmarks toolbar
* Installed **RoboForm** extension

---

## Flatpak Installation

```bash
sudo apt install flatpak
sudo apt-get --reinstall install -y gnome-software-plugin-flatpak
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

---

## OS Tweaks

* Removed unused software (Sound Recorder, Mail, Music, GNOME Tour, Videos)
* Added non-free repos: `contrib non-free` to `/etc/apt/sources.list`
* Installed essentials: `fastfetch`, `htop`, `btop`

### Installed Software

* **Deb:** Thunderbird
* **Flatpak:** Discord, Telegram, Extension Manager
* **GNOME Extensions:** Dash to Dock, Blur My Shell, Caffeine, Clipboard Indicator, Removable Drive Menu, Vitals, DING (Desktop Icons NG)
* **Tweaks:** added minimize/maximize buttons
* **Other:** Shortcut app, GParted, BalenaEtcher, gdisk, Steam, Lutris, GIMP, Krita, Inkscape, KdenLive, OBS

---

## Driver Issues & Fixes

* Installed NVIDIA drivers via Synaptic (`firmware-nvidia-gsp 550`)
* Broke system trying `nvidia-smi` → fixed by purging NVIDIA, reinstalling Nouveau
* Later used proper NVIDIA setup:

  * Blacklisted Nouveau
  * Installed `nvidia-kernel-dkms nvidia-driver firmware-misc-nonfree`
  * Rebuilt kernel modules
  * Rebooted and configured with NVIDIA X-server

---

## Gaming Setup

* Installed Wine (Staging) and dependencies
* Installed Lutris (from openSUSE repo)
* Set up ProtonPlus for Battle.net workaround
* Configured Lutris to use GE-Proton builds

---

## Final Notes

* Run `sudo update-grub` after changes
* Avoid blindly installing NVIDIA drivers without checking Debian Wiki

---
