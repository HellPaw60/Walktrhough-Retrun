# SkillFile v7 Reverse Engineering Research — Final v3

**Date:** 2026-09-26
**Status:** Complete — fixed-layout model verified by exhaustive chain parse
**Parser:** v9 (deterministic fixed-layout chain)

---

## 1. Executive Summary

### Key Numbers (final, chain-verified)

| Metric | Value | Evidence |
|---|---|---|
| Files | 20 (`skill01.edt` – `skill20.edt`) | — |
| Header max skill ID | 363 | uint16 @ offset 64 |
| **Records per file** | **362** | Chain parse reaches exact EOF in all 20 files |
| **Total records** | **7,240** (362 × 20) | — |
| Named skills | 344 | Non-empty 32-byte name field |
| Unnamed records | 18 | Empty name field (placeholders/custom) |
| Absent IDs | 1 (ID 363) | Header value itself; records run 1–362 |
| Unique skill names (named) | 337 | 6 duplicate name groups |

### Reconciliation (exact)

```
363 = header max skill ID (capacity)
362 = records per file — sequential IDs 1..362, ZERO gaps, chain-verified
344 = named skills (non-empty name)
 18 = unnamed records (empty name field)
---
362 = 344 + 18  ✓ EXACT
363 = 362 + 1 (ID 363 itself absent)  ✓ EXACT
```

### Record layout is FIXED — not variable

Every record in every file follows one fixed layout (BINARY_CONFIRMED):

```
uint32  skill_id          (sequential 1..362)
char[32] name             (fixed 32-byte field, null-padded; empty = unnamed)
uint32  fields[38]        (fixed 38 × uint32)
uint32  desc_len          (0–499)
char[]  description       (desc_len bytes)
→ next record immediately (no trailing padding)
record_size = 192 + desc_len
```

Chain parse from offset 260 walks 362 records and lands on **exactly** the last
byte of each file (109,294 / 109,295 bytes). 20/20 files. Zero non-sequential
ID transitions. This is structural proof — no heuristics involved.

---

## 2. Historical Number Audit (what each number was)

| Number | What it actually was | Status |
|---|---|---|
| 333 | v2/v3 parser: cascade bug (garbage `Rose Cross Guild Hurray!` matches swallowed 11 real records) + over-strict desc validation rejected 3 more | **Artifact** |
| 341 / 342 / 344 | v4–v8 parsers: variable-layout assumption; each version recovered different subsets. 344 = records with non-empty names that v8 could parse | **Subset, superseded** |
| **331** | v8's count of records that *happened* to parse as "38 fields" under the flawed variable-length scan. In reality ALL 362 records have exactly 38 fields | **Artifact** — see §6 |
| 362 (subagent) | Structural scan that counted garbage entries — coincidentally equals the true count because the true count IS 362 | Right number, wrong method |
| **362 (v9)** | Fixed-layout chain parse, sequential IDs, exact EOF coverage | **FINAL — verified** |
| 30 / 19 missing | v2/v3 and v8 parser failures. True absent IDs: only 363 | **Artifacts** |
| 6,660 / 6,880 | 333×20 / 344×20 — products of parser artifacts | **Superseded by 7,240** |

---

## 3. The 18 Unnamed Records (empty name field)

| IDs | Description content | Interpretation |
|---|---|---|
| 16 | `Box Viewer, Untuk Melihat Isi Dalam BOX...` (Indonesian) | Server-custom skill (Seal Return addition) |
| 21 | `Drop Viewer. Untuk Melihat Dropan Monster...` (Indonesian) | Server-custom skill |
| 66, 67, 72–76 | `Rose Cross Guild Hurray!` | Guild-skill placeholder block |
| 70 | `Seal Online` | Placeholder |
| 87 | mining skill desc | Real skill, unnamed in this client |
| 95, 97–99, 107 | guardian/disintegrate/atoms/spirits/mimic descs | Real skills, unnamed in this client |
| 225, 226 | (fully empty, desc_len=0) | Deleted/unused slots |

All 18 have `field[0] = 0xFFFFFFFF` (sentinel category). Field data is structurally
valid. They are **not** parser failures — they are genuine records with empty name
fields.

---

## 4. Field Map (final, all 362 records)

