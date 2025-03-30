---
title: "Dragon Flight"
date: 2025-03-30
description: "Eloween warns me of chaotic winds across the Floating Isles. I must guide the dragons through data, code, and storm."
tags: ["CTF", "HTB", "Algorithms", "Python", "Segment Tree"]
---

![Dragon Flight](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/hacqueen-dragonflight.png)

> *"Winds lie. The path is hidden. But in the whisper of data, truth soars." — Eloween*

---

## 🧩 Challenge Name
**Dragon Flight** — Algorithms / Segment Tree

## 📖 Storytime

In the mythical Floating Isles, ancient dragons soar between sanctuaries. But dark winds now shift the skies — unpredictably pushing, pulling, and halting the great beasts in mid-air. Eloween reaches out once more: the wind is no longer natural. Someone — or something — is distorting the currents.

My task? Become a Dragon Flight Master.

I must interpret the currents, adapt instantly, and guide these beasts to safety through changing wind segments. Every wind reading could mean life or death.

---

## 📥 Challenge Format

You receive:
- An array of wind segments, each with a **wind effect** (positive = tailwind, negative = headwind).
- A series of operations:
  - `U i x`: Update the wind effect at index `i` to `x`.
  - `Q l r`: Query for the **maximum net favorable stretch** — i.e., the **maximum contiguous subarray sum** between `l` and `r`.

The final flag comes from solving the right queries correctly.

---

## ⚙️ Python Solution (Segment Tree Power)

Here’s the full Python script I used to solve it efficiently, even with hundreds of updates and queries:

```python
class SegmentTreeNode:
    def __init__(self, left, right):
        self.left = left
        self.right = right
        self.left_child = None
        self.right_child = None
        self.sum = 0
        self.prefix_max = float('-inf')
        self.suffix_max = float('-inf')
        self.max_subarray = float('-inf')

def build_tree(arr, l, r):
    node = SegmentTreeNode(l, r)
    if l == r:
        val = arr[l]
        node.sum = val
        node.prefix_max = val
        node.suffix_max = val
        node.max_subarray = val
    else:
        mid = (l + r) // 2
        node.left_child = build_tree(arr, l, mid)
        node.right_child = build_tree(arr, mid + 1, r)
        merge(node)
    return node

def merge(node):
    left = node.left_child
    right = node.right_child
    node.sum = left.sum + right.sum
    node.prefix_max = max(left.prefix_max, left.sum + right.prefix_max)
    node.suffix_max = max(right.suffix_max, right.sum + left.suffix_max)
    node.max_subarray = max(
        left.max_subarray,
        right.max_subarray,
        left.suffix_max + right.prefix_max
    )

def update(node, index, value):
    if node.left == node.right:
        node.sum = value
        node.prefix_max = value
        node.suffix_max = value
        node.max_subarray = value
    else:
        if index <= node.left_child.right:
            update(node.left_child, index, value)
        else:
            update(node.right_child, index, value)
        merge(node)

def query(node, l, r):
    if node.left == l and node.right == r:
        return node
    if r <= node.left_child.right:
        return query(node.left_child, l, r)
    elif l >= node.right_child.left:
        return query(node.right_child, l, r)
    else:
        left_result = query(node.left_child, l, node.left_child.right)
        right_result = query(node.right_child, node.right_child.left, r)
        temp = SegmentTreeNode(0, 0)
        temp.left_child = left_result
        temp.right_child = right_result
        merge(temp)
        return temp

def main():
    N, Q = map(int, input().split())
    arr = list(map(int, input().split()))
    root = build_tree(arr, 0, N - 1)

    for _ in range(Q):
        parts = input().split()
        if parts[0] == 'U':
            i = int(parts[1]) - 1
            x = int(parts[2])
            update(root, i, x)
        elif parts[0] == 'Q':
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1
            res = query(root, l, r)
            print(res.max_subarray)

if __name__ == "__main__":
    main()
```

---

## 🧪 Test Case Walkthrough

```text
Input:
5 3
10 -2 3 -1 5
Q 1 5
U 2 4
Q 1 3

Output:
15
17
```

✅ First query returns the max sum of [10, -2, 3, -1, 5] → 15  
✅ After updating index 2 to 4 → array becomes [10, 4, 3, -1, 5]  
✅ Second query over [10, 4, 3] → max sum = 17

---

## 🏁 The Flag

```text
HTB{Dragons_Navigate_By_Data}
```

I tamed the winds not with wings, but with logic. Eloween smiles — the skies are ours again.

---

## 🧵 TL;DR

- Wind segments = array values (+ = good wind, - = bad).
- `U i x` updates a segment’s value.
- `Q l r` asks for best continuous segment (max subarray sum).
- Segment Tree handles fast updates/queries.
- Final flag: `HTB{Dragons_Navigate_By_Data}`

---

🔥 Hacqueen out.
