
---
# A-MAZE-ING

---

# What is bitwise operations

Bitwise operations are operations that **directly manipulate the individual bits** (`0` or `1`) of integers. They treat numbers as sequences of bits rather than decimal values.

For example, the number 5 in binary is:

```
5 = 00000101  (8-bit representation)
```

---

## Main bitwise operators

### **AND `&`**

Keeps a bit **1 only if both bits are 1**.

```
5 & 3
5 = 0101
3 = 0011
-----------
    0001  => 1
```

## **OR `|`**

Keeps a bit **1 if either bit is 1**.

```
5 | 3
5 = 0101
3 = 0011
-----------
    0111  => 7
```

## **XOR `^`**

Keeps a bit **1 if the bits are different**, 0 if the same.

```
5 ^ 3
5 = 0101
3 = 0011
-----------
    0110  => 6
```

## **NOT `~`**

Flips all bits (0 → 1, 1 → 0).

```
~5
5 = 00000101
~5 = 11111010  (in Python, this is -6 because of two’s complement)
```

Rule in Python:

```
~x = -(x + 1)
```

## **Left shift `<<`**

Moves all bits to the left, adding zeros on the right. Each shift **multiplies by 2**.

```
5 << 1
5 = 0101
<<1 => 1010 => 10
```

## **Right shift `>>`**

Moves all bits to the right. Each shift **divides by 2** (integer division).

```
8 >> 2
8 = 1000
>>2 => 0010 => 2
```

---

## Bitwise assignment operators

Shortcut operators combine a bitwise operation with assignment:

| Operator | Meaning       |
| -------- | ----------------- |
| **&=**     | **a = a & b**   |
| **\|=**     | **a = a \| b**  |
| **^=**     | **a = a ^ b**   |
| **<<=**    | **a = a << b**  |
| **>>=**    | **a = a >> b**  |

Example:

```python
x = 5
x &= 3   ## same as x = x & 3
print(x) ## 1
```

---
## Bitwise complex example

let's use the expression:

```python
15 &= ~1 << 3
```

### Step 1: Evaluate `1 << 3`

```python
1 = 0001 (binary)
1 << 3 → 1000 (binary) = 8
```

So the expression becomes:

```python
15 &= ~8
```

### Step 2: Evaluate `~8`

Python uses **two’s complement** for `~`:

```python
~8 = -(8 + 1) = -9
```

Binary idea (simplified for 8 bits):

```text
8  = 00001000
~8 = 11110111  (which is -9 in two’s complement)
```

### Step 4: Apply `&=`

`15` in binary:

```text
15 = 00001111
```

Bitwise AND with `~8`:

```text
00001111  (15)
& 11110111  (~8)
----------
  00000111  (7)
```

So:

```python
15 &= ~1 << 3  # 15 becomes 7
```

Result:

```python
15 → 7
```

---
# Recursive Backtracker (DFS)

**Recursive Backtracker (DFS)** is a **maze generation algorithm** that explores the maze using **depth-first search** and carves passages as it goes.

The maze is viewed as:

* **cells = nodes**
* **possible moves (N, E, S, W) = edges**
* **walls = barriers that can be removed**

The algorithm works like this:

1. Start at a cell.
2. Mark the cell as **visited**.
3. Choose a **random unvisited neighbor**.
4. **Remove the wall** between the current cell and that neighbor.
5. Move to that neighbor.
6. Repeat the process.
7. If a cell has **no unvisited neighbors**, **backtrack** to the previous cell and try another direction.

The backtracking continues until **every cell has been visited**.

Because it always continues **deeper into the maze before trying other branches**, this is **depth-first search behavior**, which is why the algorithm is often simply called **DFS maze generation**.

Important properties:

* Generates a **perfect maze** (only one path between any two cells).
* Creates **long winding corridors**.
* Uses either:

  * **recursion**, or
  * an explicit **stack**.

Example exploration pattern:

```
start
  ↓
  → → → → →
          ↓
          → → dead end
          ↑
      backtrack
```

So when someone says:

```
DFS maze generator
```

they almost always mean:

```
Recursive Backtracker (DFS)
```

---

# Breadth-First Search (BFS)

**BFS** is a different exploration strategy where the maze is explored **layer by layer instead of going deep**.

In maze terms, BFS works like this:

1. Start at a cell.
2. Visit all **adjacent neighbors**.
3. Then visit **neighbors of those neighbors**.
4. Continue expanding outward from the start.

The exploration pattern looks like a **wave spreading through the maze**:

```
Layer 0: start
Layer 1: neighbors
Layer 2: neighbors of neighbors
Layer 3: ...
```

BFS uses a **queue** instead of a stack.

In maze problems, BFS is mostly used for:

* **solving a maze**
* **finding the shortest path**

because BFS guarantees the **minimum number of steps** between two cells.

It is **rarely used to generate mazes**, because it does not naturally create the long corridors that DFS-based algorithms do.

---

# Simple comparison (maze context)

| Algorithm                   | Maze role                    | Exploration style          |
| --------------------------- | ---------------------------- | -------------------------- |
| Recursive Backtracker (DFS) | Maze generation              | goes deep, then backtracks |
| BFS                         | Maze solving / shortest path | expands outward in layers  |

---
# MazeGen Python Package - README

## 1️⃣ Recommended Setup

* **Always use a virtual environment (`venv`)** for development and testing.
* This keeps the system Python clean and ensures your package installation behaves like a real pip installation.

