```
                       888~~\                                           888   _                    
                       888   |  e88~~8e  888-~\  d88~\  e88~~8e  888-~\ 888 e~ ~   e88~~8e  888-~\ 
                       888 _/  d888  88b 888    C888   d888  88b 888    888d8b    d888  88b 888    
                       888  \  8888__888 888     Y88b  8888__888 888    888Y88b   8888__888 888    
                       888   | Y888    , 888      888D Y888    , 888    888 Y88b  Y888    , 888    
                       888__/   "88___/  888    \_88P   "88___/  888    888  Y88b  "88___/  888    
```

<div align="center">

**Home Lab · Proxmox VE · Self-Hosted · Security-Focused**

[![Proxmox](https://img.shields.io/badge/Proxmox-VE_8.4.1-E57000?style=flat-square&logo=proxmox)](https://proxmox.com)
[![OS](https://img.shields.io/badge/kernel-6.8.12--11--pve-00d4ff?style=flat-square&logo=linux)](https://kernel.org)
[![License](https://img.shields.io/badge/license-MIT-ff6b35?style=flat-square)](LICENSE)
[![wh1s.org](https://img.shields.io/badge/brand-wh1s.org-00d4ff?style=flat-square)](https://wh1s.org)

*Built to Learn. Built to Last. Built to Scale.*

</div>

---

## ⚔️ What is BERSERKER?

BERSERKER is a repurposed Intel Xeon server running Proxmox VE as a personal home lab. It serves three purposes simultaneously:

- **Self-Hosted Services** — Gitea, Paperless-NGX, and backups running on my own hardware, in my own network
- **Cybersecurity Lab** — Penetration testing, Red Team tooling, CTF environments
- **Open Source Platform** — Development base for DeployerTool and [HashBerry](https://github.com/hashberry)

> No open ports. No trust assumed. Backed up across three geographic locations.

---

## 🖥️ Hardware

| Component | Spec |
|-----------|------|
| **Mainboard** | Supermicro X9SRI-F |
| **CPU** | Intel Xeon E5-2690 v2 · 10C/20T · 3.0–3.8 GHz |
| **RAM** | 64 GB Samsung DDR3 ECC (expandable to 256 GB) |
| **System Storage** | 120 GB Samsung SSD |
| **Data Storage** | 2 × 3 TB WD Red · Software RAID 1 (2.6 TB usable) |
| **GPU** | NVIDIA GTX 1050 Ti · 4 GB VRAM |
| **OS** | Proxmox VE 8.4.1 · Kernel 6.8.12-11-pve |
| **Power** | ~100–150W · ~27 EUR/month |

---

## 🗂️ Service Stack

| CT ID | Service | Status |
|-------|---------|--------|
| CT 100 | Nginx Proxy Manager | ✅ Production |
| CT 101 | Proxmox Backup Server | ✅ Production |
| CT 102 | web-check-list (Web App) | ✅ v1.0 |
| CT 104 | Paperless-NGX | ✅ Production |
| CT 105 | Gitea | ✅ Production |
| CT 106 | PatchMon | ✅ Production |
| CT 107 | Homepage | ⚠️ Review pending |
| CT 108 | Grafana + Prometheus | ⏸️ Paused — rebuild planned |

---

## 🌐 Network Architecture

```
Internet
    │
    ▼
Cloudflare Zero Trust Tunnel  ◄── No open ports on router
    │
    ▼
LXC Containers (per-service)
    │
    └── *.domain.tld       → Lab & personal services
    
Admin Access: Tailscale VPN (GitHub OAuth + 2FA)
```

**Security model:** Zero Trust by default. No port forwarding. No exposed management interfaces.

---

## 💾 Backup Strategy (3-2-1)

```
Tier 1  │  RAID 1 (Live)       │  2× WD Red 3TB     │  Köln (live)
Tier 2  │  PBS → Restic/SFTP   │  Hetzner BX11       │  Falkenstein (daily)
Tier 3  │  FTP Cold Snapshot   │  All-Inkl Webspace  │  Dresden (monthly)
```

- PBS retention: `7 daily / 4 weekly / 6 monthly`
- RTO: < 2 hours for full system restore
- Host config backup: `/etc/pve/`, SSH keys, UFW rules, Tailscale identity
- All storage providers: German data centers, no US cloud involved

---

## 🔐 Security Hardening

- SSH: `PermitRootLogin no` · named user only · `MaxAuthTries 3`
- UFW: default deny incoming, allow outgoing
- Tailscale interface whitelisted; roommate network fully blocked
- Proxmox roles: `admins / vm-managers / security-lab / monitoring` (pool-isolated)
- Planned: isolated `vmbr1` network for vulnerable target VMs (no internet)

---

## 🔴 Security Lab

| Tool | Purpose |
|------|---------|
| Kali Linux VM | Primary pentesting platform |
| OWASP Juice Shop | Web vulnerability training |
| Burp Suite CE | HTTP proxy & request interception |
| TryHackMe | Structured learning paths |

**Certifications in progress:** Pre Security ✓ · AoC 2024 ✓  
**Target role:** Red Team @ REWE Digital

---

## 🚀 Roadmap

### Now
- [x] Hetzner Storage Box BX11 — Tier 2 backup active
- [ ] All-Inkl FTP cold snapshot script — activate Tier 3
- [ ] Monitoring rebuild — cAdvisor + Proxmox Exporter

### Short-term
- [ ] Vaultwarden — self-hosted password manager
- [ ] Nextcloud — iCloud/Drive replacement
- [ ] Raspberry Pi watchdog — external uptime monitoring
- [ ] GPU upgrade → GTX 1080 Ti (11 GB VRAM, 13B+ models)

### Long-term
- [ ] 3-Node Proxmox Cluster (N+1, ASRock Rack B450D4U-V1L)
- [ ] Dedicated AI GPU Node (RTX 4090/5090)
- [ ] 19" Rack infrastructure with USV + intelligent PDU
- [ ] 10 GbE internal networking

---

## 📦 Projects

| Project | Description | Status |
|---------|-------------|--------|
| DeployerTool | GUI-driven Linux deployment script generator | 🟡 Alpha |
| [HashBerry](https://github.com/hashberry) | ARM64 security distro for Apple Silicon | 🟡 Phase 1 |
| web-check-list | Spring Boot + Angular · full-stack web application | ✅ v1.0 |

---

## 💰 Running Costs

| Item | Cost |
|------|------|
| All-Inkl Premium (domains + mail + FTP) | 9.95 €/mo |
| Power (~125W average) | ~27 €/mo |
| Hetzner Storage Box BX11 | 3.20 €/mo |
| **Total** | **~40 €/mo** |

---

## 🏗️ Design Philosophy

> **Redundancy over maximum load. Understanding over copy-paste. Security by design.**

- Symmetry over complexity — clear structures, identical nodes in future cluster
- Horizontal scaling over vertical overkill — more nodes, not one superserver  
- Offline capability over cloud dependency — local infrastructure first
- Zero Trust, Least Privilege, network isolation at every layer

---

<div align="center">

**⚔️ BERSERKER** · Built on [Proxmox VE](https://proxmox.com) · Operated by [wh1s](https://wh1s.org)

`// think like a threat actor. present like a professional.`

</div>






