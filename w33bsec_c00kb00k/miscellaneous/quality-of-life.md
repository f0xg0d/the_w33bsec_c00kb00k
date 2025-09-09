---
description: Quality of Life tools and scripts
---

# Quality of Life

### [Penelope](https://github.com/brightio/penelope)

Penelope is a powerful shell handler built as a modern netcat replacement for RCE exploitation, aiming to simplify, accelerate, and optimize post-exploitation workflows.

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
