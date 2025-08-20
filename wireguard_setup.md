## WireGuard VPS Setup Guide - Ubuntu 24.04
**Panduan Step-by-Step Installation & Configuration**

## Prerequisites Check

### 1. Sistem Requirements
```bash
# Check Ubuntu version
root@NetPhantom:~# lsb_release -a

# Check available memory
root@NetPhantom:~# free -h

# Check disk space
root@NetPhantom:~# df -h

# Ensure system is up to date
root@NetPhantom:~# apt update && apt upgrade -y
```

### 2. Network & Firewall Check
```bash
# Check IP Public
root@NetPhantom:~# curl -4 ifconfig.me
165.101.18.19

# Check if port 51820 is available
root@NetPhantom:~# ss -tulpn | grep :51820

# Install UFW if not installed
root@NetPhantom:~# apt install ufw -y

# Allow SSH (important!)
root@NetPhantom:~# ufw allow ssh (port ssh)

# Allow WireGuard port
root@NetPhantom:~# ufw allow 51820/udp
```

## Installation Process

### Step 1: Download WireGuard Installer
```bash
root@NetPhantom:~# wget https://git.io/wireguard -O wireguard-install.sh
```

### Step 2: Run Installation
```bash
root@NetPhantom:~# bash wireguard-install.sh
```

### Step 3: Configuration Prompts

