# 📁 Week 2 — Active & Passive Reconnaissance + Intrusion Detection

<p align="center">
  <img src="https://img.shields.io/badge/Week-2%20of%2012-7B2FBE?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Complete-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Track-Purple%20Team-4B0082?style=for-the-badge" />
</p>

---

## 🎯 Week Objectives

This week focused on two core cybersecurity disciplines that sit at the heart of Purple Team operations:

- **Offensive side:** Performing passive DNS intelligence gathering and active network reconnaissance using industry-standard tools
- **Defensive side:** Deploying and configuring a Network Intrusion Detection System (Snort 3) to detect reconnaissance activity

---

## 📋 Tasks Completed

### ✅ Task 01 — Active & Passive Discovery

| Sub-Task | Tool | Description |
|----------|------|-------------|
| DNS Enumeration (AAAA, MX, CNAME) | nslookup / dig | Queried DNS records for example.com using Google's public resolver (8.8.8.8) |
| WHOIS Lookup | whois.com | Gathered domain registration intelligence including registrar, dates, and nameservers |
| ICMP Host Sweep | nmap -sn | Discovered live hosts across the 192.168.56.0/24 subnet |
| ARP Scan | arp-scan | Layer 2 host discovery, bypassing host-based firewalls |
| TCP SYN Stealth Scan | nmap -sS | Identified open ports without completing the TCP handshake |
| Full Connect Scan | nmap -sT | Compared against stealth scan to understand detectability trade-offs |
| Service Version Fingerprinting | nmap -sV | Identified exact service versions and OS details on the target |
| Wireshark Packet Capture | Wireshark 4.6.0 | Captured and analysed SYN scan traffic at the packet level |

---

### ✅ Task 02 — Defensive Summary & Snort IDS

| Sub-Task | Tool | Description |
|----------|------|-------------|
| Snort 3 Installation | apt install snort | Installed Snort 3.11.1.0 on Kali Linux |
| Configuration Exploration | find / snort.lua | Located and reviewed Snort 3's Lua-based config structure |
| Custom Rule Creation | nano local.rules | Wrote ICMP detection rule with custom SID |
| ICMP Traffic Generation | ping | Generated test traffic to trigger the detection rule |
| Log Directory Verification | ls -la /var/log/snort/ | Confirmed Snort logging infrastructure is operational |

---

## 🔍 Key Findings

**Ports Discovered on Target (192.168.56.40 — Windows 11):**

| Port | Service | Version |
|------|---------|---------|
| 135/tcp | msrpc | Microsoft Windows RPC |
| 139/tcp | netbios-ssn | Microsoft Windows NetBIOS |
| 445/tcp | microsoft-ds | SMB / Windows Directory Services |

**DNS Intelligence Gathered (example.com):**
- IPv6 Addresses: `2606:4700::6812:1b78`, `2606:4700::6812:1a78` (Cloudflare CDN)
- MX Record: Root mail exchanger (IANA documentation domain)
- Nameservers: `elliott.ns.cloudflare.com`, `hera.ns.cloudflare.com`
- Registration: 1995-08-14 | Expires: 2026-08-13

**Snort Rule Written:**
```
alert icmp any any -> any any (msg:"ICMP Packet Detected"; sid:1000001; rev:1;)
```

---

## 🛠️ Lab Environment

| Component | Details |
|-----------|---------|
| Attacker | Kali Linux 2025.4 — 192.168.56.30 |
| Target | Windows 11 Build 10.0.26200.7840 — 192.168.56.40 |
| Network | VMware LAN Segment (Intnet) — fully isolated |
| Tools | nmap 7.95, arp-scan 1.10.0, Wireshark 4.6.0, Snort 3.11.1.0 |

---

## 📂 Folder Contents

```
Week2/
├── README.md                        ← This file
├── Week2_Lab_Report.pdf             ← Full lab report with analysis
└── screenshots/                     ← Evidence screenshots (Figs 1–18)
    ├── 01_nslookup_aaaa.png
    ├── 02_nslookup_mx.png
    ├── 03_nslookup_cname.png
    ├── 04_whois.png
    ├── 05_kali_ping.png
    ├── 06_nmap_sn_sweep.png
    ├── 07_arp_scan.png
    ├── 08_nmap_ss_stealth.png
    ├── 09_wireshark_capture.png
    ├── 10_wireshark_syn_filter.png
    ├── 11_nmap_st_fullconnect.png
    ├── 12_nmap_sv_versions.png
    ├── 13_snort_version.png
    ├── 14_local_rules.png
    ├── 15_snort_lua_config.png
    ├── 16_ping_icmp_traffic.png
    ├── 17_snort_log_directory.png
    └── 18_snort_execution.png
```

---

## 💡 Key Learnings

**Active vs Passive Recon:**
Passive techniques (DNS, WHOIS) leave no trace on the target. Active techniques (nmap, arp-scan) generate network traffic that can be detected. In real engagements, passive recon always comes first.

**SYN vs Full Connect Scanning:**
The SYN scan (-sS) never completes the TCP handshake, making it harder to log. The full connect scan (-sT) creates complete connection records. Modern EDR solutions detect both — neither is truly invisible.

**Snort 3 vs Snort 2:**
Snort 3 uses Lua-based configuration instead of the legacy snort.conf format. The multi-threaded architecture improves performance significantly on high-bandwidth networks.

---

*Muhammed Umer Khan | CSI-B1-597 | Purple Team | Batch 1*
