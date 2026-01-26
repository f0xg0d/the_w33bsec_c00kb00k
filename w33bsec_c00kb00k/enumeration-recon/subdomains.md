---
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

# Subdomains

## FFuF for Subdomain Discovery

```bash
ffuf -u http://example.com/ -w <WORDLIST> -H "Host: FUZZ.example.com" -ac
```

#### **Explanation**

This FFuF command brute-forces **subdomains** by injecting words into the `Host` header.

| Option | Description                                                  |
| ------ | ------------------------------------------------------------ |
| `-u`   | Target URL (`http://example.com/`)                           |
| `-w`   | **Wordlist** for subdomains                                  |
| `-H`   | Injects **FUZZ** into the `Host` header (`FUZZ.example.com`) |
| `-ac`  | **Auto-calibration**, filters out false positives            |

## Using gobuster

gobuster vhost -u TARGET -w /usr/share/seclists/Discovery/DNS/combined\_subdomains.txt --append-domain -t 50
