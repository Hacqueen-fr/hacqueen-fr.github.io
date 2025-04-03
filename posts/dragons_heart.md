---
title: "Dragon's Heart"
date: 2025-03-30
description: "In the ruins of an age-old sanctum, the path to power lies in choosing wisely — for even magic has its limits."
tags: ["CTF", "HTB", "Dynamic Programming", "Non-Adjacent Sum"]
---

![Dragon's Heart](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/dragon-hearth.png)

> *"The tokens pulse with ancient energy... but choose wrong, and all is lost to the void."*

---

## 🧩 Challenge Name  
**Dragon's Heart** — Maximize Magical Energy (Non-Adjacent Sum)

## 📖 Storytime

Deep within the forgotten sanctum lies the **Dragon’s Heart**, a living artifact of unimaginable power. To awaken it, summoners must align enchanted tokens inscribed with arcane runes.

But the magic is unstable.

If two adjacent tokens are combined, their energy collapses into nothing. Only those who understand the balance of risk and strategy will unlock the Heart’s full potential.

You hold the incantation tokens. Choose wisely.

---

## 🔍 Input Format

A single line containing a Python-style list of integers representing token energies.  
Example:
```python
[3, 2, 5, 10, 7]
```

---

## 🎯 Output Format

A single integer: the **maximum energy** obtainable by summing non-adjacent tokens.

---

## 🧪 Example 1

**Input**:
```python
[3, 2, 5, 10, 7]
```

**Output**:
```
15
```

**Explanation**:  
Choose tokens `3`, `5`, and `7` → total energy = `15`

---

## 🧪 Example 2

**Input**:
```python
[10, 18, 4, 7]
```

**Output**:
```
25
```

**Explanation**:  
Choose tokens `18` and `7` → total energy = `25`

---

## 🛠️ Python Solution

```python
def max_energy(tokens):
    if not tokens:
        return 0
    elif len(tokens) == 1:
        return tokens[0]

    include = tokens[0]
    exclude = 0

    for energy in tokens[1:]:
        new_exclude = max(include, exclude)
        include = exclude + energy
        exclude = new_exclude

    return max(include, exclude)

input_str = input()
tokens = eval(input_str)
print(max_energy(tokens))
```

---

## 🏁 The Flag

When the right tokens aligned, the Heart pulsed with life.

```
HTB{SUMM0N3RS_INC4NT4T10N_R3S0LV3D_8d614a0f4f5b7186ea37dece5a972d91}
```

> *The Heart stirs... ancient power flows again.*

---

## 🧵 TL;DR

- Classic "non-adjacent sum" problem
- Use dynamic programming
- Be careful: adjacent tokens cancel each other
- Output a single integer
- Flag:
  ```
  HTB{SUMM0N3RS_INC4NT4T10N_R3S0LV3D_8d614a0f4f5b7186ea37dece5a972d91}
  ```

---

🐉🔥 Hacqueen