```
Welcome to this WireGuard road warrior installer!

Which IPv4 address should be used?
     1) 165.101.18.20
     2) 10.0.4.5
IPv4 address [1]: 1

What port should WireGuard listen on?
Port [51820]: (Enter)

Enter a name for the first client:
Name [client]: User0xbyalak

Select a DNS server for the client:
   1) Default system resolvers
   2) Google
   3) 1.1.1.1
   4) OpenDNS
   5) Quad9
   6) AdGuard
   7) Specify custom resolvers
DNS server [1]: 3

WireGuard installation is ready to begin.
Press any key to continue...
Get:1 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Hit:2 http://archive.ubuntu.com/ubuntu noble InRelease
Hit:3 http://archive.ubuntu.com/ubuntu noble-updates InRelease
Hit:4 http://archive.ubuntu.com/ubuntu noble-backports InRelease
Get:5 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [1081 kB]
Get:6 http://security.ubuntu.com/ubuntu noble-security/main Translation-en [187 kB]
Get:7 http://security.ubuntu.com/ubuntu noble-security/main amd64 Components [21.6 kB]
Get:8 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [881 kB]
Get:9 http://security.ubuntu.com/ubuntu noble-security/universe Translation-en [195 kB]
Get:10 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Components [52.2 kB]
Get:11 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [1625 kB]
Get:12 http://security.ubuntu.com/ubuntu noble-security/restricted Translation-en [359 kB]
Get:13 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Components [212 B]
Get:14 http://security.ubuntu.com/ubuntu noble-security/multiverse amd64 Components [208 B]
Fetched 4528 kB in 2s (1917 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
qrencode is already the newest version (4.1.1-1build2).
The following NEW packages will be installed:
  wireguard wireguard-tools
0 upgraded, 2 newly installed, 0 to remove and 5 not upgraded.
Need to get 0 B/92.2 kB of archives.
After this operation, 345 kB of additional disk space will be used.
Selecting previously unselected package wireguard-tools.
(Reading database ... 106502 files and directories currently installed.)
Preparing to unpack .../wireguard-tools_1.0.20210914-1ubuntu4_amd64.deb ...
Unpacking wireguard-tools (1.0.20210914-1ubuntu4) ...
Selecting previously unselected package wireguard.
Preparing to unpack .../wireguard_1.0.20210914-1ubuntu4_all.deb ...
Unpacking wireguard (1.0.20210914-1ubuntu4) ...
Setting up wireguard-tools (1.0.20210914-1ubuntu4) ...
wg-quick.target is a disabled or a static unit, not starting it.
Setting up wireguard (1.0.20210914-1ubuntu4) ...
Processing triggers for man-db (2.12.0-4build2) ...
Scanning processes...
Scanning linux images...

Pending kernel upgrade!
Running kernel version:
  6.8.0-71-generic
Diagnostics:
  The currently running kernel version is not the expected kernel version 6.8.0-78-generic.

Restarting the system to load the new kernel will not be handled automatically, so you should consider rebooting.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
Created symlink /etc/systemd/system/multi-user.target.wants/wg-iptables.service → /etc/systemd/system/wg-iptables.service.
Created symlink /etc/systemd/system/multi-user.target.wants/wg-quick@wg0.service → /usr/lib/systemd/system/wg-quick@.service.

█████████████████████████████████████████████████████████████████████████
█████████████████████████████████████████████████████████████████████████
████ ▄▄▄▄▄ █▀████ ▄▄▀█▀▄▄█▄▄  ▀  ▄ ███▄▀▀█▄▄▄▀▄▄ █▀  ▄ ▀ ▀█▄▀█ ▄▄▄▄▄ ████
████ █   █ █▄▀█▄  ▀▄▀▄▀▄ █▀ ▄▀ ▀▄▄   █▄███▀▄ ▄    ▄ █▀█▀ ▄ ▀▄█ █   █ ████
████ █▄▄▄█ █ ▀▀▀▀█▄ ██▄▄█▀██▀▀▄██▀ ▄▄▄ █▀▄   █ ▀█▀▀▄▀▄ ▀▄▄▀█▄█ █▄▄▄█ ████
████▄▄▄▄▄▄▄█ ▀▄▀▄▀▄▀▄█ █▄█ ▀ ▀▄▀▄▀ █▄█ █▄█ ▀ ▀ █▄▀ ▀ █ █▄▀▄▀▄█▄▄▄▄▄▄▄████
████▄▄▀▄██▄▄█ ▄▄ █▄▄ ▀▄ ▀▀███▄▀▀  ▄    ▄▀▄▀  ▀  ▀  ▄▀ ▄▀▀▀  ▄▀▄  ▀▄ ▀████
████▄█▀▀▄▄▄ ▄▀█▀▄██▄ ▀ ▀▀█▀▄ ▄▄ ██▄   █▀█ ▀██▄  █▀█▀█ ▀▀ █  █ ▄▄▀▀▀▄▄████
████▄▄▀  █▄ ▀▀ ▄▄▀█▀▀▄▄▄▄ ▄ ▄▀█   ▀█▄██▄█▄▀█▀▄▀▀ █▀▀ ▄▀█ █▀█▄▄ ▀▀█▄▀ ████
████▄██▀▀ ▄▄█ █ ▀█  ▄▀█ █▀▀▀▄▀█▀▄ ▄ ▀█▀▄ ▄█▀▄ ▄▀█▄▄  ▄▀▄▀  ▀ ▀█▄ ▀█▄ ████
████▀███▄█▄▄▀▀ ▄ ▀█▀ ▄ ▄▀ ▀▄██ ▄▄▄██▄  ▀█▀ ▀█ █▀▀▄██▄█▀ ███ █  ▀ ▀█▀ ████
████▀▄██ █▄ █▀█▄ ▄▀▄▀█▄▄▄█ ▄▄█▄ ▀▀▀█ ▀▀▀▀█▀█ ▀█ ██▄▀ █▀▄ ██▄▀▀█▀▀ ▄█ ████
████▄▀▄██ ▄██▀▀  ▄  ▄▄▄███▄█████▀▀ ▀▀▀▀ ▄ ▀▀ ▀█▄▀ █▄  ▀ ▄▄▄█▄▀▀▀ ▀ ▄█████
████▄▄▄▄▄▀▄▀█ █▄  █ █▀▀▀▀██▄ ▄  █▄█▄▄ ▄▄  █████  ▀ █ ▀▄▀ ▄▀▀ ▀█▄ ▀ ▄ ████
█████▄▄█▀█▄ ██▄█▄ ▀▄▀█▀█ ▄▄▀ ██▄▀  █▀▄ █ ▀▄█▀▀██▀█▀██ ▄█▀  ▄█▀ ▀▄█ ▀▀████
██████▄▀█ ▄  ██  ▄▄▀▄▄ ▀▄█ ▀▄▀▀  ██▀▀ ▀▄█ ▀█▄ ▀█▀   █ ██  █▀█▀█▄▄▀ █▄████
████ ███ ▄▄▀▀▀ ▀ ▄ █▀▀ ▄▄ ▀▀ ▄█ ▀██▀ ██ █▀▄▄▄█▀▄▄ ▄ ▄▀▀▀██▀█ ▄▄ ▀ ▀█▄████
████▀  █ ▄▄▄ ▀▄ █▄███▄▀▀▀ ██▀ ▄▀▀▀ ▄▄▄ █ █▀▀ █ █▄▀█  █▀▀▄▄▀  ▄▄▄ █▄▄ ████
████▀█▀  █▄█ ▀█▄▀▄ ▄▄█ ██  █▄█▀█▄▀ █▄█  █▀█▀ ▀▀█▄▀ ▄▄▄█  ▀█▀ █▄█ █ ▄ ████
████▀▀ ▄ ▄▄ ▄█▀ ▄▀▀ ▀▄▀ █  ▄ ▄▀▄▀  ▄▄▄▄▄▄▀▀███▄▀▀ █ █▄▀  ▄█▄   ▄▄▄▀▀█████
████  ▀█▀▄▄▄▄▄▀█▀▄  █ █▄▀ ▀█ ▀▄▀ ▀ ▄▄██▀   ▀▄▄ ▀▄▀ █▀ ▄▄▀██▀▄▀▄▀ ▄██▄████
████▄█ ▄▀▄▄█▀▄▀▀█ ▀▀▀▄ ▀ █▄▀▄█▄  ▄▄  ▀ ▀▀ █▀█▀▄▀▄▀▄ ▀▀▀█▄ █▀▀▀█▀ █▄█▀████
████▄▄ █▀█▄▄ █▄▄▄ █▄██▄█ ▀▄█▄█▄▀▀▄▄█ ▄▀▀ ▄ ▄▄█▀▀▄▀ ▄ ▀▀█ ▄▀▄ ▀▀▀█ ▄▀█████
████    ▀ ▄▀  ▄█▄▀ ▀██▀█ █▄ ▄▀▀  █▀█▄█▄▀ ▄▄▀█ ███▀█▀▄█▀▀▄▀  ▄▀▀▀█ █▄█████
████▀▄▄█ ▀▄▀ ▄█▀▄ ▀ ▄ ▀ █   ▄ █ █▀▀  ██▄█ ███▀ ██▄▀▄▄  █ ██▀▀▄▀▀██ ██████
████ ▄▀▀▀▄▄█▄▄ ▄▄▀▀ █  ▀ █ █▄█▄  ███ █ ▀▀  █▄██▄▄█▀  █▄▀▀▄█▀▀█▀▀▀ ▀▀█████
████▄█▀▄▄▄▄▀▀▄▄▄ █ █   █▄  █▄▀ █  █ ▄▀▄███▀ █▀██▄▄▀▀▄▀█▄ ▀▄ ▄▄ ▄▄ █▀▄████
████▄   ▄█▄ █ █▄▄███▄▄▄▄▀█▄ █▀ ▀▀▀ ▀▀▀█ ▀ ▄█▄ ▄▀▀██ ▄ ▄ ▄▄▄██▄▄ ▀███ ████
████▀ ▀█▀ ▄ ▄█ █▄ ▄▄█▀▄▀▄██   ▀█▀▀  █▀█▄ ▀▄▀▀▄ █▀▀▄▀ █ ▄▄ ▄█ ▀▀▄▀ █▀▀████
████▀█▄ █▄▄██▀▀█▄██ ▄█▀▄██  █▀▄ █▄█▄█▀▄▀█▄▄█▀ ▄▀▄ █▀▄▄ ▀▄ ▀█▄ ▄ ▀▀█ ▄████
█████▄▄█▄█▄█▀█  █  ▀█ ▀▀▀▄ ▄█▀▄▀▄▄ ▄▄▄  ▀█ █▀▄█▀ ▄▀█▀▀ ▄ ███ ▄▄▄ ▀ ▀▀████
████ ▄▄▄▄▄ █▄ ▄▄█▄▄ ▄▀ ▀ █ ▄ ▄█ ▀▄ █▄█ ▀█▀ █▄▄  ▄ ▀█▀▀▄█ █▄▄ █▄█  ██▀████
████ █   █ █▀▄ ▄█▀ █▄█▀▀   ▀▄▀  █▀ ▄  ▄█ ▀ █▄ ▀ ▄  ▄▄ █▄▄▀█▄   ▄ ▄█▀▀████
████ █▄▄▄█ █▀█▄ ▀▀ █▀█▄█ ▄▄█▄▀▄█▄█ █▄▀█▄  ▀▀▄▄ ▀█▀▄  ██▀▄▀ ▀█▄ █▄▀█▀▄████
████▄▄▄▄▄▄▄█▄▄▄█▄█▄█▄▄▄████▄▄█▄██▄▄▄▄█▄█▄█▄▄█▄▄▄▄▄▄█▄▄▄▄███▄▄██▄▄▄█▄█████
█████████████████████████████████████████████████████████████████████████
█████████████████████████████████████████████████████████████████████████
↑ That is a QR code containing the client configuration.

Finished!

The client configuration is available in: /root/User0xbyalak.conf
New clients can be added by running this script again.
```

