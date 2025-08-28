# Quality of Life

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

### 🌳 Tree replacement - Bash

```bash
#!/bin/bash
function list_files {
    local dir="$1"
    local indent="$2"

    # Loop through everything, including hidden files
    shopt -s nullglob dotglob  # Enable dotfiles in globbing

    for file in "$dir"/*; do
        [ -e "$file" ] || continue  # Skip weird broken symlinks

        # Get permissions
        perms=$(stat -c "%A" "$file")

        # Print formatted output
        echo "${indent}${perms} $(basename "$file")"

        # If it's a directory, recurse
        if [ -d "$file" ]; then
            list_files "$file" "$indent  "  # More indentation for cursed depth
        fi
    done

    shopt -u nullglob dotglob  # Reset globbing settings
}

# Start crawling from current directory
echo "🔍 EXPLORING EVERY DARK CORNER... EVEN YOUR HIDDEN ONES 🔍"
list_files "." ""

echo "💀 NOTHING ESCAPES ME 💀"

```

```bash
# Save to file, fix permissions and run
chmod +rwx tree.sh
./tree.sh
```
