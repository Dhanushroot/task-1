# 🛡️ Task 1 — Cyber Security Internship  
## 🔍 Scan Your Local Network for Open Ports

**Author:** DHANUSH  
**Repository:** `Task1`  
**Internship Program:** Ministry of MSME, Govt. of India — Cyber Security Internship  
**Task Objective:** Perform basic network reconnaissance using Nmap to identify open ports and assess potential risks.

---

## 📁 Files Included

- `scan.html` — Nmap HTML output converted from XML using `xsltproc`.
- `scan.xml` — Raw XML output from the Nmap scan.
- `README.md` — This documentation file.
- `screenshots/` — Screenshots captured during scan execution and result analysis.

---

## 🧭 Task Summary

This task involved scanning the local network (`192.168.1.0/24`) using a TCP SYN scan (`-sS`) with Nmap to identify active hosts and open ports. The goal was to understand network exposure, recognize potentially vulnerable services, and document findings in a structured format suitable for GitHub submission.

The scan revealed **4 active hosts**, with one host (`192.168.1.5`) exposing a wide range of services across 23 open TCP ports. The results were saved in both XML and HTML formats for clarity and presentation.

---

## 🖥️ Commands Used (Kali Linux)

```bash
nmap -sS 192.168.1.0/24 -oX scan.xml
xsltproc scan.xml -o scan.html
```

- The first command performs a stealth TCP SYN scan and saves results in XML.
- The second command converts XML to HTML using the `xsltproc` tool for easier viewing.

---

## 📊 Scan Results — Key Findings

### 🔹 Summary
- Total IPs scanned: **256**
- Hosts up: **4**
- Hosts with open ports: **2**
- Hosts with all ports closed: **2**

### 🔹 Host Breakdown

| IP Address     | Open Ports | Notable Services |
|----------------|------------|------------------|
| `192.168.1.1`  | 135, 445   | MSRPC, Microsoft-DS |
| `192.168.1.2`  | None       | All ports closed |
| `192.168.1.4`  | None       | All ports closed |
| `192.168.1.5`  | 23 ports   | FTP, SSH, Telnet, SMTP, HTTP, RPC, NetBIOS, SMB, RMI, NFS, MySQL, PostgreSQL, VNC, X11, IRC, etc. |

> Full port list for `192.168.1.5`:  
`21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 5432, 5900, 6000, 6667, 8009, 8180`

---

## 🔐 Security Analysis

### ⚠️ Risks Identified

1. **Legacy Services Exposed**
   - Telnet, FTP, rsh/rlogin, and RPC are outdated and insecure.
   - These protocols transmit data in plaintext and are vulnerable to sniffing and exploitation.

2. **Database Ports Open**
   - MySQL (`3306`) and PostgreSQL (`5432`) are accessible over the network.
   - If misconfigured, they can leak sensitive data or allow unauthorized access.

3. **Remote Access Services**
   - VNC (`5900`), SSH (`22`), and X11 (`6000`) are open.
   - These can be exploited if weak credentials or outdated versions are used.

4. **SMB/NetBIOS Exposure**
   - Ports `139` and `445` are commonly targeted for lateral movement and ransomware attacks.

5. **Multiple Attack Surfaces**
   - The host `192.168.1.5` appears to be a lab or test machine with intentionally exposed services, ideal for vulnerability testing.

---

## 🛡️ Recommended Mitigations

- 🔒 **Disable unused services** or restrict them to localhost or internal VLANs.
- 🔥 **Configure firewalls** (e.g., `ufw`, `iptables`, Windows Firewall) to block unnecessary ports.
- 🔁 **Replace insecure protocols**: Use SSH instead of Telnet, SFTP instead of FTP.
- 🧱 **Patch and harden** all exposed services; enforce strong authentication and encryption.
- 🧩 **Segment the network**: Isolate vulnerable or experimental devices on separate subnets.
- 📈 **Enable monitoring**: Use IDS/IPS tools and log analysis to detect suspicious activity.

---

## 🧪 How to Reproduce

1. **Install Nmap**  
   - Download from [nmap.org](https://nmap.org/download.html) for your OS.
     
```bash
   sudo apt update            #Debian
   sudo apt install nmap      #Debian
```

2. **Identify your IP range**  
   - Use `ip a`, `ifconfig`, or check your router settings.

3. **Run the scan**
   ```bash
   nmap -sS 192.168.1.0/24 -oX scan.xml
   xsltproc scan.xml -o scan.html
   ```

4. **Review results**
   - Open `scan.html` in a browser for a formatted report.

---

## 📚 What I Learned

- How to perform a TCP SYN scan using Nmap and interpret results.
- How to convert scan output to HTML using `xsltproc`.
- How to identify common services and assess their security implications.
- How to document findings and mitigations in a structured format.
- Importance of network reconnaissance in vulnerability assessment.

---

## 🧠 Reflections & Next Steps

This task helped reinforce the fundamentals of port scanning and service enumeration. It also highlighted how even basic scans can reveal significant exposure in a network. Going forward, I plan to:

- Automate scans with CLI wrappers and branded output.
- Integrate service version detection (`-sV`) and OS fingerprinting (`-O`) for deeper analysis.
- Expand this into a modular recon tool with flags for scan type, output format, and alerting.

---
