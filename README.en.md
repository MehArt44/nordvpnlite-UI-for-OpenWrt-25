**English** | [Persian](README.md)


# Comprehensive Guide to NordVPN Lite UI for OpenWrt 25

[![OpenWrt](https://img.shields.io/badge/OpenWrt-25.12%2B-blue)](https://openwrt.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)
[![GitHub release](https://img.shields.io/github/v/release/MehArt44/nordvpnlite-UI)](https://github.com/MehArt44/nordvpnlite-UI/releases)

**NordVPN Lite UI** is a lightweight, fast, and fully responsive LuCI-based user interface for easy management of NordVPN on OpenWrt routers. This project is built with pure JavaScript (no dependency on heavy external libraries) and supports **OpenWrt 25.12+** with the **apk** package management system.

---

## 📋 Table of Contents

- [Quick Installation](#-installation)
- [Prerequisites](#-prerequisites)
- [Key Dashboard Features](#-key-dashboard-features)
  - [1. Connection Status and Central Dashboard](#1-connection-status-and-central-dashboard)
  - [2. Security and Privacy Section](#2-security-and-privacy-section)
  - [3. Advanced Network Settings](#3-advanced-network-settings)
  - [4. Live Log System](#4-live-log-system)
- [Architecture and Project Structure](#-architecture-and-project-structure)
- [Management Commands](#-management-commands)
- [Troubleshooting and Resolving Common Issues](#-troubleshooting-and-resolving-common-issues)
- [Complete Removal](#-complete-removal)
- [License](#-license)

---

## 🚀 Installation
For quick installation, connect to the router via SSH software (such as MobaXterm, PuTTY, or Terminal) with root access and run one of the following two commands. Run on the router via SSH with root access:


** wget:**
```bash
apk update && apk add ca-bundle wget-ssl
wget -O /tmp/nordvpnlite_0_2_0-r1_UI.sh https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/releases/download/0.2.0-r1/nordvpnlite_0_2_0-r1_UI.sh && sh /tmp/nordvpnlite_0_2_0-r1_UI.sh install
```

** curl:**
```bash
curl -fsSL https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/releases/download/0.2.0-r1/nordvpnlite_0_2_0-r1_UI.sh -o /tmp/nordvpnlite_0_2_0-r1_UI.sh && sh /tmp/nordvpnlite_0_2_0-r1_UI.sh install
```

> **⚠️ Note:** Running the script requires `root` access.

After installation, the **NordVPN Lite** menu will be added to your router's web interface. **Be sure to log out of your LuCI account and log back in**, then refresh the page with `Ctrl+Shift+R`.

---

## 📦 Prerequisites

- **Operating System:** OpenWrt 25.12 or higher (with apk support)
- **Architecture:** arm64, x86_64, or other supported architectures
- **Disk Space:** At least 5 MB of free space
- **Required packages (installed automatically):**
  - `nordvpnlite` (from the official NordVPN repository)
  - `rpcd-mod-file`
  - `wireguard-tools` and `kmod-wireguard` (for Fixed Server mode)
  - `luci-proto-wireguard`
  - `curl` and `ca-bundle`

> **Note:** Starting from OpenWrt 25.12, package management has changed from `opkg` to `apk` (Alpine Package Keeper). The installation script automatically detects your OpenWrt version and uses the appropriate package manager.

---

## 🎨 Key Dashboard Features

### 1. Connection Status and Central Dashboard

- **Real-time Monitoring:** Display of the current tunnel status (connected, connecting, disconnected, no route, or token required) with visual color codes.
- **Traffic Status:** Live monitoring of Ping (in milliseconds), and download and upload volume.
- **IP Management:** Display of the exit IP address (Endpoint), Public IP, and Baseline IP along with quick copy-to-clipboard buttons.
- **Country and Server Display:** Flag, country name, and destination host are automatically extracted from the NordVPN server and displayed.
- **Uptime Display:** Connection duration along with the age of the last Handshake.

### 2. Security and Privacy Section

| Security Tool | Functional Description |
|---|---|
| **Kill Switch** | When enabled, in the event of an unexpected VPN disconnection, internet traffic is completely blocked to prevent your identity from being exposed. |
| **IPv6 Mode** | Ability to choose between two modes: complete blocking of IPv6 across the entire router (maximum security) or blocking only forwarding from the local network to the WAN (recommended mode). |
| **Leak Protection** | Continuous monitoring of tunnel status, DNS leaks, and IP leaks. Pressing the "Privacy Check" button generates an in-depth text report of the current security and privacy status. |
| **DoH Enforcement** | Blocking of DNS-over-HTTPS traffic to prevent bypassing DNS rules. Includes two modes: Standard (blocking QUIC) and Strict (blocking TCP 443 to DoH providers). |

### 3. Advanced Network Settings

- **Authentication (Token):** A dedicated field for entering your NordVPN account token with a smart warning system if left empty. The ability to save or forget the token is built into this section.
- **Server and Country Selection:** An advanced dropdown menu with text search capability, display of country names and flags, and support for Automatic connection.
- **Communication Protocols:** Active support for the WireGuard-based protocol (named NordLynx) and consideration of the necessary architecture for future protocols (such as NordWhisper and OpenVPN).
- **DoH Blocking Modes:**
  - **Standard:** Blocking QUIC traffic (UDP 443) to prevent bypassing DNS rules.
  - **Strict:** In addition to QUIC, TCP 443 traffic to well-known DoH providers is also blocked.
- **MTU Optimization:** Ability to set MTU to `Auto` (recommended — 1420 for WireGuard) or manually set values such as 1280, 1360 up to 1500 depending on the network type (e.g., PPPoE).
- **LAN Discovery:** An option to exclude private networks (RFC1918) from the VPN tunnel so that access to devices within the local network is not interrupted.
- **Allowlist:** A field for entering subnets (CIDR) on separate lines so that their traffic is routed directly without passing through the VPN.
- **Post-quantum Encryption:** A toggle to enable new encryption standards to counter threats from quantum computers.
- **Analytics Reporting:** Ability to enable or disable sending anonymous analytics data to the server.

### 4. Live Log System

The dashboard is equipped with an internal terminal that monitors the `/var/log/nordvpnlite.log` file in real time.

- Expressions are highlighted by type (INFO, ERROR, WARN, SEC) and keywords (such as timeout or connected) with distinct colors to make troubleshooting as fast as possible.
- The system automatically scrolls to the latest logs.
- Manual refresh or clearing of logs is available through the provided buttons.

---

## 🏗 Architecture and Project Structure

This project consists of a Shell script that installs the user interface and executable files into the appropriate OpenWrt paths:

| Path | Description |
|---|---|
| `/usr/libexec/nordvpnlite-ui` | The background executable file (Helper) that establishes communication between the UI and the system. |
| `/www/luci-static/resources/view/nordvpnlite/settings.js` | The main JavaScript file of the user interface (LuCI View). |
| `/usr/share/luci/menu.d/luci-app-nordvpnlite.json` | The LuCI menu definition file. |
| `/usr/share/rpcd/acl.d/luci-app-nordvpnlite-ui.json` | The ACL file for controlling secure access permissions. |
| `/etc/nordvpnlite/` | The settings and configuration files directory. |

The user interface communicates directly with the `/usr/libexec/nordvpnlite-ui` executable to apply changes and receive data.

---

## 🛠 Management Commands

After installation, you can use the following commands for management:

```bash
# Update to the latest version
sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh upgrade

# Recover to the initial state (remove the WireGuard tunnel and return to automatic mode)
sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh recover

# Manually connect to a specific server (WireGuard)
SERVER_HOST="de1119.nordvpn.com" sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh wireguard

# Remove the manual WireGuard tunnel
sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh wireguard remove
```

### Internal Helper Commands

```bash
/usr/libexec/nordvpnlite-ui status           # Display full status
/usr/libexec/nordvpnlite-ui killswitch-on    # Enable Kill Switch
/usr/libexec/nordvpnlite-ui killswitch-off   # Disable Kill Switch
/usr/libexec/nordvpnlite-ui diagnostics      # Run full diagnostics
/usr/libexec/nordvpnlite-ui route-debug      # Route debugging
/usr/libexec/nordvpnlite-ui log              # Display live log
```

---

## 🔧 Troubleshooting and Resolving Common Issues

| Issue | Solution |
|---|---|
| **"Authentication token required" message** | Go to the settings section and enter your NordVPN token. You can obtain the token from [Nord Account](https://my.nordaccount.com/dashboard/nordvpn/access-tokens/) → Manual setup → NordLynx → Access token. |
| **Connection is established but the internet doesn't work** | There is likely an MTU issue. Go to the advanced settings section and set MTU to 1280 or 1360. |
| **Kill Switch doesn't activate** | Make sure the token is saved and the VPN tunnel is connected. Kill Switch only activates when a Handshake is established. |
| **DNS Leak detected** | Make sure the DoH mode is set to Standard or Strict. You can also use the "Privacy Check" button for a more detailed check. |
| **After updating, the UI doesn't load** | Run the `upgrade` command again. The script automatically restores the `settings.js` file. |
| **Fixed Server doesn't connect** | Use the "Diagnose" button in the Fixed server section and check the log. |


## 🗑 Complete Removal

```bash
# Remove the user interface (settings and repositories are preserved)
sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh uninstall

# Complete removal (including Kill Switch, DNS Hardening, Allowlist, and repositories)
sh /tmp/nordvpnlite.0.1.0-r1_UI_Final.sh uninstall --purge
```

> **⚠️ Note:** If Fixed Server mode is enabled, first disable it with the `recover` command.

---

## 📄 License

This project is released under the MIT License. For more information, see the [LICENSE](LICENSE) file.

---

## 🤝 Contributing

Your contributions are welcome! Please use the [Issues](https://github.com/MehArt44/nordvpnlite-UI/issues) section to report bugs or request new features.

---

## 🔗 Useful Links

- [Project GitHub Repository](https://github.com/MehArt44/nordvpnlite-UI)
- [NordVPN Lite on OpenWrt](https://openwrt.org/docs/guide-user/services/vpn/nordvpn)
- [LuCI Documentation](https://openwrt.org/docs/guide-developer/luci)
- [OpenWrt Forum](https://forum.openwrt.org/)

---

**Built with ❤️ for the OpenWrt community**

Software Environment


<img width="2154" height="1556" alt="image" src="https://github.com/user-attachments/assets/b763a67d-efe2-4325-9eae-74743cc80617" />

Source: https://downloads.nordcdn.com/nordvpnlite/