## Post-Installation Configuration

### Step 4: Verify Installation
```bash
# Check WireGuard status
root@NetPhantom:~# systemctl status wg-quick@wg0

# Check interface
root@NetPhantom:~# wg show

# Check if service is enabled
root@NetPhantom:~# systemctl is-enabled wg-quick@wg0

# Enable if not enabled
root@NetPhantom:~# systemctl enable wg-quick@wg0
```

### Step 5: Get Client Configuration
```bash
# List generated client files
root@NetPhantom:~# ls -l ~/*.conf

# Display client config (untuk copy-paste)
root@NetPhantom:~# cat User0xbyalak.conf
[Interface]
Address = 10.7.0.2/24, fddd:2c4:2c4:2c4::2/64
DNS = 1.1.1.1, 1.0.0.1
PrivateKey = IMw1ftPLTEt14qilr5We+eSxM5t1JUzSHQI2DVKs82I=

[Peer]
PublicKey = rWifz9/PllmhsVhvjgtw+WLwzaUmWJzUPP8RkAXWnB8=
PresharedKey = kwyP5qCBCzs4TU0GU9/S5WB+6oGJMCppU78RIU3JnkE=
AllowedIPs = 0.0.0.0/0, ::/0
Endpoint = 165.101.18.19:51820
PersistentKeepalive = 25

# Show QR code untuk mobile
root@NetPhantom:~# qrencode -t ansiutf8 < ~/User0xbyalak.conf
```

