---
title: "Clockwork Guardian"
date: 2025-03-30
description: "Eloween warns me of rogue sentinels haunting the Skywatch Spire. The only way out is through the gears..."
tags: ["CTF", "HTB", "Pathfinding", "A* or Die"]
---

![Clockwork Guardian](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/hacqueen_clockwork.png)

> *"Not all machines are mindless... Some remember. Some protect. And some — some just want you gone."*

---

## 🧩 Challenge Name  
**Clockwork Guardian** — Grid Navigation, Shortest Path

## 📖 Storytime

Eloween called me in the middle of a code break. *“They’ve gone rogue,”* she whispered. *“The Sentinels. Skywatch Spire is sealed, and the only way out is to outthink the Clockwork.”*

So I went in.

Up the ancient lifts. Past the silent gears. At the top? A grid. A maze. A trap.

The guardians of the tower were once protectors, now corrupted. Anyone trying to escape is hunted by the sentinels. My mission was clear: find the path through the spire — before the watchers find me.

---

## 🧠 Understanding the Grid

The input is a **2D matrix**:

- `0`: Safe tile  
- `1`: Sentinel tile (don’t step here)  
- `'E'`: Exit  

You start at `(0, 0)` and need to reach `'E'` with the **minimum number of moves**, using only **up/down/left/right** steps.

### 🔍 Sample Grid

```python
[
  [0, 0, 1, 0, 'E'],
  [1, 0, 1, 0, 1],
  [0, 0, 0, 1, 0],
  [1, 1, 0, 0, 0]
]
```

Expected output: `6`

---

## 🧪 The Wrong Way

I started with a basic BFS, but got stuck returning `-1` way too often. Turns out `'E'` wasn't being treated right, or my script was using `return` instead of `print()` — **and HTB expects a printed int**, not a return value.

💡 Also: many challenges give the grid via `input()` as a single line (e.g. `[[...]]`). If you treat it line-by-line, **you’ll break it.**

---

## 🛠️ The Fix

Here’s the trick: parse the input using `ast.literal_eval()` and treat `'E'` as a target coordinate, not just a traversable tile.

```python
from collections import deque
import ast

def shortest_safe_path(grid):
    rows, cols = len(grid)
    end = None

    for i in range(rows):
        for j in range(cols):
            if str(grid[i][j]).upper() == 'E':
                end = (i, j)
                break
        if end:
            break

    if not end or grid[0][0] == 1:
        print(-1)
        return

    ex, ey = end
    visited = [[False]*cols for _ in range(rows)]
    directions = [(-1,0),(1,0),(0,-1),(0,1)]

    queue = deque([(0, 0, 0)])
    visited[0][0] = True

    while queue:
        x, y, steps = queue.popleft()
        if (x, y) == (ex, ey):
            print(steps)
            return
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols and not visited[nx][ny]:
                if grid[nx][ny] == 0 or (nx, ny) == (ex, ey):
                    visited[nx][ny] = True
                    queue.append((nx, ny, steps + 1))

    print(-1)

raw_input = input().strip()
import ast
grid = ast.literal_eval(raw_input)
shortest_safe_path(grid)
```

---

## 🏁 The Flag

Once the algorithm was fixed, the sentinels couldn’t stop me.

```
HTB{CL0CKW0RK_GU4RD14N_OF_SKYW4TCH_2de713ad5f3d161426f6c6fcb723aeaa}
```

> *The gears slow. The lights dim. I’m out — not because I was faster… but because I was smarter.*

---

## 🧵 TL;DR

- Grid navigation problem, avoid `1`, reach `'E'`
- Input is one big list → parse with `ast.literal_eval()`
- BFS is the move
- Output the distance with `print(...)`, not `return`
- Watch out for `'E'` as a string
- Flag:  
  `HTB{CL0CKW0RK_GU4RD14N_OF_SKYW4TCH_2de713ad5f3d161426f6c6fcb723aeaa}`

---

*Skywatch may have been corrupted… but I still own the tower.*

🕰️⚙️ Hacqueen
