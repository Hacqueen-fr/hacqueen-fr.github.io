---
title: "Dragon Fury"
date: 2025-03-30
description: "Eloween and I face Malakar's final onslaught. Dragons rise, guided by precision and code."
tags: ["CTF", "HTB", "Coding", "Python"]
---

![Dragon Fury](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/dragon-fury.png)

> *"Precision over power. The right strike breaks the darkness." — Eloween*

---

## 🧩 Challenge Name
**Dragon Fury** — Logic / Brute-force Combination

## 📖 Storytime

At the edge of the Obsidian Cliffs, the skies darken. Malakar’s horde surges one last time, shrouded in storm and steel. The ancient dragons awaken — but their fury must be focused. Each round, a choice. One wrong move, and the chance is lost.

I stand with Eloween once more. The data is our path. Each choice matters.

---

## 📥 Challenge Format

You receive:
- A string that encodes a list of subarrays. Each subarray represents **possible damage values** for a round of dragon attacks.
- An integer `T`, the **total damage** required to defeat Malakar.

Your task:
- Select **exactly one** value from each subarray.
- The sum of selected values must equal `T`.
- It is **guaranteed** that one and only one valid combination exists.

---

## ⚙️ Python Solution (Brute-force with `itertools.product`)

```python
import ast
from itertools import product

data = input()
target = int(input())

arrays = ast.literal_eval(data)

for combo in product(*arrays):
    if sum(combo) == target:
        print(list(combo))
        break
```

---

## 🧪 Example

**Input:**

```python
[[13, 15, 27, 17], [24, 15, 28, 6, 15, 16], [7, 25, 10, 14, 11], [23, 30, 14, 10]]
77
```

**Output:**

```python
[23, 30, 14, 10]
```

✅ One choice per round  
✅ Sum = `77`  
✅ Unique valid solution

---

## 🏁 The Flag

```text
HTB{DR4G0NS_FURY_SIM_C0MB0_d80f6e8d2aaed7836d8db7dcf53c1d65}
```

The storm breaks. Malakar falls. The dragons roar in harmony — not from rage, but from victory earned through precision.

---

## 🧵 TL;DR

- One number per round, choose wisely.
- Only one combo gives the exact total.
- `itertools.product` + sum check = instant solve.
- Final flag: `HTB{DR4G0NS_FURY_SIM_C0MB0_d80f6e8d2aaed7836d8db7dcf53c1d65}`

---

🔥 Hacqueen rides the storm.