## Client Setup Instructions

### For Mobile (Android/iOS)
1. Install WireGuard app dari Play Store/App Store
2. Tap **+** button
3. Select **Create from QR code**
4. Scan QR code yang di-generate server
5. Tap **Save** dan aktifkan connection

### For Desktop (Windows/macOS/Linux)
1. Download WireGuard client dari wireguard.com
   
   <img width="1819" height="847" alt="image" src="https://github.com/user-attachments/assets/f5bbd8cd-79c5-4131-ad28-cd820baf41b6" />

2. Install aplikasi
3. Import file `.conf` yang sudah di-generate
   ```bash
   # di terminal laptop
   PS C:\Users\iyhor> scp -P 2025 root@165.101.18.19:/root/User0xbyalak.conf H:/Downloads/
   User0xbyalak.conf                                                                     100%  349     3.8KB/s   00:00
   ```
4. Add tunnel
   
   <img width="444" height="186" alt="image" src="https://github.com/user-attachments/assets/7485b036-6d69-4f72-b2dd-6e72dcff3157" />

5. Activate tunnel
   <img width="1818" height="801" alt="image" src="https://github.com/user-attachments/assets/a418fd37-1a53-4fbb-b522-fea2e7bae965" />

## Client Management

### Add New Client
```bash
# Run installer
root@NetPhantom:~# bash wireguard-install.sh

WireGuard is already installed.

Select an option:
   1) Add a new client
   2) Remove an existing client
   3) Remove WireGuard
   4) Exit
Option: 1

Provide a name for the client:
Name: User1

Select a DNS server for the client:
   1) Default system resolvers
   2) Google
   3) 1.1.1.1
   4) OpenDNS
   5) Quad9
   6) AdGuard
   7) Specify custom resolvers
DNS server [1]: 3

█████████████████████████████████████████████████████████████████████████
█████████████████████████████████████████████████████████████████████████
████ ▄▄▄▄▄ █ ▀ ██▄▄ ▄█▀█▄▄████▀▀▀ ▄ ▀▀▄▄  ▄█▀▀  ▄█ ▀▄█▄▄▄██▄▀█ ▄▄▄▄▄ ████
████ █   █ █ ▀ ▀█▄ ▀ ▀█▀▀▀  ▄█▀ ▀▄▀▀▀▄ ▀▀▀▀█▄▄▀██ ▀ ▄██ ▄▄ ▀▄█ █   █ ████
████ █▄▄▄█ █▀▄▄ ██▄▀ ▄ ▄█▄███ ▄▀█▀ ▄▄▄ ▄█▄ ▄▄█▄▀██▀▄█▄ █▄▄▀█▄█ █▄▄▄█ ████
████▄▄▄▄▄▄▄█▄█▄█▄▀▄▀ █ █▄█ ▀ ▀▄▀▄▀ █▄█ █▄█▄▀ ▀▄▀▄▀▄█▄█ ▀ ▀ ▀▄█▄▄▄▄▄▄▄████
████▄ ▀█▄▄▄▀▀█ ▄▀▀   ▄ ▀▄█▄ ▀█▄█▀█ ▄ ▄ ▀██▄ ▄▄▀ █▀▄▀▀█▀▄ ▀▀█▄█▀ ▀  ▄ ████
████▀▄  ██▄▀  █  █   ▄▀▄▄▄▄▀▀▀▀▀ ▀  █▀ ▄▄█▄▄  ▀█ ▀  ███ ▄▀ ▄█   ▀▄▄█▀████
████▀ ▄▀ █▄█ ▄▀▄█ ▄█▄  ▄▄▄▀▀▄▄▀  ▄▀▀█▀  ▀ █▀▄██▄▄▀▄▄█▀█▀█▀█   ███  ██████
████▄██▀ ▄▄▄▄▄▀  ▀▄▄▀▄▀▀  █▄ █   ▀▀█▀ ▀▄█▄  ▄ ▀██▀▄ █▄▀▀▀ ██  █▄▀▄▀█▄████
████▀█  ▄▄▄▄ ▄█  █▀██ ▄▄▀█ █ █ ███ █▄▄  ▀█ ▄█ ▀▀▄▀▄  █▀▀     ▀▄▀█ ▄█▄████
████▀▄▀ ▀▄▄ █▀██▄█▄▀ ▀ ▄ ▀█▀ ██ ▀  █▀▄▀█▄█▀▄▄▀▄ █▄▄ ▀█▀▄ █▄█ ▀▄   █▄ ████
████▀█▀▄  ▄▄▄█▀ ███▀▀▄▀▄ ▄▄ ▀▀▀▀ ▄▄ ▄▄▀▀█▀▄▄▀▄▀▀▄▄▄▀▄▀▄▄█▀ ▄▀█ ▄▄ █▀▄████
████▀▀▄▀██▄██▄▀▀▀ █▄█▄ ▄▄▄ ▀▀▀█▀ ▀▄▀█▀█  █  ▀  █▀ █ █▄█▄█▀▄▄█  █▀▄███████
████ █ ▄ ▄▄▄▄ ▀▄ █▄▄▀▀█ ▄ ▀█▄ ▀ ▄▄▄▄▀ █▀▄▄ ▄ ▄█ █ ▄▄█ █▀▄▄▀█▄ █   ▄█▄████
████ ████▀▄██  ▀▄▄▀▀▀▄▀█▄▄▄▀██▀▀▄██▄▀▀█▄▄▄▀▄  ▀▀  ▄ ███▄▄▀█▄█▀▀▄██ ▄ ████
█████ ▄█  ▄█ ▀ ▄▀ █▄█▀▄▀ ▄▀▄ ▄██▀ ▀▄▀▀█ ▀ █ ▄ ▀▄▄▄▄  ▀▀█████  ▄ █ ▀▀▄████
████▄    ▄▄▄ █▀  █ ▀ ▄ ▄█▄█▀██ ▄ ▄ ▄▄▄  ██▄  █ █▀▄▀▀██  ▄▀   ▄▄▄  ██ ████
████▄██  █▄█ ▀  ▀▀ █▀▀█  ▀▄▄▀▄▀▀█▄ █▄█  ▀▀▄███ ▀ ▀▀▀ █▀▀▀▄▄  █▄█ ██▄▀████
████▀█▀▄  ▄▄  ▀▀▀▀█ █▄██▄▄  ▄█▄ ██ ▄▄▄ ██▄█  ▀█▄▄▀▀▄▄ ▄ ▄ ██  ▄ ▄▀▄█▀████
█████▄█  ▀▄███ ▄▀█▀▄ ▀ ▀█▀█▀▄█▀███▄▀ █▀▀▄█▄ ▀ ▄▄ ██▀██  ▄▀▀█ ▀▀█▄▀▀▀▀████
████▄▀▀█▀ ▄  ▀▄▀▄▀  █▄▀█ ▄ ▀█▀▄▀▄▄█▄ ▄█ ▀▄█  ▀▀█▄ ▄▄▄██    ▄▀▀   ▀▄▄█████
████ ▀▄▀█ ▄▄▀█▀▄▄▄█ ██ █ █▄█ █▄█▀▄ █ ▀▄▀▄▄ ▀▄ ▀▄██▄▀▄█▀█  ▀▄ █▀▀▀ ▄██████
████▄ █ ██▄ ▄▀ ▄█ ▄ █▄ █▀▄▄▀█▀ ▀ ▄ ███▀ ▀▄█ ▄▀██▄ ▄ ▄█▀ █▀ ▄█ ▀ ▄ ▄██████
████▀██ ▀█▄▀▄█▀▄██▀▀▀██▄▄▄▀▀▄▀ ▄▀█▄▀▄▀▄▀▀██▀▄ ▀▄   ▀ ▀█▀▀ ▀ ▀▀▄█▄ ▀▀▄████
████ ▀  ▄ ▄█▄▀▀▀▄▄█▄█▄ ▀ ▄▄▄ ▀█▀▄▀  ▀▄█  ▀█▄▀▀▄█▀ ▄▀▀▄█▄▄▀▄█▄ ▀▀█▄▀  ████
████▀ ▀█▀█▄ ▀▄█ ▄█ ▀██▄█ ▀ ▄█ ███▀▄▀▄█▀▀▀█▄█ ▀ █▀▄▄ ▀▀█ ▀▀ █  █  ▄ ██████
████▄█   ▄▄▄▀▄▄██▀▀ ▀▀ █▀▀█▀█▄█ █▀██▄ ▀█▄██▀▄ ▄█▀▀▄ ██▄ ▀███▀█   ▀█▄ ████
████▀█▀▄▀█▄█▄▀▄ ▀▄█ █▄▀▀▄▄▄ ▄ ▄▀▀▀▄ ██ ██▀▄▄▀▄▄█▀▀█▀▄█  ▄  █ █▀ ▀ ██ ████
████▀█▄ █▄▄  █ ▀ ██▀▄▄ ▄▄▄ ▀▄▀█▀██▄▄▄█▀ █▄█▄▀▀█▀▀▀█▄▀▄▀▄ ▀ ▄█▄▀  ▀▄▀▄████
█████▄▄█▄█▄█▀▄▀██▄██▄██ ▄ ▀▀▀ ▀██▀ ▄▄▄ ██▄█▀▄▄▄█▄█▄▀▀█▀▀ ██▄ ▄▄▄ ▀██ ████
████ ▄▄▄▄▄ █▀▀▀▀▀▀▀▀▀▄▀▄█▄█▀▀▀ ▀▄▀ █▄█ ▄▄▄█▄ ▀██ ▀█ ▀▄ ▄▄▀▄▀ █▄█ █ ▄▄████
████ █   █ █▄█▄ ▄▀▄▄█▀█▀ ▄██ ▄▄ ▀█▄ ▄▄▄▀▄█▀▀▀▄▄█ ▄█  █▀ ▀█▀▀▄▄▄ ▄▀▀█▄████
████ █▄▄▄█ █▀ █▄ █ ▀ ▄ ▄█▄▀ ▀█ █ ▄▄▀▄▀  █▄▀▄   ██ ▄ ███ ▄▀█ ██▄██ █  ████
████▄▄▄▄▄▄▄█▄█▄███▄█▄█▄▄█▄▄█▄▄█▄█▄▄▄▄█▄▄██▄████▄▄███▄█▄████▄▄██▄▄██▄█████
█████████████████████████████████████████████████████████████████████████
█████████████████████████████████████████████████████████████████████████
↑ That is a QR code containing your client configuration.

User1 added. Configuration available in: /root/User1.conf
```

