# Helix Python Competitive Programming Setup Guide

A complete guide to setting up Helix editor for optimal Python competitive programming experience.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Template Files](#template-files)
- [Usage Guide](#usage-guide)
- [Keybindings Reference](#keybindings-reference)
- [Workflow Examples](#workflow-examples)
- [Troubleshooting](#troubleshooting)

## Prerequisites

### Required Software
- **Helix Editor** - Latest version
- **Python 3.8+** - For running solutions
- **Node.js** - For language servers
- **Git** - For cloning repositories

### Language Servers (Install these for best experience)
```bash
# Pyright (Python language server)
npm install -g pyright

# Ruff (Python linter/formatter)
pip install ruff

# Black (Python formatter)
pip install black

# Optional: Pylyzer (additional Python analysis)
pip install pylyzer
```

## Installation

### 1. Install Helix
```bash
# Via package manager (recommended)
# Ubuntu/Debian
sudo add-apt-repository ppa:maveonair/helix-editor
sudo apt update
sudo apt install helix

# Arch Linux
sudo pacman -S helix

# macOS
brew install helix

# Or via Cargo
cargo install helix-term --locked
```

### 2. Set up runtime files
```bash
# Create config directory
mkdir -p ~/.config/helix

# If installed via cargo, set up runtime
hx --grammar fetch
hx --grammar build
```

### 3. Verify installation
```bash
hx --health python
```

Should show:
- ✓ pyright-langserver
- ✓ ruff  
- ✓ black (formatter)
- ✓ Highlight queries
- ✓ Textobject queries
- ✓ Indent queries

### Optional Theme
```bash
cd /tmp

git clone https://github.com/gokayburuc/helix-themes.git
cd helix-themes

mkdir ~/.config/helix/themes
cp *.toml ~/.config/helix/themes/
```

## Configuration

### Complete Helix Configuration File

Create `~/.config/helix/config.toml` with the following content:

```toml
# ~/.config/helix/config.toml
# Optimized for Competitive Programming in Python

theme = "low-eye-strain"

[editor]
line-number = "relative"  # Easier navigation
mouse = true
middle-click-paste = true
auto-completion = true
auto-format = true
auto-save = true
idle-timeout = 250  # Fast autocompletion
completion-timeout = 250
preview-completion-insert = true
completion-replace = true
completion-trigger-len = 1
color-modes = true
cursorline = true
cursorcolumn = false
gutters = ["diagnostics", "spacer", "line-numbers", "spacer", "diff"]
auto-info = true
true-color = true
rulers = [80]  # Keep lines readable
bufferline = "multiple"
text-width = 80

[editor.auto-pairs]
'(' = ')'
'{' = '}'
'[' = ']'
'"' = '"'
"'" = "'"
"`" = "`"

[editor.statusline]
left = ["mode", "file-name", "file-modification-indicator"]
center = ["spinner"]
right = ["diagnostics", "selections", "position", "file-encoding"]
separator = "│"
mode.normal = "NORMAL"
mode.insert = "INSERT"
mode.select = "SELECT"

[editor.lsp]
enable = true
display-messages = true
auto-signature-help = true
display-inlay-hints = false
display-signature-help-docs = true
snippets = true
goto-reference-include-declaration = true

[editor.cursor-shape]
insert = "bar"
normal = "block"
select = "underline"

[editor.file-picker]
hidden = false
follow-symlinks = true
deduplicate-links = true
parents = true
ignore = true
git-ignore = true
git-global = true
git-exclude = true

[editor.search]
smart-case = true
wrap-around = true

[editor.whitespace]
render = "all"
characters = { space = "·", nbsp = "⍽", tab = "→", newline = "⏎" }

[editor.indent-guides]
render = true
character = "╎"
skip-levels = 1

# COMPETITIVE PROGRAMMING KEYBINDINGS
[keys.normal]
# Quick file operations
"C-s" = ":write"
"C-q" = ":quit"

# Fast compilation and execution
"space" = { "r" = ":sh python3 solution.py", "t" = ":sh python3 solution.py < input.txt", "c" = ":sh python3 -m py_compile solution.py", "d" = ":sh python3 -m pdb solution.py", "o" = ":sh python3 solution.py > output.txt", "b" = ":sh python3 solution.py < input.txt > output.txt", "z" = ":sh cat solution.py | clip.exe" }

# Quick navigation and editing
"C-h" = "jump_backward"
"C-l" = "jump_forward"
"g" = { "h" = "goto_line_start", "l" = "goto_line_end", "s" = "goto_first_nonwhitespace", "g" = "goto_file_start", "e" = "goto_last_line" }

# Fast selection and manipulation
"V" = "extend_line_below"
"C-a" = "select_all"
"C-d" = "select_next_sibling"

# Terminal and splits
"C-t" = ":sh"
"C-\\" = ":vsplit"
"C-_" = ":hsplit"

# Quick commenting (for debugging)
"C-/" = "toggle_comments"

# Buffer navigation
"C-n" = ":buffer-next"
"C-p" = ":buffer-previous"
"C-w" = ":buffer-close"

# Custom CP shortcuts
"F5" = ":sh python3 solution.py"  # Run current file
"F6" = ":sh python3 solution.py < input.txt"  # Run with input file
"F7" = ":sh python3 solution.py < input.txt > output.txt"  # Run with input/output files
"F8" = ":sh clear && python3 solution.py"  # Clear screen and run
"F9" = ":sh python3 -u solution.py"  # Run with unbuffered output

[keys.insert]
# Quick escape
"j" = { "k" = "normal_mode" }
"C-c" = "normal_mode"

# Line navigation
"C-a" = "goto_line_start"      # Go to beginning of line
"C-e" = "goto_line_end"        # Go to end of line (alternative)
"C-0" = "goto_line_start"      # Alternative home key
"C-$" = "goto_line_end"        # Alternative end key

# Quick deletion
"C-u" = "kill_to_line_start"   # Delete from cursor to line start
"C-k" = "kill_to_line_end"     # Delete from cursor to line end
"C-d" = "delete_char_forward"  # Delete character forward
"C-b" = "delete_char_backward" # Delete character backward

# Quick save without leaving insert mode
"C-s" = ":write"               # Save file while staying in insert

# Undo/Redo without leaving insert mode
"C-z" = "undo"                 # Undo last change
"C-y" = "redo"                 # Redo last undone change
"C-r" = "redo"                 # Alternative redo (common in many editors)

[keys.select]
# Fast selection manipulation
"C-c" = "normal_mode"
"V" = "extend_line_below"

# Language-specific settings
[[language]]
name = "python"
scope = "source.python"
injection-regex = "python"
file-types = ["py", "pyi", "py3", "pyw", ".pythonstartup", ".pythonrc"]
shebangs = ["python", "python3"]
roots = ["pyproject.toml", "setup.py", "Poetry.lock", "Pipfile", "requirements.txt"]
comment-token = "#"
language-servers = ["pyright", "ruff"]
indent = { tab-width = 4, unit = "    " }

[language.formatter]
command = "black"
args = ["--quiet", "-"]

[language-server.pyright]
command = "pyright-langserver"
args = ["--stdio"]
config = {}

[language-server.ruff]
command = "ruff"
args = ["server", "--preview"]
```

Create `~/.config/helix/languages.toml` with the following content:

```toml
# ~/.config/helix/language.toml
[[language]]
name = "python"
language-servers = ["pyright", "ruff", "pylyzer"]
auto-format = true
[language-server.pyright.config.python.analysis]
typeCheckingMode = "basic"
[language-server.ruff]
command = "ruff"
args = ["server"]
[language-server.pylyzer]
command = "pylyzer"
args = ["--server"]
[language.formatter]
command = "black"
args = ["-"]
```

Edit `~/.tmux.conf`

```conf
# Enable 24-bit truecolor support
set -g default-terminal "tmux-256color"
set-option -ga terminal-overrides ",xterm-256color:RGB"

# Fix for TERM inside tmux (for Helix, Neovim, etc.)
set -as terminal-overrides ',*:Tc'

# Improve scrolling (optional but recommended)
set -g mouse on
setw -g mode-keys vi
```

```bash
tmux kill-server
tmux
```
## Template Files

### 1. Basic Competitive Programming Template

Create `~/cp_template.py`:

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Competitive Programming Template
Fast I/O and common imports included
"""

import sys
from collections import defaultdict, deque, Counter
from heapq import heappush, heappop, heapify, nlargest, nsmallest
from bisect import bisect_left, bisect_right, insort
from math import gcd, lcm, sqrt, ceil, floor, log2, inf
from itertools import combinations, permutations, product, accumulate
from functools import lru_cache, reduce
from operator import add, mul, xor

# Fast I/O
input = sys.stdin.readline
print = sys.stdout.write

def read_int():
    return int(input())

def read_ints():
    return list(map(int, input().split()))

def read_str():
    return input().strip()

def read_strs():
    s = input().strip()
    if " " in s:
        return s.split()
    return list(s)

def solve():
    """
    Main solution function
    Implement your logic here
    """
    # Example: Read number of elements
    # n = read_int()
    
    # Example: Read array
    # arr = read_ints()
    
    # Your solution logic here
    # result = 0
    
    # Output result
    # print(f"{result}\n")

def main():
    """
    Main function to handle single/multiple test cases
    """
    # For single test case, just call solve()
    solve()
    
    # For multiple test cases, uncomment below:
    # t = read_int()
    # for _ in range(t):
    #     solve()

if __name__ == "__main__":
    main()
```

### 2. Advanced Template with Common Algorithms

Create `~/cp_advanced_template.py`:

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Advanced Competitive Programming Template
Includes common algorithms and data structures
"""

import sys
from collections import defaultdict, deque, Counter
from heapq import heappush, heappop, heapify
from bisect import bisect_left, bisect_right
from math import gcd, lcm, sqrt, ceil, floor, inf
from itertools import combinations, permutations, product
from functools import lru_cache

# Fast I/O
input = sys.stdin.readline

class FastIO:
    @staticmethod
    def read_int():
        return int(input())
    
    @staticmethod
    def read_ints():
        return list(map(int, input().split()))
    
    @staticmethod
    def read_str():
        return input().strip()
    
    @staticmethod
    def read_strs():
        return input().strip().split()

io = FastIO()

# Common algorithms and data structures
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.size = [1] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        self.size[px] += self.size[py]
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(arr, 1, 0, self.n - 1)
    
    def build(self, arr, v, tl, tr):
        if tl == tr:
            self.tree[v] = arr[tl]
        else:
            tm = (tl + tr) // 2
            self.build(arr, 2*v, tl, tm)
            self.build(arr, 2*v+1, tm+1, tr)
            self.tree[v] = self.tree[2*v] + self.tree[2*v+1]
    
    def query(self, v, tl, tr, l, r):
        if l > r:
            return 0
        if l == tl and r == tr:
            return self.tree[v]
        tm = (tl + tr) // 2
        return (self.query(2*v, tl, tm, l, min(r, tm)) +
                self.query(2*v+1, tm+1, tr, max(l, tm+1), r))
    
    def update(self, v, tl, tr, pos, new_val):
        if tl == tr:
            self.tree[v] = new_val
        else:
            tm = (tl + tr) // 2
            if pos <= tm:
                self.update(2*v, tl, tm, pos, new_val)
            else:
                self.update(2*v+1, tm+1, tr, pos, new_val)
            self.tree[v] = self.tree[2*v] + self.tree[2*v+1]

def prime_sieve(n):
    """Generate all primes up to n using Sieve of Eratosthenes"""
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    
    for i in range(2, int(sqrt(n)) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    
    return [i for i in range(2, n + 1) if is_prime[i]]

def binary_search(arr, target):
    """Binary search implementation"""
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

def gcd_extended(a, b):
    """Extended Euclidean Algorithm"""
    if a == 0:
        return b, 0, 1
    gcd_val, x1, y1 = gcd_extended(b % a, a)
    x = y1 - (b // a) * x1
    y = x1
    return gcd_val, x, y

def power_mod(base, exp, mod):
    """Fast modular exponentiation"""
    result = 1
    base = base % mod
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        exp = exp >> 1
        base = (base * base) % mod
    return result

def solve():
    """
    Main solution function
    """
    # Read input
    n = io.read_int()
    arr = io.read_ints()
    
    # Example usage of utilities
    # uf = UnionFind(n)
    # primes = prime_sieve(1000)
    # seg_tree = SegmentTree(arr)
    
    # Your solution here
    result = sum(arr)
    
    print(result)

def main():
    """Handle single or multiple test cases"""
    # Single test case
    solve()
    
    # Multiple test cases (uncomment if needed)
    # t = io.read_int()
    # for _ in range(t):
    #     solve()

if __name__ == "__main__":
    main()
```

### 3. Quick Contest Setup Script

Create `~/setup_contest.sh`:

```bash
#!/bin/bash
# Quick contest setup script

if [ -z "$1" ]; then
    echo "Usage: $0 <contest_name> [num_problems]"
    echo "Example: $0 codeforces_round_900 6"
    exit 1
fi

CONTEST_NAME=$1
NUM_PROBLEMS=${2:-6}  # Default to 6 problems (A-F)

# Create contest directory
mkdir -p "$CONTEST_NAME"
cd "$CONTEST_NAME"

# Create problem files
for i in $(seq 1 $NUM_PROBLEMS); do
    LETTER=$(printf "\\$(printf '%03o' $((64+i)))")  # Convert to A, B, C, etc.
    
    # Copy template
    cp ~/cp_template.py "${LETTER}.py"
    
    # Create input/output files
    touch "${LETTER}_input.txt"
    touch "${LETTER}_output.txt"
    
    echo "Created ${LETTER}.py, ${LETTER}_input.txt, ${LETTER}_output.txt"
done

# Create a general input.txt and output.txt
touch input.txt output.txt

echo "Contest setup complete in directory: $CONTEST_NAME"
echo "Files created for problems A through $(printf "\\$(printf '%03o' $((64+NUM_PROBLEMS)))")"
```

Make it executable:
```bash
chmod +x ~/setup_contest.sh
```

## Usage Guide

### Starting a new problem
```bash
cd ~/cp
cp ~/cp_template.py solution.py
hx solution.py
```

### Basic workflow
1. **Write solution** - Use insert mode (`i`)
2. **Test quickly** - Press `F5` to run
3. **Test with input** - Press `F6` to run with `input.txt`
4. **Debug if needed** - Press `Space + d` for debugger

### Contest setup
```bash
~/setup_contest.sh codeforces_round_900 6
cd codeforces_round_900
hx A.py
```

## Keybindings Reference

### Mode Switching
| Key | Action |
|-----|--------|
| `i` | Enter insert mode (before cursor) |
| `a` | Enter insert mode (after cursor) |
| `I` | Insert at line start |
| `A` | Insert at line end |
| `o` | New line below and insert |
| `O` | New line above and insert |
| `v` | Visual/select mode |
| `V` | Line select mode |
| `jk` | Exit insert mode (custom) |
| `Ctrl+c` | Exit insert mode (custom) |
| `Escape` | Exit to normal mode |

### Code Execution
| Key | Action |
|-----|--------|
| `F5` | Run `solution.py` |
| `F6` | Run with `input.txt` |
| `F7` | Run with input/output files |
| `F8` | Clear screen and run |
| `F9` | Run with unbuffered output |
| `Space + r` | Run solution |
| `Space + t` | Test with input |
| `Space + c` | Compile check |
| `Space + d` | Debug with pdb |
| `Space + o` | Run and save output |
| `Space + b` | Run with input and save output |

### Navigation
| Key | Action |
|-----|--------|
| `gg` | Go to file start |
| `G` | Go to file end |
| `Ctrl+h` | Jump backward |
| `Ctrl+l` | Jump forward |
| `w` | Next word |
| `b` | Previous word |
| `0` | Line start |
| `$` | Line end |
| `{` | Previous paragraph |
| `}` | Next paragraph |

### File Operations
| Key | Action |
|-----|--------|
| `Ctrl+s` | Save file |
| `Ctrl+q` | Quit |
| `Ctrl+n` | Next buffer |
| `Ctrl+p` | Previous buffer |
| `Ctrl+w` | Close buffer |

### Editing
| Key | Action |
|-----|--------|
| `dd` | Delete line |
| `yy` | Copy line |
| `p` | Paste |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `Ctrl+a` | Select all |
| `Ctrl+/` | Toggle comments |

### Terminal & Splits
| Key | Action |
|-----|--------|
| `Ctrl+t` | Open terminal |
| `\` | Vertical split |
| `-` | Horizontal split |

## Workflow Examples

### Example 1: Single Problem
```bash
# Setup
cd ~/cp
cp ~/cp_template.py solution.py
echo "5
1 2 3 4 5" > input.txt

# Edit
hx solution.py

# Test cycle
# 1. Write solution
# 2. Press F6 to test
# 3. Check output
# 4. Repeat until correct
```

### Example 2: Contest with Multiple Problems
```bash
# Setup contest directory
~/setup_contest.sh cf_round_900 6
cd cf_round_900

# Work on problem A
hx A.py
# Write solution, test with F6
# When done, switch to B.py
```

### Example 3: Debugging Session
```bash
# When solution doesn't work
hx solution.py

# Add debug prints or use debugger
# Press Space + d to run with pdb
# Or add print statements and use F5
```

## Troubleshooting

### Language Server Issues
```bash
# Check language server status
hx --health python

# Reinstall language servers
npm install -g pyright
pip install --upgrade ruff black
```

### Runtime Files Missing
```bash
# Rebuild grammar files
hx --grammar fetch
hx --grammar build

# Manual setup if needed
mkdir -p ~/.config/helix/runtime/queries/python
cd ~/.config/helix/runtime/queries/python
curl -O https://raw.githubusercontent.com/helix-editor/helix/master/runtime/queries/python/highlights.scm
curl -O https://raw.githubusercontent.com/helix-editor/helix/master/runtime/queries/python/indents.scm
curl -O https://raw.githubusercontent.com/helix-editor/helix/master/runtime/queries/python/textobjects.scm
```

### Python Execution Issues
```bash
# Make sure python3 is available
which python3

# Check if files exist
ls -la solution.py input.txt

# Test manually
python3 solution.py < input.txt
```

### Config Not Loading
```bash
# Check config syntax
hx --health

# Backup and recreate config
mv ~/.config/helix/config.toml ~/.config/helix/config.toml.backup
# Then recreate with the provided config
```

## Tips for Competitive Programming

### Speed Optimization
1. **Use standard filename** - Always name files `solution.py`, `A.py`, etc.
2. **Prepare templates** - Have common algorithms ready
3. **Learn keybindings** - Especially `jk`, `F5`, `F6`
4. **Use splits** - Keep input/output visible while coding

### Common Patterns
```python
# Fast I/O template
import sys
input = sys.stdin.readline

# Multiple test cases
t = int(input())
for _ in range(t):
    solve()

# Array input
arr = list(map(int, input().split()))

# 2D array input
matrix = []
for _ in range(n):
    matrix.append(list(map(int, input().split())))
```

### Debugging Tips
1. **Use print statements** - Add `print(f"Debug: {variable}")` 
2. **Test small cases** - Create simple `input.txt` files
3. **Use debugger** - `Space + d` for pdb when needed
4. **Check edge cases** - Empty input, single element, etc.

## Advanced Features

### Custom Snippets
You can create custom snippets for common CP patterns by modifying the Helix runtime files.

### Multiple Language Support
The config can be extended for C++ or other languages by adding language-specific sections.

### Integration with Online Judges
Consider using tools like `competitive-companion` browser extension with custom scripts that work with these keybindings.

---

**Happy Competitive Programming with Helix! 🚀**

For more advanced Helix features, check the [official documentation](https://docs.helix-editor.com/)