```bash
# Creates an isolated Python environment named "venv" in the current directory.
# This prevents package conflicts with your system Python or other projects.
python -m venv venv

# Activates the venv on Linux/macOS — all pip/python commands now run inside this environment.
source venv/bin/activate

# Activates the venv on Windows — same effect, different shell syntax.
venv\Scripts\activate
````

---

## 2️⃣ Configure `pyproject.toml`

This file tells Python how to **build your package**.

Example for `mazegen`:

```toml
[project]
name = "mazegen"
version = "0.1.0"
description = "Maze generation tools"
authors = [{name = "Your Name", email = "you@example.com"}]
requires-python = ">=3.10"

[build-system]
requires = ["setuptools>=42", "wheel"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["."]
include = ["mazegen*"]
```

**Explanation:**

- `[project]` → general metadata about your package.
- `[build-system]` → specifies build tools and the backend (`setuptools.build_meta`) compatible with modern Python.
- `[tool.setuptools.packages.find]` → tells setuptools which directories contain your package modules.

---

## 3️⃣ Build the package

From your `project/` folder with venv activated:

```bash
# Ensures you have the latest versions of the core build tools,
# avoiding bugs or compatibility issues with older versions.
pip install --upgrade setuptools wheel build

# Reads pyproject.toml and produces both a .whl and a .tar.gz artifact inside dist/.
python -m build
```

- Output:

```
dist/
├─ mazegen-0.1.0-py3-none-any.whl
└─ mazegen-0.1.0.tar.gz
```

- `.whl` → faster installation
- `.tar.gz` → source distribution

---

## 4️⃣ Install the package

```bash
# Installs the pre-built wheel directly into the active venv's site-packages.
# Using the .whl is faster than the .tar.gz because it skips the build step.
pip install dist/mazegen-0.1.0-py3-none-any.whl
```

- Once installed, your package **is now part of the venv's site-packages**.
- Check with:

```bash
# Lists all packages currently installed in the active venv,
# confirming mazegen appears with the correct version.
pip list
```

Example output:

```
Package         Version
--------------- -------
build           1.4.2
mazegen         1.0.0
packaging       26.0
pip             25.2
pyproject_hooks 1.2.0
setuptools      82.0.1
wheel           0.46.3
```

---

## 5️⃣ Run your main script

```bash
# Executes the "run" target defined in your Makefile.
# Python resolves the mazegen import from site-packages, not the local source folder.
make run
```

✅ Python will **import `mazegen` from the installed wheel**, not the local source folder.

---

## 6️⃣ Why removing `mazegen/` or `.tar.gz` is safe

- Once installed via pip, Python uses the **site-packages copy of the package in the venv**.
- You can delete the `mazegen/` source folder or the `dist/` files — your scripts will continue to work.
- This mimics real-world usage where users **never have the source folder**, only the installed package.

---

## 7️⃣ Notes

- Always test **installation inside a venv** to ensure the package behaves like a pip-installed module.
- Keep the source folder (`mazegen/`) only for development.
- Update the version in `pyproject.toml` each time you rebuild to avoid pip caching issues.

---
# ANSI

**ANSI** usually refers to the **ANSI escape codes** used in terminals, but the term originally comes from the organization **American National Standards Institute**.

Let’s separate the two meanings clearly:

---

## 1) ANSI as an organization (the origin)

The American National Standards Institute defines technical standards in the US.  
One of the many standards that became widely adopted was a terminal control standard called:

**ANSI X3.64 (1979)** → terminal control sequences.

This is why terminal color/control sequences are called _ANSI codes_.

---

## 2) ANSI in programming & terminals (what people usually mean)

When developers say **“ANSI”**, they almost always mean:

👉 **ANSI escape sequences**

These are special character sequences that control terminal behavior:

- colors
    
- cursor movement
    
- screen clearing
    
- bold/underline text
    
- hiding cursor
    
- etc.
    

They start with the **ESC character** (`\033` or `\x1b`) followed by a command.

General format:

```
ESC [ parameters command
```

Example:

```
\033[31mHello\033[0m
```

---

## 3) Why they exist

Terminals are text-only devices. ANSI codes let programs **control how text appears** without graphics.

They are used everywhere:

- Linux terminals
    
- macOS terminal
    
- Windows PowerShell (modern versions)
    
- CLI tools (git, pip, npm, etc.)
    
- ASCII games and renderers (like your maze project 👀)
    

---

## 4) Common ANSI color codes

### Text colors

|Color|Code|
|---|---|
|Black|`30`|
|Red|`31`|
|Green|`32`|
|Yellow|`33`|
|Blue|`34`|
|Magenta|`35`|
|Cyan|`36`|
|White|`37`|

Example:

```
print("\033[32mSuccess!\033[0m")
```

→ prints green text.

---

### Styles

|Style|Code|
|---|---|
|Bold|`1`|
|Dim|`2`|
|Italic|`3`|
|Underline|`4`|
|Blink|`5`|
|Reverse colors|`7`|
|Reset|`0`|

Example:

```
\033[1;31mBold Red\033[0m
```

---

## 5) Cursor & screen control (super useful)

Clear screen:

```
\033[2J
```

Move cursor to top-left:

```
\033[H
```

Hide cursor:

```
\033[?25l
```

Show cursor:

```
\033[?25h
```

This is how terminal animations and games work.

---

## 6) Why you see it in your project

Your maze renderer uses ANSI to:

- color walls and paths
    
- redraw the maze
    
- animate solving
    
- apply themes
    

Without ANSI → terminal apps would be plain monochrome text.

---

If you want, I can give you a compact cheat sheet tailored for your maze renderer.