
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
