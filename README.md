# WireGuard VPS Setup - Ubuntu 24.04

## Overview
Dokumentasi ini berisi panduan lengkap untuk menginstall dan mengkonfigurasi WireGuard VPN server di VPS Ubuntu 24.04 menggunakan installer otomatis dari GitHub.

## Features
- ✅ Auto-installer untuk kemudahan setup
- ✅ Support multiple clients
- ✅ NAT traversal dan port forwarding otomatis
- ✅ DNS resolver configuration
- ✅ QR Code generation untuk mobile clients
- ✅ Easy client management (add/remove)
- ✅ IPv4 dan IPv6 support

## Requirements
- VPS dengan Ubuntu 24.04 LTS
- Root atau sudo access
- Port UDP (default 51820) terbuka di firewall
- Internet connection yang stabil

## Quick Start
```bash
# Download dan jalankan installer
wget https://get.vpnsetup.net/wg -O wireguard.sh
chmod +x wireguard.sh
sudo ./wireguard.sh
```

## What's Included
- `wireguard-setup.md` - Step-by-step installation guide
- Auto-generated client configurations
- QR codes untuk mobile setup
- Firewall rules configuration
- DNS resolver setup

## Default Configuration
- **Server Port**: UDP 51820
- **Server IP Range**: 10.66.66.0/24
- **DNS Servers**: 1.1.1.1, 1.0.0.1 (Cloudflare)
- **Allowed IPs**: 0.0.0.0/0 (full tunnel)

## Post-Installation
Setelah instalasi selesai, kamu akan mendapatkan:
1. Server configuration file di `/etc/wireguard/wg0.conf`
2. Client configuration files di `/root/` atau home directory
3. QR codes untuk mobile clients

## Client Setup
- **Android/iOS**: Scan QR code dengan WireGuard app
- **Windows/Linux/macOS**: Import `.conf` file ke WireGuard client

## Management Commands
```bash
# Start WireGuard
sudo systemctl start wg-quick@wg0

# Stop WireGuard  
sudo systemctl stop wg-quick@wg0

# Status check
sudo systemctl status wg-quick@wg0

# Add new client
sudo ./wireguard.sh

# View active connections
sudo wg show
```

## Troubleshooting
- Pastikan port UDP 51820 terbuka di firewall VPS
- Check routing table dengan `ip route`
- Verify IP forwarding: `cat /proc/sys/net/ipv4/ip_forward`
- Check logs: `journalctl -u wg-quick@wg0`

## Security Notes
- Simpan private keys dengan aman
- Gunakan strong passwords untuk VPS
- Regular update system dan WireGuard
- Monitor connection logs secara berkala

## Support
Jika ada masalah, check dokumentasi lengkap di `wireguard-setup.md` atau buka issue di repository GitHub.

---
**Author**: Setup Guide  
**Version**: 1.0  
**Last Updated**: August 2025
