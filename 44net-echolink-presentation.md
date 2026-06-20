---
marp: true
theme: default
class: invert
paginate: true
header: '44Net Connect & EchoLink'
footer: 'Amateur Radio Digital Networking'
---

# 44Net Connect & WireGuard Tunnels

## Connecting Amateur Radio Networks

### Debian Linux & Windows Setup
### EchoLink Node Configuration

---

# What is 44Net?

- **AMPRNet** (Amateur Packet Radio Network)
- IP address block **44.0.0.0/8** allocated to amateur radio
- Managed by ARDC (Amateur Radio Digital Communications)
- **44Net Connect** provides easy tunnel access to the 44/8 network

---

# Why Use 44Net Connect?

- Access amateur radio services on 44/8 addresses
- Connect to EchoLink, IRLP, and other ham radio services
- Participate in the global amateur radio IP network
- No special hardware required - just an internet connection

---

# Prerequisites

- Valid amateur radio license
- Registered 44Net account at **https://portal.44net.io**
- Approved IP allocation (if running services)
- Internet connection

---

# Part 1: Debian Linux Setup

---

# Install WireGuard on Debian

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install WireGuard
sudo apt install wireguard wireguard-tools -y

# Verify installation
wg --version
```

---

# Generate WireGuard Keys

```bash
# Create config directory
sudo mkdir -p /etc/wireguard
cd /etc/wireguard

# Generate private and public keys
umask 077
wg genkey | tee privatekey | wg pubkey > publickey

# View your public key (submit to 44Net portal)
cat publickey
```

---

# 44Net Portal Configuration

1. Log in to **https://portal.44net.io**
2. Navigate to **Tunnels** section
3. Click **Add Tunnel**
4. Enter your WireGuard public key
5. Select your nearest endpoint
6. Save and download configuration

---

# Create WireGuard Config

```bash
sudo nano /etc/wireguard/44net.conf
```

```ini
[Interface]
PrivateKey = YOUR_PRIVATE_KEY_HERE
Address = 44.x.x.x/32

[Peer]
PublicKey = 44NET_ENDPOINT_PUBLIC_KEY
Endpoint = endpoint.44net.io:51820
AllowedIPs = 44.0.0.0/8
PersistentKeepalive = 25
```

---

# Start the Tunnel

```bash
# Bring up the tunnel
sudo wg-quick up 44net

# Check tunnel status
sudo wg show

# Enable at boot
sudo systemctl enable wg-quick@44net
```

---

# Verify Connectivity

```bash
# Check your 44Net IP
ip addr show 44net

# Ping a known 44Net host
ping 44.0.0.1

# Verify routing
ip route | grep 44.0
```

---

# Part 2: Windows Setup

---

# Install WireGuard on Windows

1. Download from **https://www.wireguard.com/install/**
2. Run the installer
3. Launch WireGuard application

---

# Configure WireGuard on Windows

1. Click **Add Tunnel** > **Add empty tunnel**
2. Copy your private key or generate new
3. Submit public key to 44Net portal
4. Import configuration from portal

---

# Windows WireGuard Config

```ini
[Interface]
PrivateKey = YOUR_PRIVATE_KEY_HERE
Address = 44.x.x.x/32

[Peer]
PublicKey = 44NET_ENDPOINT_PUBLIC_KEY
Endpoint = endpoint.44net.io:51820
AllowedIPs = 44.0.0.0/8
PersistentKeepalive = 25
```

---

# Activate Windows Tunnel

1. Select your 44net tunnel
2. Click **Activate**
3. Verify connection status shows "Active"
4. Test with: `ping 44.0.0.1`

---

# Part 3: EchoLink Setup

---

# What is EchoLink?

- Links amateur radio stations over the Internet
- **EchoLink-L** = Link node (simplex)
- **EchoLink-R** = Repeater node
- Requires valid callsign and verification

---

# EchoLink Node Types

| Type | Description | Use Case |
|------|-------------|----------|
| **-L** | Link Node | Simplex operation |
| **-R** | Repeater Node | Connected to repeater |
| Sysop | Full client | PC-based operation |

---

# Hardware Requirements

- Windows PC (for EchoLink software)
- Audio interface to radio
- PTT interface (VOX or hardware)
- VHF/UHF radio or repeater connection

---

# Download EchoLink

1. Visit **https://www.echolink.org**
2. Register with your callsign
3. Wait for validation (manual process)
4. Download EchoLink software

---

# EchoLink Sysop Setup

```
Setup > Sysop Settings

- Node Type: Repeater (-R) or Link (-L)
- Callsign: YOURCALL-R or YOURCALL-L
- Audio Input: Select interface
- Audio Output: Select interface
- PTT: Configure COM port or VOX
```

---

# Audio Interface Wiring

```
Radio TX Audio  ------>  PC Line In / Mic In
Radio RX Audio  <------  PC Line Out / Speaker
Radio PTT       <------  COM Port RTS/DTR (or VOX)
Ground          <------>  Common Ground
```

---

# Configure for 44Net

EchoLink can use 44Net addresses for:
- Lower latency connections
- Direct amateur-to-amateur networking
- Avoiding NAT/firewall issues

Set your EchoLink binding to your 44Net IP.

---

# EchoLink Network Settings

```
Setup > Preferences > Servers

- Binding Address: 44.x.x.x (your 44Net IP)
- Enable UDP ports: 5198, 5199
- TCP port: 5200
```

---

# Windows Firewall Rules

```powershell
# Allow EchoLink through firewall
netsh advfirewall firewall add rule name="EchoLink UDP" `
    dir=in action=allow protocol=UDP localport=5198-5199

netsh advfirewall firewall add rule name="EchoLink TCP" `
    dir=in action=allow protocol=TCP localport=5200
```

---

# Testing Your Node

1. Start EchoLink in Sysop mode
2. Verify connection to servers
3. Check audio levels with test tones
4. Connect to *ECHOTEST* conference
5. Make a test transmission

---

# Repeater Integration (-R Node)

```
Repeater Controller
        |
        v
 [Audio Interface] <---> [Windows PC]
        |                      |
        v                      v
   VHF/UHF Radio         EchoLink-R
```

---

# Best Practices

- Use a dedicated PC or VM for EchoLink
- Set up auto-start for WireGuard and EchoLink
- Monitor audio levels regularly
- Configure timeout timers
- Keep software updated

---

# Troubleshooting

| Issue | Solution |
|-------|----------|
| No tunnel connection | Check firewall, verify keys |
| EchoLink won't validate | Email support with callsign |
| Audio issues | Check levels, sample rates |
| PTT stuck | Verify COM port settings |

---

# Useful Resources

- **44Net Portal:** https://portal.44net.io
- **WireGuard Docs:** https://www.wireguard.com
- **EchoLink:** https://www.echolink.org
- **ARDC:** https://www.ardc.net

---

# Summary

1. Register for 44Net and get IP allocation
2. Install WireGuard on Debian or Windows
3. Configure tunnel with portal credentials
4. Install and configure EchoLink
5. Connect your repeater or simplex radio
6. Join the global amateur radio network!

---

# Questions?

## 73 de Your Callsign

*Happy networking on 44Net and EchoLink!*