### Remove Client
```bash
# Run installer
root@NetPhantom:~# bash wireguard-install.sh

WireGuard is already installed.

Select an option:
   1) Add a new client
   2) Remove an existing client
   3) Remove WireGuard
   4) Exit
Option: 2

Select the client to remove:
     1) User0xbyalak
     2) User1
Client: 2

Confirm User1 removal? [y/N]: y

User1 removed!
```

### List All Clients
```bash
# Check server config
root@NetPhantom:~# sudo cat /etc/wireguard/wg0.conf
# Do not alter the commented lines
# They are used by wireguard-install
# ENDPOINT 165.101.18.19

[Interface]
Address = 10.7.0.1/24, fddd:2c4:2c4:2c4::1/64
PrivateKey = wEBdUs2bRKLBXMVut2G8PudeN6HoTxo9psFqyA7TSlM=
ListenPort = 51820

# BEGIN_PEER User0xbyalak
[Peer]
PublicKey = Ucfzh9ZmUA5GEwZuoPPcyjyO28i53y3Ww/lPSLR1JzA=
PresharedKey = kwyP5qCBCzs4TU0GU9/S5WB+6oGJMCppU78RIU3JnkE=
AllowedIPs = 10.7.0.2/32, fddd:2c4:2c4:2c4::2/128
# END_PEER User0xbyalak

# Or check with wg command
root@NetPhantom:~# wg show wg0
interface: wg0
  public key: rWifz9/PllmhsVhvjgtw+WLwzaUmWJzUPP8RkAXWnB8=
  private key: (hidden)
  listening port: 51820

peer: Ucfzh9ZmUA5GEwZuoPPcyjyO28i53y3Ww/lPSLR1JzA=
  preshared key: (hidden)
  endpoint: 103.226.232.164:57076
  allowed ips: 10.7.0.2/32, fddd:2c4:2c4:2c4::2/128
  latest handshake: 1 minute, 15 seconds ago
  transfer: 1.02 MiB received, 1.64 MiB sent
```

