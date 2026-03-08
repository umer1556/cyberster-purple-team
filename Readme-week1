# 📄 Week 1 — Network Security & Protocol Analysis

**Intern:** Muhammed Umer Khan | CSI-B1-597  
**Submission Date:** March 8, 2026  
**Track:** Purple Team | Batch 1

---

## 🎯 Objectives

1. Build a fully isolated virtual lab using VMware Workstation (Kali Linux + Windows 11)
2. Capture live TCP traffic with Wireshark and perform a deep-dive protocol analysis

---

## ✅ Task 01 — Virtualization & Network Topology Setup

### What Was Done
- Created a VMware LAN Segment named **"Intnet"** and applied it to both VMs
- Assigned static IP **192.168.56.30/24** on Kali Linux (eth0) via Network Manager GUI
- Assigned static IP **192.168.56.40/24** on Windows 11 via Settings → Network & Internet
- Disabled Windows Defender Firewall to allow ICMP traffic
- Verified **bidirectional ping** — 0% packet loss in both directions
- Confirmed **internet isolation** — ping 8.8.8.8 returned 100% packet loss

### Key Findings
| Check | Result |
|-------|--------|
| Kali → Windows ping | ✅ 6/6 packets, 0% loss |
| Windows → Kali ping | ✅ 4/4 packets, 0% loss |
| Internet access (8.8.8.8) | ❌ 100% packet loss — isolated |

---

## ✅ Task 02 — Packet Analysis & Protocol Deep-Dive

### Methodology
```
sudo wireshark           # Started capture on eth0
nmap -p 445 192.168.56.40   # Generated TCP SYN to port 445
# Applied Wireshark filter: tcp.port == 445
```

### TCP Three-Way Handshake Captured

| Packet | Flag | Source | Destination | Meaning |
|--------|------|--------|-------------|---------|
| #35 | [SYN] | 192.168.56.30 (Kali) | 192.168.56.40 (Win11) | Connection initiation |
| #36 | [SYN, ACK] | 192.168.56.40 (Win11) | 192.168.56.30 (Kali) | Connection accepted |
| #37 | [RST] | 192.168.56.30 (Kali) | 192.168.56.40 (Win11) | Nmap reset (normal) |

### Protocol Header Analysis

**Ethernet (Layer 2)**
| Field | Value | Significance |
|-------|-------|--------------|
| Source MAC | 00:0c:29:dd:55:48 | Kali Linux VMware NIC |
| Destination MAC | 00:0c:29:ae:d6:da | Windows 11 VMware NIC |
| EtherType | 0x0800 | IPv4 payload confirmed |

**IP (Layer 3)**
| Field | Value | Significance |
|-------|-------|--------------|
| Source IP | 192.168.56.30 | Kali — initiator |
| Destination IP | 192.168.56.40 | Windows 11 — target |
| Protocol | 6 (TCP) | Transport layer |
| TTL | 53 | OS fingerprinting hint |

**TCP (Layer 4)**
| Field | Value | Significance |
|-------|-------|--------------|
| Source Port | 49692 | Ephemeral port (Kali) |
| Destination Port | 445 | Windows SMB service |
| Flags | 0x002 (SYN) | Connection initiation |
| Window Size | 1024 | Sender buffer capacity |
| MSS | 1460 | Max segment size |

---

## 🗂️ Files in This Folder

| File | Description |
|------|-------------|
| `Week1_Lab_Report.pdf` | Full lab report with all 18 screenshots embedded |
| `screenshots/` | Raw screenshot files (Figures 1–18) |

---

## 🔧 Tools Used

- **VMware Workstation** — Virtualization & network isolation
- **Kali Linux 2025.4** — Attacker node
- **Windows 11** — Target node
- **Wireshark** — Packet capture & protocol analysis
- **Nmap v7.95** — TCP port scan to generate handshake traffic
- **ip / NetworkManager** — Static IP configuration on Linux

---

*Cyberster Internship Program | Network Security & Protocol Analysis | Batch 1*
