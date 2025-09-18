# Debian trixie 13 Journey

### Installation process

Chose 'manual installation'
Chose the ssd i want to install to
Created boot partition, /boot mount point, ext4 type, boot flag, 512MB
Created swap partition, swap type, swap flag, 8GB
Created / partition, ext4 type, All space available

Next i chose debian desktop enviroment + gnome + basic system tools (i want to exclude debian desktop enviroment + gnome in the future in favour of hyprland)

### First steps, config, etc 
vision nocturna
performance mode
Night light
added spanish latinoamerican as keyboard (besides english us)
set timezone to utc-3
nautilus -> preferences -> sort folders before files
disable mouse acceleration

#### added the following keyboard shortcuts:
super shift s as interactive screenshot
ctrl alt T as gnome-terminal
alt + wasd as window resizing
super + d as hide all normal windows

#### Changed backgroung image and profile pic
settings -> appereances -> backgroung image 
settings -> system -> profile pic

#### Add user to sudoers
su
sudo usermod -aG sudo username
sudo reboot

(as your user)
sudo visudo /etc/sudoers

add lines: '
youruser    ALL=(ALL) NOPASSWD:ALL
'

sudo apt upgrade
sudo apt update

#### Automatically mount disks

Access 'disks'
Select desired disk
Click Additional partition options (ruedita)
Click 'Edit Mount Options'
Disable User defaults
Edit Mount Point to a recognizible name
Save

#### add shortcuts for other disks
go to desired folder, hit ctrl + D

#### Log in to accounts
- Netflix -> Enable DRM
- Google (unc.edu.ar)

#### Firefox

Log in with my account, sync in bookmarks
Settings -> Ctrl+Tab cycles through tabs in recently used order
Second click next to url bar -> Bookmarks Toolbar -> always show

Add RoboForm firefox extension

#### Flatpak was missing so i installed it with

sudo apt install flatpak

sudo apt-get --reinstall install -y gnome-software-plugin-flatpak

sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

#### OS tweaks
- removed unused software like:
sound recorder
mail app
music
gnome tour
videos

- add non-propietary repos
sudo nano /etc/apt/sources.list
addd 'contrib non-free' after 'main' in every line

sudo apt update

- install nice-to-have programs
sudo apt install fastfetch htop btop -y

- installed Thunderbird through software app (deb)
- Installed Discord through software app (flatpak)
- Installed Telegram through software app (flatpak)
- Installed Extension Manager ""
    - Inside extension manager, installed:
        - Dash to Dock
            Click Behaviour, "focus, minimize or show previews"
            Shrink icons
        - Blur my shell
        - Caffeine
        - Clipboard Indicator
        - Removable drive menu
        - Vitals
            Settings -> Use Higher precision
            Disable system track
            Added cpu usage, system temperature and ram gb usage
        - Gtk4 Desktop Icons NG (DING)
- Gnome Tweaks
    added minimize, maximize buttons
- Installed Shortcut in software app
- Added fastfetch at every terminal except vscode
    nano ~/.bashrc

    add the following lines

    if [ "$TERM_PROGRAM" != "vscode" ]; then
        fastfetch
    fi

#### Installed Gparted

#### Downloaded balenaEtcher

#### Installed gdisk

sudo apt update && sudo apt install gdisk

#### Use gdisk to change partition table from mbr to gpt

sudo fdisk -l
sudo gdisk /dev/sda
'select option 3, blank gpt'
'press w' to write
'press y' to confirm

#### Install steam

go to steam page https://steamcommunity.com/ and download .deb

install dependencies like
sudo apt install curl

install with 
sudo dpkg -i <name>.deb
then press enter twice for installing all missing dependencies

#### Install wow via lutris (via wine) https://www.youtube.com/watch?v=vlV26V8yE1A https://www.youtube.com/watch?v=NUjQDl1xzGs

##### Install wine https://gitlab.winehq.org/wine/wine/-/wikis/Debian-Ubuntu
sudo dpkg --add-architecture i386 
- Remember version os with 
cat /etc/os-release
(trixie)
sudo mkdir -pm755 /etc/apt/keyrings
wget -O - https://dl.winehq.org/wine-builds/winehq.key | sudo gpg --dearmor -o /etc/apt/keyrings/winehq-archive.key -

sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/debian/dists/trixie/winehq-trixie.sources

sudo apt update

sudo apt install --install-recommends winehq-staging

##### Install nvidia drivers (optional i think)
- Installed synaptic package manager (to install nvidia drivers) through software app

Went to synaptic package manager, searched nvidia
Installed 'firmware-nvidia-gsp 550' (not sure this is the one)