## Testing & Verification

### Step 6: Check IP Address
`https://https://whatismyipaddress.com/`

**Sebelum**

<img width="1919" height="947" alt="image" src="https://github.com/user-attachments/assets/5a969db5-340e-44ea-a7e8-6cdaf9bc1f4a" />


**Sesudah**

<img width="1919" height="943" alt="image" src="https://github.com/user-attachments/assets/4bd5f5bc-1544-4c19-9300-e25ea76aea9a" />

### Step 7: Ping Server
```bash
PS C:\Users\iyhor> ping 10.7.0.1

Pinging 10.7.0.1 with 32 bytes of data:
Reply from 10.7.0.1: bytes=32 time=55ms TTL=64
Reply from 10.7.0.1: bytes=32 time=45ms TTL=64
Reply from 10.7.0.1: bytes=32 time=44ms TTL=64
Reply from 10.7.0.1: bytes=32 time=45ms TTL=64

Ping statistics for 10.7.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 44ms, Maximum = 55ms, Average = 47ms
```

### Step 8: Server Monitoring
```bash
# Real-time connection monitoring
root@NetPhantom:~# watch wg show

Every 2.0s: sudo wg show                                                            NetPhantom: Wed Aug 20 21:42:29 2025
interface: wg0
  public key: rWifz9/PllmhsVhvjgtw+WLwzaUmWJzUPP8RkAXWnB8=
  private key: (hidden)
  listening port: 51820

peer: Ucfzh9ZmUA5GEwZuoPPcyjyO28i53y3Ww/lPSLR1JzA=
  preshared key: (hidden)
  endpoint: 103.226.232.164:53741
  allowed ips: 10.7.0.2/32, fddd:2c4:2c4:2c4::2/128
  latest handshake: 50 seconds ago
  transfer: 4.44 MiB received, 30.34 MiB sent

# Check bandwidth usage
root@NetPhantom:~# wg show wg0 transfer
Ucfzh9ZmUA5GEwZuoPPcyjyO28i53y3Ww/lPSLR1JzA=    4862064 31981632

# System logs
root@NetPhantom:~# journalctl -u wg-quick@wg0 -f
Aug 20 20:47:49 NetPhantom wg-quick[143317]: [#] ip link delete dev wg0
Aug 20 20:47:49 NetPhantom systemd[1]: wg-quick@wg0.service: Deactivated successfully.
Aug 20 20:47:49 NetPhantom systemd[1]: Stopped wg-quick@wg0.service - WireGuard via wg-quick(8) for wg0.
Aug 20 20:49:45 NetPhantom systemd[1]: Starting wg-quick@wg0.service - WireGuard via wg-quick(8) for wg0...
Aug 20 20:49:45 NetPhantom wg-quick[144073]: [#] ip link add wg0 type wireguard
Aug 20 20:49:45 NetPhantom wg-quick[144073]: [#] wg setconf wg0 /dev/fd/63
Aug 20 20:49:45 NetPhantom wg-quick[144073]: [#] ip -4 address add 10.7.0.1/24 dev wg0
Aug 20 20:49:45 NetPhantom wg-quick[144073]: [#] ip -6 address add fddd:2c4:2c4:2c4::1/64 dev wg0
Aug 20 20:49:45 NetPhantom wg-quick[144073]: [#] ip link set mtu 1420 up dev wg0
Aug 20 20:49:45 NetPhantom systemd[1]: Finished wg-quick@wg0.service - WireGuard via wg-quick(8) for wg0.

# Check listening ports
sudo ss -tulpn | grep 51820
```