| Index | Type | Observed | Meaning | Confidence |
|---|---|---|---|---|
| field[0] | uint32 | 0, 1–31, 131, 231, 0xFFFFFFFF | Category/class-group ID. 0 = basic utility (Sleep, Trade, Fishing, Party, Inventory…); 0xFFFFFFFF = special/unnamed (21 records); 1–6 = base job groups | Numeric field `BINARY_CONFIRMED`; job-name mapping `PROBABLE` |
| field[15] | float32 | 0.0, 0.5, 1.0, 0.7, 1800.0 … | Effect multiplier A | `PROBABLE` |
| field[16] | float32 | 0.0, 3.0, 1.0, 6.0, 5.0 … | Effect multiplier B | `PROBABLE` |
| field[17] | uint32 | 0 (185), 1 (175), 10 (1), 2 (1) | Binary flag | `BINARY_CONFIRMED` (flag; semantics UNRESOLVED) |
| field[18] | uint32 | 0–15000 range | Power/damage/heal candidate | `PROBABLE` |
| field[34] | uint32 | see §5 | Internal skill index (NOT skill_id copy) | `BINARY_CONFIRMED` (index; see §5) |
| field[36] | uint32 | 0 in 362/362 | Constant zero | `BINARY_CONFIRMED` |
| field[37] | uint32 | 0 in 360/362; 350, 150 | Mostly zero | `UNRESOLVED` |
| desc_len | uint32 | 0–499 | Description byte length | `BINARY_CONFIRMED` |

All other fields (1–14, 19–33, 35) → `UNRESOLVED`.

---

## 5. field[34] — Internal Index (verified on all 362 records)

```
field[34] == skill_id exactly:        65/362
field[34] - skill_id == -4:         118/362
field[34] - skill_id == -5:          98/362
field[34] - skill_id == -7:          37/362
field[34] - skill_id == 0:           65/362
(other small offsets: remainder)
```

The offset drifts in steps (-4, -5, -7 …) as skill_id grows — consistent with an
index into a compacted/original skill table that skips certain entries. It is
**not** a skill_id duplicate. Denominator is **362** (all records).

---

## 6. What "331" Was (explicit resolution)

The v8 parser assumed variable-length records and scanned for description
boundaries heuristically. Under that scan:

- 331 records happened to yield a "38-field" parse
- 13 records misparsed as 32/34/36/37-field layouts
- The misparses were caused by field values that coincidentally satisfied the
  desc-length heuristic (e.g. ID 356 `Triple Arrow`: field[32]=1 followed by
  byte `d` (0x64) was accepted as "desc_len=1, desc='d'")

With the v9 fixed-layout model, **all 362 records parse as exactly 38 fields**
— the 32/34/36/37 "layouts" never existed. Previous statements like
"field[36] = 0 in 331/331" are corrected to "field[36] = 0 in 362/362".

---

## 7. Variant Analysis (final numbers)

| Metric | Count (of 362) |
|---|---|
| Records with any field change across 20 files | 330 |
| Constant records (named) | 21 (e.g. Sleep) |
| Constant records (unnamed) | 11 |

**Fireball (ID 18):** field[18] power 75 → 135 → 240 (files 01→10), jump to 410
at file 11, plateau to file 20. field[9]: 15 → 24.

**Textual variant exception:** Reload (ID 322) is the only record whose
description differs across files (4% → 8% → 12% → 16% → 20% headshot-cooldown
reset chance, plateau at 20% from file 05). All other 361 records have
byte-identical descriptions in all 20 files.

Conclusion unchanged: **skill01–skill20 = skill level/rank variants — `PROBABLE`**
(330/362 records change; systematic progression + plateau; no loader found).

---

## 8. field[0] Category Distribution (final, 362 records)

| Value | Count | Sample | Interpretation |
|---|---|---|---|
| 0 | 12 | Sleep, Trade, Fishing, Refine, Party, Inventory, Emoticon, Seller's/Buyer's Kiosk, Duel Request | Basic/utility (all players) |
| 1 | 18 | Great Sword Combo, Quick Slash | Warrior group |
| 2 | 18 | Sword Combo, Chivalry | Knight group |
| 3 | 17 | Knife Combo, Merriment | Jester group |
| 4 | 25 | Staff Combo, Fireball | Mage group |
| 5 | 27 | Mace Combo, Prayer | Priest group |
| 6 | 20 | Hammer Combo, Cook | Craftsman group |
| 7–31, 131, 231 | ~200 | Hunter/Gunner/Archer/Chef/advanced groups | Sub-class groups (`UNRESOLVED` names) |
| 0xFFFFFFFF | 21 | 18 unnamed + Seal Online, Unknown Skill, Royal Food | Special/sentinel |

---

## 9. Consumer/Runtime Research

**NOT FOUND.** No v7 loader or `skill%02d` format string in client executable.
Variant semantics remain `PROBABLE`.

---

## 10. Outputs

- CSV: `D:\SealR_Database\skill_v7_parsed.csv` (7,240 rows, 44 columns, 1.8 MB)
- Parser data: `D:\SealR_Database\skill_v9_data.pkl`

---

## 11. Validation

| Check | Result |
|---|---|
| Chain parse reaches exact EOF | PASS (20/20 files) |
| Sequential IDs 1–362, zero gaps | PASS |
| 344 named + 18 unnamed = 362 | PASS |
| All records fixed 38-field layout | PASS |
| field[36] = 0 in 362/362 | PASS |
| No raw client artifacts published | PASS |
| Canonical DB unchanged | PASS |
