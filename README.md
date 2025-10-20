# task-1
## Cyber Security Internship — Task 1: Scan Your Local Network for Open Ports

**Author:** DHANUSH  
**Repository:** `Task1`  
**Files included:**  
- `scan.html` — Nmap HTML output of the scan.  
- `scan.xml` — (optional) Nmap XML output if present.  
- `README.md` — this file.  
- `screenshots/` — (optional) screenshots taken while performing the task.  
- `notes.md` — (optional) additional notes or Wireshark analysis.

---

## Short summary

I performed a TCP SYN port scan of my local network range (`192.168.1.0/24`) using Nmap and saved the results as an HTML report (`scan.html`). The scan found 4 hosts up and multiple open services on one host.

---

## Exact command(s) used in kali linux

```bash```
 nmap -sS 192.168.1.0/24 -oX scan.xml        

 xsltproc scan.xml -o scan.html               # optional: convert XML to HTML


The command and timestamp are recorded in `scan.html`.

---

## Results — important findings (extracted from `scan.html`)

- **Scan summary:** Nmap scanned 256 IP addresses; **4 hosts up**.

- **Host: 192.168.1.1**  
  - Open TCP ports: **135 (msrpc), 445 (microsoft-ds)**.

- **Host: 192.168.1.2**  
  - All scanned ports closed.

- **Host: 192.168.1.4**  
  - All scanned ports closed.

- **Host: 192.168.1.5**  
  - Multiple open TCP services, including:  
    `21 (ftp), 22 (ssh), 23 (telnet), 25 (smtp), 53 (domain), 80 (http), 111 (rpcbind), 139 (netbios-ssn), 445 (microsoft-ds), 512, 513, 514 (rsh/rlogin/rsh), 1099 (rmiregistry), 1524 (ingreslock), 2049 (nfs), 2121 (ccproxy-ftp), 3306 (mysql), 5432 (postgresql), 5900 (vnc), 6000 (X11), 6667 (irc), 8009 (ajp13), 8180`.  
    See `scan.html` for the full verbatim output.

---

## Analysis — potential security risks

1. **Unnecessary exposed services:** Services such as FTP, Telnet, VNC, databases (MySQL/PostgreSQL), and remote shells increase the attack surface. Telnet and other cleartext protocols may leak credentials.

2. **Legacy or risky ports:** Services like rsh/rlogin (512/513/514), NFS/RPC (111/2049), and others can be exploited on misconfigured or unpatched systems.

3. **Database exposure:** Databases accessible over the network can lead to data leakage or unauthorized access if not properly secured.

4. **SMB/NetBIOS (139/445):** Exposed SMB services can be used for lateral movement or exploitation if vulnerable.

---

## Recommended mitigations

- **Disable unnecessary services** or restrict them to management VLANs or localhost.  
- **Use host and network firewalls** (ufw/iptables/windows firewall and gateway rules) to block unused ports.  
- **Replace insecure protocols** (use SSH instead of Telnet, SFTP/FTPS instead of FTP).  
- **Harden and patch** all exposed services; enforce strong authentication and TLS where supported.  
- **Network segmentation:** put untrusted devices on separate VLANs/subnets.  
- **Monitoring & logging:** enable logs and IDS/IPS to detect suspicious activity.

---

## How to reproduce

1. Install Nmap from https://nmap.org (choose the appropriate installer for your OS).  
2. Identify your local network range (e.g., `ip a`, `ifconfig`, or check your router).  
3. Run the scan using the command shown above.  
4. Open `scan.html` in a browser to review the results.

---

## What I learned

- How to run a TCP SYN (`-sS`) scan with Nmap and save results as XML/HTML.  
- How to interpret Nmap output to identify live hosts and open ports.  
- Basic mitigation techniques for exposed services.

---

