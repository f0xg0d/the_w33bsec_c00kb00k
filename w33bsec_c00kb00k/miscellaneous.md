---
description: Miscellaneous useful commands and infos
---

# Miscellaneous

## 🛠️ Upgrading Shells with Python

Sometimes, when you get a **reverse shell**, it's limited and doesn't support features like job control (`Ctrl + Z` / `fg`), command history, or tab completion. You can upgrade your shell using Python’s **PTY module** to make it fully interactive.

***

### &#x20;**📌 Spawning a TTY Shell in Bash**

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

