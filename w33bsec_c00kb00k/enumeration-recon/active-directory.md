---
description: smb, ldap a good boy🐶 and everything in between
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

# Active Directory

## NetExec (nxc)

[NetExec Wiki](https://www.netexec.wiki/)

#### SMB Shares Enumeration

```bash
nxc smb <IP> -u 'USER' -p 'PASS' --shares
```

#### SMB Share Spidering (and download

```bash
nxc smb <IP> -u 'USER' -p 'PASS' -M spider_plus -o DOWNLOAD_FLAG=True
```

#### LDAP Active User Enumeration

```bash
nxc ldap <IP> -u 'USER' -p 'PASS' --active-users
```

***

## SMB

### SMBClient: Interacting with SMB Shares

```bash
smbclient --no-pass -L <IP>
smbclient \\\\<IP>\\SHARE$ -N
```

#### Explanation

| Option             | Description                                                                               |
| ------------------ | ----------------------------------------------------------------------------------------- |
| `--no-pass`        | Connects **without requiring a password**, useful when **guest access** is enabled        |
| `-L <IP>`          | Lists **available SMB shares** on the target system                                       |
| `\\\\<IP>\\SHARE$` | Connects to the **SHARE$ share** (doubled `\` is required in Bash to escape `\` properly) |
| `-N`               | **No authentication prompt**, attempts **anonymous access**                               |

***

## BloodHound

#### BloodHound-python Active Directory Enumeration

```bash
bloodhound-python -u USER -p PASS -d DOMAIN.LOCAL -c all -dc DC -ns NS
```

| `-u USER` | Specifies the **username** for authentication |
| --------- | --------------------------------------------- |

| `-p PASS` | Specifies the **password** for authentication |
| --------- | --------------------------------------------- |

| `-d DOMAIN.LOCAL` | Defines the **Active Directory domain** to enumerate |
| ----------------- | ---------------------------------------------------- |

| `-c all` | Collects **all** AD information, including users, groups, sessions, ACLs, and GPOs |
| -------- | ---------------------------------------------------------------------------------- |

| `-dc DC` | Specifies the **Domain Controller (DC)** to target |
| -------- | -------------------------------------------------- |

| `-ns NS` | Defines a **custom nameserver (NS)** for DNS resolution |
| -------- | ------------------------------------------------------- |

#### **Start the Neo4j Database and Bloodhound**

```bash
neo4j console
bloodhound
```
