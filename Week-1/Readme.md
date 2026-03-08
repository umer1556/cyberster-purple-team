# 📄 Week 1 — Network Security & Protocol Analysis

**Intern:** Muhammed Umer Khan | CSI-B1-597  
**Submission Date:** March 8, 2026  
**Track:** Purple Team | Batch 1

> ⚠️ *IP addresses are intentionally omitted from public documentation as part of OPSEC practice. Full technical details including network configuration are documented in the submitted lab report.*

---

## 🎯 Objectives

1. Build a fully isolated virtual lab using VMware Workstation (Kali Linux + Windows 11)
2. Capture live TCP traffic with Wireshark and perform a deep-dive protocol analysis

---

## ✅ Task 01 — Virtualization & Network Topology Setup

### What Was Done
- Created a VMware LAN Segment and applied it to both VMs — creating a private isolated switch with no gateway or DHCP
- Assigned static IPs manually on both Kali Linux (via Network Manager) and Windows 11 (via Settings)
- Disabled Windows Defender Firewall to allow ICMP traffic during lab verification
- Verified **bidirectional ping** between both machines — 0% packet loss in both directions
- Confirmed **complete internet isolation** — external ping returned 100% packet loss

### Connectivity Results

| Check | Result |
|-------|--------|
| Attacker VM → Target VM ping | ✅ 6/6 packets, 0% loss |
| Target VM → Attacker VM ping | ✅ 4/4 packets, 0% loss |
| External internet (public DNS) | ❌ 100% packet loss — fully isolated |

---

## ✅ Task 02 — Packet Analysis & Protocol Deep-Dive

### Methodology
```bash
sudo wireshark                  # Started capture on eth0
nmap -p 445 [TARGET-IP]         # Generated TCP SYN to port 445
# Wireshark filter: tcp.port == 445
```

### TCP Three-Way Handshake Captured

| Packet | Flag | Direction | Meaning |
|--------|------|-----------|---------|
| #35 | [SYN] | Attacker → Target | Connection initiation |
| #36 | [SYN, ACK] | Target → Attacker | Connection accepted |
| #37 | [RST] | Attacker → Target | Nmap reset (normal scan behavior) |

### Protocol Header Analysis

**Ethernet (Layer 2)**
| Field | Forensic Significance |
|-------|-----------------------|
| Source MAC | Identifies the initiating NIC — vendor traceable |
| Destination MAC | Identifies the receiving NIC |
| EtherType 0x0800 | Confirms IPv4 payload |

**IP (Layer 3)**
| Field | Value | Significance |
|-------|-------|--------------|
| Protocol | 6 (TCP) | Transport layer identifier |
| TTL | 53 | Hop count — assists OS fingerprinting |
| Source / Destination | [Attacker-IP] → [Target-IP] | Logical addressing |

**TCP (Layer 4)**
| Field | Value | Significance |
|-------|-------|--------------|
| Source Port | Ephemeral (dynamic) | Randomly assigned by OS |
| Destination Port | 445 | Windows SMB service |
| Flags | 0x002 (SYN) | Marks connection initiation |
| Window Size | 1024 | Sender receive buffer capacity |
| MSS | 1460 | Maximum segment size |

---

## 💡 Key Learnings

**Network Isolation in Practice**  
Building the LAN Segment demonstrated how organizations create controlled security labs. Without DHCP or a gateway, every IP must be manually planned — reinforcing IP address management as a foundational skill.

**TCP Handshake as Forensic Evidence**  
From just 3 packets an analyst can determine: who initiated the connection, what service was targeted, the sender's buffer capacity, an OS estimate via TTL, and exact timestamps of each exchange.

**Real-World Troubleshooting**  
Encountered and resolved two issues mid-lab: a syntax error with smbclient (switched to Nmap as alternative), and an empty Wireshark capture (timing issue — resolved by restarting capture). Both reflect real-world scenarios requiring quick pivoting.

---

## 🗂️ Files in This Folder

| File | Description |
|------|-------------|
| `Week1_Lab_Report.pdf` | Full lab report with all 18 screenshots embedded |
| `screenshots/` | Raw screenshot evidence (Figures 1–18) |

---

## 🔧 Tools Used

| Tool | Purpose |
|------|---------|
| VMware Workstation | Virtualization & network isolation |
| Kali Linux 2025.4 | Attacker node |
| Windows 11 | Target node |
| Wireshark | Packet capture & protocol analysis |
| Nmap v7.95 | TCP port scan to generate handshake traffic |
| NetworkManager / ip | Static IP configuration on Linux |

---

*Cyberster Internship Program | Network Security & Protocol Analysis | Batch 1*
