# WireGuard VPS Setup Guide - Ubuntu 24.04
**Panduan Step-by-Step Installation & Configuration**

---

## Prerequisites Check

### 1. Sistem Requirements
```bash
# Check Ubuntu version
lsb_release -a

# Check available memory
free -h

# Check disk space
df -h

# Ensure system is up to date
sudo apt update && sudo apt upgrade -y
```

### 2. Network & Firewall Check
```bash
# Check current IP address
curl -4 icanhazip.com

# Check if port 51820 is available
sudo netstat -tulpn | grep :51820

# Install UFW if not installed
sudo apt install ufw -y

# Allow SSH (important!)
sudo ufw allow ssh
sudo ufw allow 22

# Allow WireGuard port
sudo ufw allow 51820/udp
```

---

## Installation Process

### Step 1: Download WireGuard Installer
```bash
# Method 1: Using curl
curl -O https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh

# Method 2: Using wget (alternative)
wget https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh

# Make executable
chmod +x wireguard-install.sh
```

### Step 2: Run Installation
```bash
# Run installer as root
sudo bash ./wireguard-install.sh
```

### Step 3: Configuration Prompts
Installer akan menanyakan beberapa konfigurasi:

```
1. IPv4 or IPv6 public address: [AUTO-DETECTED]
   → Press Enter untuk accept auto-detection

2. Public interface: [eth0 atau sesuai network interface]  
   → Press Enter untuk default

3. WireGuard interface name: [wg0]
   → Press Enter untuk default

4. Server WireGuard IPv4: [10.66.66.1]
   → Press Enter untuk default

5. Server WireGuard IPv6: [fd42:42:42::1]
   → Press Enter atau skip jika tidak butuh IPv6

6. Server WireGuard port: [51820]
   → Press Enter untuk default (recommended)

7. First DNS resolver: [1.1.1.1]
   → Press Enter untuk Cloudflare DNS

8. Second DNS resolver: [1.0.0.1]
   → Press Enter untuk backup Cloudflare DNS

9. Client name: [client]
   → Masukkan nama client pertama (misal: laptop-john)
```

---

## Post-Installation Configuration

### Step 4: Verify Installation
```bash
# Check WireGuard status
sudo systemctl status wg-quick@wg0

# Check interface
sudo wg show

# Check if service is enabled
sudo systemctl is-enabled wg-quick@wg0

# Enable if not enabled
sudo systemctl enable wg-quick@wg0
```

### Step 5: Firewall Configuration
```bash
# Enable UFW
sudo ufw --force enable

# Check firewall status
sudo ufw status

# Verify IP forwarding is enabled
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding=1' | sudo tee -a /etc/sysctl.conf

# Apply sysctl settings
sudo sysctl -p
```

### Step 6: Get Client Configuration
```bash
# List generated client files
ls -la ~/wg0-client-*

# Display client config (untuk copy-paste)
cat ~/wg0-client-[CLIENT_NAME].conf

# Show QR code untuk mobile (jika tersedia)
qrencode -t ansiutf8 < ~/wg0-client-[CLIENT_NAME].conf
```

---

## Client Setup Instructions

### For Mobile (Android/iOS)
1. Install WireGuard app dari Play Store/App Store
2. Tap **+** button
3. Select **Create from QR code**
4. Scan QR code yang di-generate server
5. Tap **Save** dan aktifkan connection

### For Desktop (Windows/macOS/Linux)
1. Download WireGuard client dari wireguard.com
2. Install aplikasi
3. Import file `.conf` yang sudah di-generate
4. Activate tunnel

### Manual Configuration (Advanced)
```ini
[Interface]
PrivateKey = [CLIENT_PRIVATE_KEY]
Address = 10.66.66.2/32
DNS = 1.1.1.1, 1.0.0.1

[Peer]
PublicKey = [SERVER_PUBLIC_KEY]
Endpoint = [SERVER_IP]:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

## Client Management

### Add New Client
```bash
# Run installer again
sudo bash ./wireguard-install.sh

# Select option 1: Add a new client
# Enter new client name
# Configuration akan ter-generate otomatis
```

### Remove Client
```bash
# Run installer
sudo bash ./wireguard-install.sh

# Select option 2: Remove existing client
# Pilih client yang mau di-remove
```

### List All Clients
```bash
# Check server config
sudo cat /etc/wireguard/wg0.conf