### DONT DO THIS
#### TRIED INSTALLING NVIDIA-SMI TO CHECK NVIDIA DRIVERS AND BROKE EVERYTHING
Installed nvidia-smi with apt package

it broke everything

##### TO FIX IT I HAD TO:
ENTER TERMINAL INSTANCE: CTRL + ALT + F3
sudo apt purge nvidia-smi
sudo apt remove --purge '^nvidia-.*'
sudo apt install xserver-xorg-video-nouveau
sudo dpkg-reconfigure xserver-xorg
sudo reboot

##### Install nvidia-detect
sudo apt install nvidia-detect

### DONT DO THIS
##### Not sure which version of the nvidia driver i need, the latest seem to be 580 but official docs point to 550, some docs say 535 as the more stable but you might miss out on newer fixes, so i go with 550

### DO NOT RUN
sudo apt install -y nvidia-driver-550 libvulkan1 libvulkan1:i386

### IMPORTANT
and this crashed again everything

i did

sudo nvidia-bug-report.sh

then

gunzip nvidia-bug-report.log.gz

and then repetead the nouveau fix commands

Now i will try with a deepseek answer + some debian wiki https://wiki.debian.org/NvidiaGraphicsDrivers#Debian_13_.22Trixie.22

## Solution Steps:

### 1. First, completely remove all NVIDIA packages and clean up:
```bash
sudo apt purge nvidia-* libnvidia-*
sudo apt autoremove
sudo apt clean
```

### 2. Blacklist the Nouveau driver (critical step):
```bash
sudo touch /etc/modprobe.d/blacklist-nouveau.conf
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```

Add these lines:
```
blacklist nouveau
options nouveau modeset=0
```

### 3. Update initramfs:
```bash
sudo update-initramfs -u
```

### 4. Install Linux Headers
```bash
sudo apt install linux-headers-$(dpkg --print-architecture)
```

### 5. Install the correct NVIDIA drivers:
```bash
#official install
sudo apt install nvidia-kernel-dkms nvidia-driver firmware-misc-nonfree
```

### 6. Rebuild kernel modules:
```bash
sudo dpkg-reconfigure nvidia-kernel-dkms
```

### 7. Reboot:
```bash
sudo reboot
```

### 8. Config
Go to Nvidia x-server to check that everything is okay
go to display settings in gnome settings to select the correct resolution

#### Install Lutris https://lutris.net/downloads https://www.youtube.com/watch?v=vlV26V8yE1A

echo -e "Types: deb\nURIs: https://download.opensuse.org/repositories/home:/strycore/Debian_12/\nSuites: ./\nComponents: \nSigned-By: /etc/apt/keyrings/lutris.gpg" | sudo tee /etc/apt/sources.list.d/lutris.sources > /dev/null

wget -q -O- https://download.opensuse.org/repositories/home:/strycore/Debian_12/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/lutris.gpg

sudo apt update

sudo apt install lutris

#### Lutris did not work for installing battle net :(
#### Some solutions online indicate:
https://forums.lutris.net/t/last-battle-net-installer-not-working/23063
Hello, you will need to install WIne Staging 10.7 TKG, follow this steps:

How to install Wine Staging 10.7 TKG to Lutris using ProtonPlus and set it as default Lutris WIne version

    install ProtonPlus with Flatpak: Install ProtonPlus on Linux | Flathub 53
    To install ProtonPlus from a terminal:
    flatpak install flathub com.vysp3r.ProtonPlus
    You can also compile it from sources: GitHub - Vysp3r/ProtonPlus: A modern compatibility tools manager for Linux. 17

    Then open ProtonPlus and install Wine-Staging 10.7 TKG for Lutris

    Now set default Lutris Wine version to Wine Staging 10.7 TKG In Lutris left panel,
    choose Wine Runner, click on config icon, then, in “Runner options” tab,
    change Wine version to Wine Staging 10.7 TKG and save


→ set “GE-Proton latest” as default in Lutris to resolve Battle.Net 5 install issue
in Lutris menu, preferences, runners, wine, click on the wheel, set “GE-Proton latest” as default Lutris Wine version
→ use “GE-Proton latest” to play WOW, Warcraft III, starcraft etc…
→ If Battle.Net 5 interface is black, enable hardware acceleration in Battle.Net 5 options

If you don’t have “GE-Proton latest” in your list, you will need to install GE-Proton 10.4 (or an older version) with a tierce app like ProtonPlus

#####
https://www.reddit.com/r/cachyos/comments/1ke6tea/battlenet_via_lutris_failing_to_reinstall/

    Install Proton 10 Beta from steam

    Select in Lutris

    Proceed Installation



#### Installed graphic/image/video editing software through software app
- GIMP
- Krita
- Inkscape
- KdenLive
- OBS

##### Run update-grub

sudo update-grub
