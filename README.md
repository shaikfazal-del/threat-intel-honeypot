<div align="center">

# 🍯 T-Pot Honeypot on Microsoft Azure

**A complete deployment of the T-Pot All-In-One Multi Honeypot Platform on an Azure Ubuntu VM — built from scratch using the free tier.**

[![T-Pot](https://img.shields.io/badge/T--Pot-Honeypot%20Platform-orange?style=for-the-badge&logo=docker)](https://github.com/telekom-security/tpotce)
[![Azure](https://img.shields.io/badge/Microsoft%20Azure-Cloud%20Host-0078D4?style=for-the-badge&logo=microsoftazure)](https://azure.microsoft.com)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-E95420?style=for-the-badge&logo=ubuntu)](https://ubuntu.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()

---

*Watch the internet try to break in — in real time.*

[📖 Full Documentation](#-full-documentation) · [🚀 Quick Start](#-quick-start) · [📸 Screenshots](#-screenshots) · [🛠 Troubleshooting](#-troubleshooting)

</div>

---

## 📌 Table of Contents

- [About the Project](#-about-the-project)
- [What is T-Pot?](#-what-is-t-pot)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Step-by-Step Guide](#-step-by-step-guide)
  - [Phase 1 — Azure Account & VM Creation](#phase-1--azure-account--vm-creation)
  - [Phase 2 — Network Security Configuration](#phase-2--network-security-configuration)
  - [Phase 3 — SSH Connection & System Update](#phase-3--ssh-connection--system-update)
  - [Phase 4 — T-Pot Installation](#phase-4--t-pot-installation)
  - [Phase 5 — Accessing the Dashboard](#phase-5--accessing-the-dashboard)
- [Key Ports Reference](#-key-ports-reference)
- [Screenshots](#-screenshots)
- [Full Documentation](#-full-documentation)
- [Troubleshooting](#-troubleshooting)
- [Resources](#-resources)
- [Disclaimer](#-disclaimer)

---

## 🎯 About the Project

This project deploys **T-Pot** — one of the most comprehensive honeypot platforms available — on a **Microsoft Azure Virtual Machine** running Ubuntu 22.04 LTS, using the **Azure free tier** ($200 in credits, no upfront cost).

A honeypot is a decoy system designed to attract attackers. Instead of defending against attacks, a honeypot *invites* them — recording every connection attempt, credential spray, exploit payload, and malware drop for analysis.

**Within minutes of going live, automated scanners from across the world will find your VM and start attacking it.** T-Pot captures all of it and presents the data in beautiful Kibana dashboards.

### What this project demonstrates

- Cloud infrastructure provisioning on Azure (VM, NSG, disk, networking)
- Linux system administration on Ubuntu
- Docker orchestration of 20+ containerized services
- Real-world threat intelligence collection and visualization
- ELK stack (Elasticsearch, Logstash, Kibana) for security analytics

---

## 🕵️ What is T-Pot?

[T-Pot](https://github.com/telekom-security/tpotce) is an open-source, all-in-one honeypot platform developed by **Deutsche Telekom Security**. It bundles over 20 honeypots in a single Docker Compose deployment, each mimicking a different internet-facing service.

### Included Honeypots

| Honeypot | Simulates |
|---|---|
| **Cowrie** | SSH & Telnet — records full attacker sessions |
| **Dionaea** | Multiple protocols — captures malware samples |
| **Honeytrap** | Unknown/custom services being probed |
| **Elasticpot** | Exposed Elasticsearch instance |
| **RDPy** | Windows Remote Desktop Protocol |
| **Mailoney** | SMTP server — catches spam campaigns |
| **Glutton** | Catch-all for unrecognized probes |
| **ADBHoney** | Android Debug Bridge |
| **CitrixHoneypot** | Citrix Gateway login pages |
| **Log4Pot** | Log4Shell (CVE-2021-44228) exploit attempts |
| + 10 more | Various protocols and services |

### Built-in Tools

| Tool | Purpose |
|---|---|
| **Kibana** | Primary dashboard — attack heatmaps, timelines, geolocation |
| **Attack Map** | Real-time animated world map of incoming attacks |
| **Cyberchef** | Data encoding, decoding, and analysis |
| **Elasticvue** | Elasticsearch cluster browser |
| **Spiderfoot** | OSINT and threat intelligence |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────┐
│              INTERNET (Global Attackers)                 │
│         Bots · Scanners · Exploit Frameworks            │
└──────────────────────────┬──────────────────────────────┘
                           │ All ports / All protocols
                           ▼
┌─────────────────────────────────────────────────────────┐
│        Azure Network Security Group (NSG)               │
│        Inbound Rule: Allow * → * (ports 0–65535)        │
└──────────────────────────┬──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│          Azure Virtual Machine                          │
│          Ubuntu 22.04 LTS · 256 GB OS Disk             │
│          Public Static IP Address                       │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Docker Engine                       │   │
│  │                                                  │   │
│  │  Cowrie │ Dionaea │ Honeytrap │ Elasticpot       │   │
│  │  RDPy   │ Mailoney│ Glutton   │ Log4Pot  │ ...   │   │
│  │                                                  │   │
│  │         ↓ Logstash (log aggregation)             │   │
│  │                                                  │   │
│  │  Elasticsearch ←→ Kibana  (port 64297 HTTPS)     │   │
│  │  Attack Map · Cyberchef · Spiderfoot             │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼ (Admin access only)
              Browser → https://<public-ip>:64297
              SSH    → ssh -p 64295 user@<public-ip>
```

---

## ✅ Prerequisites

| Requirement | Details |
|---|---|
| Microsoft Account | For Azure portal access |
| Credit/Debit Card | Azure identity verification — **not charged** on free tier |
| SSH Client | Windows Terminal, PowerShell, or PuTTY |
| Internet Connection | Stable broadband recommended |
| Basic Linux | Familiarity with terminal commands |

---

## 🚀 Quick Start

```bash
# 1. SSH into your Azure VM
ssh azureuser@<your-public-ip>

# 2. Update the system
sudo apt update && sudo apt upgrade -y

# 3. Clone T-Pot
git clone https://github.com/telekom-security/tpotce
cd tpotce

# 4. Run the installer (as non-root user)
./install.sh

# 5. Reboot
sudo reboot

# 6. Access the dashboard
# Open in browser: https://<your-public-ip>:64297

# 7. SSH after reboot uses the new port
ssh -p 64295 azureuser@<your-public-ip>
```

> ⚠️ **Note:** The full guide below is strongly recommended before running these commands. The Azure portal steps (VM creation, NSG configuration) must be completed first.

---

## 📋 Step-by-Step Guide

### Phase 1 — Azure Account & VM Creation

#### 1.1 — Create Your Free Azure Account

1. Go to [azure.microsoft.com/free](https://azure.microsoft.com/free)
2. Sign in with your Microsoft account
3. Complete phone and card verification
4. Receive **$200 in free credits** (valid 30 days)
5. Access the portal at [portal.azure.com](https://portal.azure.com)

#### 1.2 — Create the Virtual Machine

Navigate to: **Create a resource → Virtual Machine → Create → Azure virtual machine**

**Basics tab:**

| Setting | Recommended Value |
|---|---|
| Subscription | Free Trial |
| Resource Group | Create new → `HoneypotRG` |
| VM Name | `tpot-vm` |
| Region | Your nearest region |
| Availability | No infrastructure redundancy required |
| Image | `Ubuntu Server 22.04 LTS - x64 Gen2` |
| VM Size | `Standard_B2ms` (2 vCPUs, 8 GB RAM) |
| Authentication type | **Password** |
| Username | `azureuser` (or your choice) |
| Password | Strong password — **save this** |

**Disks tab:**

| Setting | Value |
|---|---|
| OS disk size | **256 GiB** (click "Change size") |
| OS disk type | Standard SSD or Premium SSD |

All other tabs (Networking, Management, Monitoring, Advanced) — leave at **default**.

Click **Review + Create → Create**.

Deployment takes approximately **2–5 minutes**. Click **Go to resource** when complete.

---

### Phase 2 — Network Security Configuration

Navigate to: **VM → Networking → Network security group → Inbound security rules → Add**

Create this rule:

| Field | Value |
|---|---|
| Source | Any |
| Source port ranges | `*` |
| Destination | Any |
| Service | Custom |
| Destination port ranges | `*` |
| Protocol | Any |
| Action | **Allow** |
| Priority | `100` |
| Name | `Allow-All-Inbound` |

Click **Add**.

> ⚠️ This intentionally opens all 65,535 ports. This is correct for a honeypot. Never do this on a production system.

---

### Phase 3 — SSH Connection & System Update

#### 3.1 — Get Your Public IP

On the VM Overview page, copy the **Public IP address** (e.g., `20.x.x.x`).

#### 3.2 — Connect via SSH

```bash
ssh azureuser@<your-public-ip>
# Type 'yes' to accept the host key, then enter your password
```

#### 3.3 — Update the System

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

Wait 3–10 minutes for completion.

---

### Phase 4 — T-Pot Installation

#### 4.1 — Clone the Repository

```bash
git clone https://github.com/telekom-security/tpotce
cd tpotce
```

#### 4.2 — Run the Installer

```bash
./install.sh
```

**The installer will automatically:**

- ✅ Change SSH port from `22` → `64295`
- ✅ Install Docker Engine and Docker Compose
- ✅ Disable systemd DNS stub listener (port 53 conflict prevention)
- ✅ Set firewall to accept mode
- ✅ Add user to `docker` group
- ✅ Install recommended packages (`grc`, `exa`, `micro`)
- ✅ Set up aliases: `dps`, `dpsw`, `ll`, `la`, `mi`
- ✅ Enable `tpot.service` in systemd (auto-start on boot)

**During installation, you will be prompted to:**

1. Enter your `sudo` password
2. **Choose T-Pot edition** → Select `Standard` for full honeypot + dashboard experience
3. **Create a web UI username and password** → Used to log into Kibana

Installation takes **10–20 minutes**.

#### 4.3 — Reboot

```bash
sudo reboot
```

Wait **2–3 minutes** for the VM to restart and all Docker containers to initialize.

---

### Phase 5 — Accessing the Dashboard

#### 5.1 — Open the Web Interface

```
https://<your-public-ip>:64297
```

- Accept the self-signed SSL certificate warning in your browser
- Enter the **T-Pot username and password** you created during installation

#### 5.2 — Reconnect via SSH (new port)

```bash
ssh -p 64295 azureuser@<your-public-ip>
```

#### 5.3 — Explore the Dashboards

| Dashboard | URL Path | What You'll See |
|---|---|---|
| Kibana | `/kibana` | Attack stats, geolocation, credential lists |
| Attack Map | `/map` | Real-time animated world attack map |
| Cyberchef | `/cyberchef` | Data analysis and encoding tools |
| Elasticvue | `/elasticvue` | Raw Elasticsearch data browser |
| Spiderfoot | `/spiderfoot` | OSINT and threat intelligence |

---

## 🔌 Key Ports Reference

| Port | Protocol | Purpose |
|---|---|---|
| `64295` | TCP | SSH access (changed from default 22) |
| `64297` | TCP (HTTPS) | T-Pot Web UI / Kibana |
| `22` | TCP | Cowrie SSH honeypot (attracts attackers) |
| `23` | TCP | Cowrie Telnet honeypot |
| `80` / `443` | TCP | Web honeypots |
| `0–65535` | All | Open for honeypot traffic capture |

---

## 📸 Screenshots

> *Images to be added — see the [full documentation](TPot_Honeypot_Azure_Documentation.pdf) for annotated step-by-step screenshots.*

| Step | Description |
|---|---|
| Azure Free Account | Sign-up page and portal dashboard |
| VM Creation | Resource group and configuration |
| Disk Settings | 256 GiB OS disk selection |
| NSG Rule | Inbound allow-all rule configuration |
| SSH Connection | Windows Terminal connected to VM |
| T-Pot Installer | Edition selection and setup progress |
| Web UI Login | HTTPS login dialog |
| Kibana Dashboard | Attack statistics and heatmaps |
| Attack Map | Real-time world attack visualization |

---

## 📖 Full Documentation

A complete step-by-step technical document with annotated screenshots, configuration tables, and troubleshooting reference is available:

📄 **[TPot_Honeypot_Azure_Documentation.pdf](TPot_Honeypot_Azure_Documentation.pdf)**

The documentation covers every phase in detail — from Azure account creation through to the Kibana dashboard — with image placeholders for each step.

---

## 🛠 Troubleshooting

| Problem | Solution |
|---|---|
| **Cannot SSH after reboot** | Use new port: `ssh -p 64295 user@<ip>`. T-Pot moves SSH from 22 to 64295. |
| **Web UI not loading** | Wait 3–5 minutes post-reboot. All Docker containers need time to initialize. |
| **SSL certificate warning** | Expected — T-Pot uses a self-signed cert. Click **Advanced → Proceed**. |
| **Installation fails midway** | Check disk space: `df -h`. Ensure 50+ GB free. Re-run `./install.sh`. |
| **Docker containers not starting** | Check service: `sudo systemctl status tpot` and `sudo journalctl -u tpot` |
| **`git clone` fails** | Install git first: `sudo apt install git -y` |
| **Azure credits running low** | Stop the VM from the Azure portal when not actively monitoring. |
| **Port conflicts during install** | The installer reports conflicting ports. Stop conflicting services manually. |

---

## 📚 Resources

- **T-Pot GitHub** — [github.com/telekom-security/tpotce](https://github.com/telekom-security/tpotce)
- **T-Pot Documentation** — [T-Pot Official Docs](https://github.com/telekom-security/tpotce#documentation)
- **Azure Free Account** — [azure.microsoft.com/free](https://azure.microsoft.com/free)
- **Azure VM Documentation** — [docs.microsoft.com/azure/virtual-machines](https://docs.microsoft.com/azure/virtual-machines)
- **Docker Documentation** — [docs.docker.com](https://docs.docker.com)
- **Kibana Documentation** — [elastic.co/guide/kibana](https://www.elastic.co/guide/en/kibana/current/index.html)
- **Medium Blog Post** — *https://fazal-sec.medium.com/i-built-a-honeypot-on-azure-and-watched-the-internet-try-to-break-in-heres-exactly-how-184de1f12a3c*

---

## ⚠️ Disclaimer

This project is built **strictly for educational and research purposes**.

- The honeypot VM intentionally has all ports open — **do not run production workloads on this VM**
- All data captured belongs to the operator and should be handled responsibly
- Do not use the techniques or tools demonstrated here for unauthorized access to any systems
- Comply with your local laws regarding network monitoring and data collection
- When done, **stop or delete the Azure VM** to avoid unexpected charges

---

## 🤝 Contributing

Found an issue or want to improve the documentation? Pull requests are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b improve/docs`
3. Commit your changes: `git commit -m 'Improve installation guide'`
4. Push the branch: `git push origin improve/docs`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

T-Pot itself is maintained by Deutsche Telekom Security and has its own license — see [tpotce/LICENSE](https://github.com/telekom-security/tpotce/blob/master/LICENSE).

---

<div align="center">

Built with curiosity, coffee, and a healthy paranoia about open ports.

**⭐ Star this repo if it helped you get started with honeypots!**

</div>
