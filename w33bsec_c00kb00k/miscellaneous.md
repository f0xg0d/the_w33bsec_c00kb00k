---
description: Miscellaneous useful commands, infos and torubleshooting
---

# Miscellaneous

## Quality of Life

### 🛠️ Upgrading Shells with Python

Sometimes, when you get a **reverse shell**, it's limited and doesn't support features like job control (`Ctrl + Z` / `fg`), command history, or tab completion. You can upgrade your shell using Python’s **PTY module** to make it fully interactive.

***

#### &#x20;**📌 Spawning a TTY Shell in Bash**

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

or for older Python versions:

```bash
python -c 'import pty; pty.spawn("/bin/sh")'
```

#### **📝 Explanation**

| Command                  | Description                              |
| ------------------------ | ---------------------------------------- |
| `import pty`             | Imports the PTY module (pseudo-terminal) |
| `pty.spawn("/bin/bash")` | Spawns a new **fully interactive shell** |

#### **🛠️ After Upgrading: Improve Shell Stability**

Once you have an upgraded shell, you can make it even better with:

```bash
stty raw -echo; fg
export TERM=xterm
```

🔹 **Why Use This?**

* Enables **arrow keys, tab completion, and job control**
* Prevents command execution issues in limited shells

## Troubleshooting

### 🕔 NTP sync with DC / KRB\_AP\_ERR\_SKEW(Clock skew too great)

```bash
sudo timedatectl set-local-rtc 0
sudo timedatectl set-ntp 1
sudo ntpdate -u IP -b
```

#### **📝** Explanation

This set of commands is used to **synchronize the system time** with an **NTP (Network Time Protocol) server** and ensure the system clock is managed correctly.

| Command                            | Description                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `sudo timedatectl set-local-rtc 0` | Configures the system to **use UTC** instead of local time, ensuring time consistency across applications.            |
| `sudo timedatectl set-ntp 1`       | Enables **NTP (Network Time Protocol)** to allow automatic time synchronization.                                      |
| `sudo ntpdate -u <IP> -b`          | Manually synchronizes the system time with an **NTP server at `<IP>`**, forcing an update with `-b` (immediate sync). |
