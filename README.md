# Snippets

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-success)

A collection of practical IT snippets (mostly PowerShell) I’ve accumulated over time — focused on identity, Microsoft 365, Exchange, Windows, and “misc” one-liners I don’t want to re-learn every six months.

## Contents

- [Entra ID](./EntraID.MD)
- [Identity (AD / auth concepts)](./Identity.MD)
- [Exchange / Exchange Online](./Exchange.MD)
- [Windows](./Windows.MD)
- [Miscellaneous](./Miscellaneous.MD)

## How to use this repo

1. Open the topic that matches what you’re doing.
2. Use the index at the top of each page to jump to the right section.
3. Copy/paste the snippet and adjust:
   - tenant / domain names
   - object IDs / group IDs
   - filters / scopes
   - permissions

> Tip: Each snippet is written to be “drop-in”, but **assume it needs review** before running in production.

## Conventions used

- **Prereqs** are listed before a snippet (modules, permissions, connectivity).
- **Outputs** are described when relevant (CSV, JSON, objects).
- **Safety**: destructive actions are marked clearly and (where practical) include `-WhatIf`.

## Contributing (to myself / future me)

If you add new snippets, try to include:

- **What it does** (1 sentence)
- **When you’d use it**
- **Prereqs**
- **Example**
- **Notes / pitfalls**

Use this mini-template:

```powershell
# What: <one line>
# When: <when you'd use it>
# Prereqs: <modules / roles / scopes>
# Notes: <gotchas>
