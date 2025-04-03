---
title: "Enchanted Cipher"
date: 2025-03-30
description: "A corrupted Codex, shifting ciphers, and a witch unraveling encrypted history."
tags: ["CTF", "HTB", "Coding"]
---

![Enchanted Cipher](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/enchanted-cipher.png)

> *"Truth hides in the twist of symbols — but magic remembers." — Eloween*

---

## 🧩 Challenge Name
**Enchanted Cipher** — Cryptography / Caesar Cipher Variant

## 📖 Storytime

The Grand Arcane Codex has been tainted. Ancient records are no longer readable — every page is scrambled by enchanted shifting ciphers. Eloween, the magical codekeeper, has tasked me with restoring the lost messages.

Each message is encoded in groups of 5 letters. For every group, a unique Caesar shift (between 1 and 25) is randomly chosen. Non-alphabetic characters are ignored during encryption but must be preserved in the final output.

Time to decipher history — one enchanted scroll at a time.

---

## 🧾 Challenge Format

Input:
- Line 1: Encrypted message (may include spaces/punctuation)
- Line 2: Number of shift groups
- Line 3: List of shift values (length = number of groups)

Goal:
- Decode the message using the inverse Caesar shifts, group by group (5 letters each), and reconstruct it with original formatting.

---

## ⚙️ Python Solution

```python
import ast

def decrypt_enchanted_cipher():
    ciphertext = input().strip()
    num_groups = int(input().strip())
    shifts = ast.literal_eval(input().strip())

    # Extract letters and store non-alphabet positions
    letters = []
    non_alpha = {i: c for i, c in enumerate(ciphertext) if not c.isalpha()}
    for c in ciphertext:
        if c.isalpha():
            letters.append(c)

    decrypted_letters = []
    index = 0
    for group in range(num_groups):
        shift = shifts[group]
        for _ in range(5):
            if index >= len(letters):
                break
            c = letters[index]
            base = ord('a') if c.islower() else ord('A')
            new_char = chr((ord(c) - base - shift) % 26 + base)
            decrypted_letters.append(new_char)
            index += 1

    # Reinsert non-alphabetic characters
    result = []
    letter_index = 0
    for i in range(len(ciphertext)):
        if i in non_alpha:
            result.append(non_alpha[i])
        else:
            result.append(decrypted_letters[letter_index])
            letter_index += 1

    print(''.join(result))

if __name__ == "__main__":
    decrypt_enchanted_cipher()
```

---

## 🧪 Sample Input

```text
eqjjxerk rmbp vupi ysfdeqeu mdxdkeq
7
[1, 8, 22, 24, 9, 19, 2]
```

🧠 Sample Output:
```text
dpiiwwjc jeft zytk auhfvhvl dkekrlo
```

And the scroll revealed the final ciphered truth:

```
HTB{{3NCH4NT3D_C1PH3R_D3C0D3D_fb71c056e6e62a3757ee3e62ff1029c3}}
```

---

## 🧵 TL;DR

- Group letters into chunks of 5.
- Each group uses its own Caesar shift.
- Ignore non-letters during shifting, but reinsert them at the end.
- Final flag: `HTB{{3NCH4NT3D_C1PH3R_D3C0D3D_fb71c056e6e62a3757ee3e62ff1029c3}}`

---

🔮 The Codex speaks once again.