# Or check with wg command
sudo wg show wg0
```

---

## Testing & Verification

### Step 7: Test Connection
```bash
# From client, test connection
ping 10.66.66.1

# Check public IP (should show VPS IP)
curl ifconfig.me

# Test DNS resolution
nslookup google.com

# Speed test
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python -
```

### Step 8: Server Monitoring
```bash
# Real-time connection monitoring
watch sudo wg show

# Check bandwidth usage
sudo wg show wg0 transfer

# System logs
sudo journalctl -u wg-quick@wg0 -f

# Check listening ports
sudo ss -tulpn | grep 51820
```

---

## Troubleshooting

### Common Issues & Solutions

#### 1. Connection Timeout
```bash
# Check if port is blocked
sudo ufw status
telnet [SERVER_IP] 51820

# Reset UFW rules
sudo ufw --force reset
sudo ufw allow ssh
sudo ufw allow 51820/udp
sudo ufw --force enable
```

#### 2. DNS Issues
```bash
# Edit server config
sudo nano /etc/wireguard/wg0.conf

# Change DNS servers
DNS = 8.8.8.8, 8.8.4.4

# Restart service
sudo systemctl restart wg-quick@wg0
```

#### 3. IP Forwarding Problems
```bash
# Check current setting
cat /proc/sys/net/ipv4/ip_forward

# Enable temporarily
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Make permanent
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

#### 4. Restart Services
```bash
# Restart WireGuard
sudo systemctl restart wg-quick@wg0

# Reload systemctl daemon
sudo systemctl daemon-reload

# Check status
sudo systemctl status wg-quick@wg0
```

---

## Security Best Practices

### Server Security
```bash
# Change SSH port (optional)
sudo nano /etc/ssh/sshd_config
# Port 2222

# Disable password auth (use keys only)
# PasswordAuthentication no

# Restart SSH
sudo systemctl restart sshd

# Update firewall
sudo ufw allow 2222/tcp
sudo ufw delete allow ssh
```

### Regular Maintenance
```bash
# Create maintenance script
cat > ~/wireguard_maintenance.sh << 'EOF'
#!/bin/bash
echo "=== WireGuard Status ==="
sudo systemctl status wg-quick@wg0
echo ""
echo "=== Active Connections ==="
sudo wg show
echo ""
echo "=== Bandwidth Usage ==="
sudo wg show wg0 transfer
EOF

chmod +x ~/wireguard_maintenance.sh
```

### Backup Configuration
```bash
# Backup server config
sudo cp /etc/wireguard/wg0.conf ~/wg0-server-backup.conf

# Backup client configs
cp ~/wg0-client-*.conf ~/backup/

# Create backup script
cat > ~/backup_wireguard.sh << 'EOF'
#!/bin/bash
backup_dir="/root/wireguard_backup_$(date +%Y%m%d)"
mkdir -p $backup_dir
sudo cp /etc/wireguard/wg0.conf $backup_dir/
cp ~/wg0-client-*.conf $backup_dir/ 2>/dev/null
echo "Backup created in $backup_dir"
EOF

chmod +x ~/backup_wireguard.sh
```

---

## Advanced Configuration

### Custom Port Configuration
```bash
# Edit server config
sudo nano /etc/wireguard/wg0.conf

# Change ListenPort
ListenPort = 443  # Use HTTPS port to bypass some firewalls

# Update firewall
sudo ufw allow 443/udp
sudo ufw delete allow 51820/udp

# Restart service
sudo systemctl restart wg-quick@wg0
```

### Split Tunneling (Allow local access)
```ini
# In client config, change AllowedIPs
AllowedIPs = 0.0.0.0/1, 128.0.0.0/1
# This excludes local network 192.168.x.x
```

### Multiple Server Setup
```bash
# Create second interface
sudo cp /etc/wireguard/wg0.conf /etc/wireguard/wg1.conf

# Edit wg1.conf
sudo nano /etc/wireguard/wg1.conf
# Change port and IP range

# Enable second interface
sudo systemctl enable wg-quick@wg1
sudo systemctl start wg-quick@wg1
```

---

**Installation Complete!** 🎉

Your WireGuard VPN server is now ready to use. Keep this documentation for reference and regular maintenance.