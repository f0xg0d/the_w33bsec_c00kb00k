---
description: smb, ldap a good boy🐶 and everything in between
---

# Active Directory

## Contents:



## 🕸️ NetExec (nxc)

[NetExec Wiki](https://www.netexec.wiki/)

#### 📡 SMB Shares Enumeration

```bash
nxc smb <IP> -u 'USER' -p 'PASS' --shares
```

#### 📂 SMB Share Spidering (and download

```bash
nxc smb <IP> -u 'USER' -p 'PASS' -M spider_plus -o DOWNLOAD_FLAG=True
```

#### 📡 LDAP Active User Enumeration

```bash
nxc ldap <IP> -u 'USER' -p 'PASS' --active-users
```

## 📂 SMB
