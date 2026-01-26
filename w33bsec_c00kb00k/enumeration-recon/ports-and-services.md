---
description: Nmap commands, localhost port scan and more
---

# Ports & Services

## Contents:

* [Nmap Stealth Scan](ports-and-services.md#nmap-stealth-scan-with-service-and-script-detection)
* [Nmap UDP Scan](ports-and-services.md#nmap-udp-scan-for-top-100-ports)
* [Local Port Scanning with Bash](ports-and-services.md#local-port-scanning-with-bash)

## Nmap

### &#x20;Nmap Stealth Scan with Service & Script Detection

```bash
nmap -sSVC -p- <IP> -v -oN FILE.txt
```

#### **Explanation**

This Nmap command performs a **stealth scan** with **service and script detection** while scanning **all ports**.

| Option         | Description             |
| -------------- | ----------------------- |
| `-sS`          | SYN scan (stealth scan) |
| `-sV`          | Detect service versions |
| `-sC`          | Run default NSE scripts |
| `-p-`          | Scan all 65,535 ports   |
| `<IP>`         | Target IP address       |
| `-v`           | Verbose output          |
| `-oN FILE.txt` | Save output to a file   |

### Nmap UDP Scan for Top 100 Ports

```bash
nmap -sU --top-ports 100 <IP>
```

#### **Explanation**

This Nmap command scans the **top 100 most common UDP ports**, which is useful for detecting services like DNS, SNMP, and TFTP.

| Option            | Description                            |
| ----------------- | -------------------------------------- |
| `-sU`             | Perform a **UDP scan**                 |
| `--top-ports 100` | Scan the **100 most common UDP ports** |
| `<IP>`            | Target IP address                      |

## Local Port Scanning with Bash

If you need to quickly check for **open ports** on `localhost` without using `nmap`, you can use a **simple Bash loop** to scan all **65,535 ports**.

***

```bash
for port in {1..65535}; do (echo >/dev/tcp/127.0.0.1/$port) &>/dev/null && echo "Port $port - OPEN"; done
```

#### **Explanation**

| Component                                      | Description                                    |
| ---------------------------------------------- | ---------------------------------------------- |
| `for port in {1..65535}`                       | Loops through all **ports from 1 to 65535**    |
| `(echo >/dev/tcp/127.0.0.1/$port) &>/dev/null` | **Attempts to connect** to the port silently   |
| `&& echo "Port $port - OPEN"`                  | **Prints open ports** if a connection succeeds |
