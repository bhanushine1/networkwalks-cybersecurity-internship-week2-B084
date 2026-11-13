# networkwalks-cybersecurity-internship-week2-B084
#!/bin/bash

REPO_NAME="networkwalks-cybersecurity-internship-week2-B084"
GITHUB_USERNAME="YOUR_GITHUB_USERNAME"

# 1. Create folder structure
mkdir -p "$REPO_NAME/Module-1-Footprinting-Reconnaissance/outputs"
mkdir -p "$REPO_NAME/Module-1-Footprinting-Reconnaissance/screenshots"
mkdir -p "$REPO_NAME/Module-5-Network-Scanning-Zenmap/reports"
mkdir -p "$REPO_NAME/Module-5-Network-Scanning-Zenmap/screenshots"

cd "$REPO_NAME" || exit

# 2. Create .gitignore and .gitkeep files
cat << 'EOF' > .gitignore
.DS_Store
Thumbs.db
desktop.ini
*.swp
*.bak
*~
EOF

touch Module-1-Footprinting-Reconnaissance/outputs/.gitkeep
touch Module-1-Footprinting-Reconnaissance/screenshots/.gitkeep
touch Module-5-Network-Scanning-Zenmap/reports/.gitkeep
touch Module-5-Network-Scanning-Zenmap/screenshots/.gitkeep

# 3. Create Root README.md
cat << 'EOF' > README.md
# Networkwalks Cybersecurity & Ethical Hacking Internship — Week 2
**Intern Batch:** B084  
**Organization:** [Networkwalks Academy](https://networkwalks.com)  
**Domain:** Cybersecurity & Ethical Hacking  

---

## Overview
This repository documents the practical labs, command-line outputs, network topologies, and security analysis completed during **Week 2** of the Networkwalks Cybersecurity & Ethical Hacking Internship.

### Completed Modules
| Module | Title | Focus Area | Tools Used |
| :--- | :--- | :--- | :--- |
| **[W2-PM1](./Module-1-Footprinting-Reconnaissance/)** | Footprinting & Reconnaissance Attacks with Multiple Kali Tools | Passive & Active OSINT, Web Fingerprinting, WAF Detection, DNS Enumeration | `whois`, `whatweb`, `nslookup`, `curl`, `wafw00f`, `dnsrecon` |
| **[W2-PM5](./Module-5-Network-Scanning-Zenmap/)** | Network Scanning with Zenmap | LAN Subnet Discovery, Host Enumeration, MAC Identification, Network Topology Mapping | `Zenmap`, `Nmap`, `ipconfig` |

---

## Disclaimer
> **For Educational & Authorized Testing Purposes Only:** All reconnaissance and scanning tasks documented in this repository were conducted strictly within authorized lab environments and educational guidelines provided by Networkwalks Academy. Unauthorized scanning or exploitation of systems without explicit written permission is illegal.
EOF

# 4. Create Module 1 README.md
cat << 'EOF' > Module-1-Footprinting-Reconnaissance/README.md
# W2-PM1: Footprinting & Reconnaissance with Multiple Kali Tools
**Target:** `networkwalks.com`  
**Environment:** Kali Linux  

---

## Objective
Perform passive and active reconnaissance to build a complete technical profile of the target—covering domain ownership, hosting provider, IP resolution, web stack fingerprinting, HTTP headers, Web Application Firewall (WAF) detection, and DNS record enumeration.

---

## Summary of Tasks & Key Findings

| Task | Tool & Command | Primary Objective | Key Findings |
| :--- | :--- | :--- | :--- |
| **Task 1** | `whois networkwalks.com` | Retrieve domain registration & registrar metadata | Registrar: GoDaddy.com, LLC; Created: 2019-11-06; Expires: 2027-11-06; Name Servers: `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM` |
| **Task 2** | `whatweb networkwalks.com` | Fingerprint CMS, server software, frameworks & plugins | Web Server: Apache; CMS: WordPress 7.0.4; Plugin: WordPress Download Manager 3.3.58; JS/CSS: Bootstrap 7.0.4, jQuery 3.7.1; Contact: `info@networkwalks.com` |
| **Task 3** | `nslookup networkwalks.com` | Resolve domain name to its IPv4 address | Resolved IPv4 Address: `192.232.216.135` (via `8.8.8.8`) |
| **Task 4** | `curl -I https://networkwalks.com` | Inspect HTTP/2 response headers, cookies & endpoints | Status: `HTTP/2 200`; Server: Apache; Caching: `x-nginx-cache: WordPress`, `x-endurance-cache-level: 0`; Cookie: `__wpdm_client` (`Secure; HttpOnly`); REST API exposed at `/wp-json/` |
| **Task 5** | `wafw00f networkwalks.com` | Detect Web Application Firewall protection | Protected behind **ModSecurity (SpiderLabs)** WAF |
| **Task 6** | `dnsrecon -d networkwalks.com` | Enumerate SOA, NS, MX, A, TXT, SPF & SRV records | DNSSEC: Not configured; BIND Version: `9.16.23-RH`; Mail Server: `mail.networkwalks.com` (`192.232.216.135`); SPF record present; 8 cPanel autodiscover SRV records identified |

---

## Security & Defensive Takeaways
* **Information Leakage:** Exposing exact CMS/plugin versions (`WordPress 7.0.4`, `WP Download Manager 3.3.58`) and DNS software versions (`BIND 9.16.23-RH`) allows attackers to map targets against known CVEs.
* **WAF Protection:** The presence of **ModSecurity (SpiderLabs)** adds a defensive layer that inspects and filters malicious HTTP payloads.
* **DNS Hardening:** Enabling DNSSEC and restricting version banners on authoritative name servers reduces reconnaissance exposure.

---

## Evidence Directory
* [`outputs/`](./outputs/) — Raw terminal text outputs for Tasks 1–6
* [`screenshots/`](./screenshots/) — Kali Linux terminal screenshots for Tasks 1–6
EOF

# 5. Create Module 5 README.md
cat << 'EOF' > Module-5-Network-Scanning-Zenmap/README.md
# W2-PM5: Network Scanning with Zenmap
**Target Subnet:** Local LAN Subnet (Reference Lab: `10.0.0.0/24`)  
**Environment:** Windows Host + Zenmap (Nmap GUI)  

---

## Objective
Install Zenmap on Windows, determine the local host IPv4 configuration and subnet mask, execute a Ping Scan (`nmap -sn`) to enumerate live hosts and MAC addresses, and export the visual network topology as a PDF report.

---

## Execution Workflow

1. **Installation (Task 1):** Installed Nmap/Zenmap along with Npcap from [nmap.org](https://nmap.org/download.html).
2. **Local Network Discovery (Task 2):** Executed `ipconfig` in Command Prompt to identify the local IPv4 address and subnet mask (e.g., `10.0.0.5` / `255.255.255.0` -> `10.0.0.0/24`).
3. **Host Discovery Scan (Task 3):** Ran a **Ping Scan** in Zenmap:
   ```bash
   nmap -sn 10.0.0.0/24
