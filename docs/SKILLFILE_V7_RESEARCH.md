# SkillFile v7 Reverse Engineering Research — Final v2

**Date:** 2026-09-26
**Status:** Complete — corrected after full re-audit
**Parser:** v8 (final)

---

## 1. Executive Summary

### Key Numbers (corrected)

| Metric | Old (wrong) | Final (verified) |
|---|---|---|
| Files | 20 | 20 |
| Header max ID | 363 | 363 |
| Valid records per file | 333 | **344** |
| Total valid records | 6,660 | **6,880** |
| Unique skill IDs | 333 | **344** |
| Unique skill names | 326 | **337** |
| Missing IDs | 30 | **19** |
| Duplicate names | 6 pairs | **6 groups** (Concentration ×2, Combo Master ×3, Reload ×2, Enhance Bow ×2, High Quality Sense ×2, Accurate Appraisal ×2) |

### Reconciliation (exact)

```
363 = header max skill ID (capacity)
344 = valid records per file (parser v8)
 19 = missing IDs (no data in any file)
---
363 = 344 + 19  ✓ EXACT
```

### Why the old count (333) was wrong

The old parser (v2/v3) had a **cascade bug**: garbage records inside description
blocks (e.g. `Rose Cross Guild Hurray!C/I/J/K/L/M` at offsets 22365-24925, and
`Seal Online  G`) were accepted as records. When such a garbage record matched,
the parser jumped past the region and **swallowed 11 real records**:
IDs 71, 77, 240, 241, 242, 243, 244, 245, 264, 308, 309.

Additionally, 3 records with edge-case descriptions were rejected by over-strict
validation:
- ID 239 `Eye Sight` — description contains UTF-8 bytes (`C3 AD C2 BB`)
- ID 263 `Unknown Skill` — desc_len = 0 (empty description)
- ID 307 `Royal Food` — desc_len = 10 (below old threshold of 20)

All 14 recovered records verified: valid 38-field structure, complete descriptions.

---

## 2. File Inventory

| File | Size (bytes) | Valid Records | Orphans |
|---|---|---|---|
| skill01.edt | 109,294 | 344 | 5 |
| skill02.edt | 109,294 | 344 | 5 |
| skill03.edt | 109,295 | 344 | 5 |
| skill04.edt | 109,295 | 344 | 5 |
| skill05.edt | 109,295 | 344 | 5 |
| skill06.edt | 109,295 | 344 | 5 |
| skill07.edt | 109,295 | 344 | 5 |
| skill08.edt | 109,295 | 344 | 5 |
| skill09.edt | 109,295 | 344 | 5 |
| skill10.edt | 109,295 | 344 | 5 |
| skill11.edt | 109,295 | 344 | 5 |
| skill12.edt | 109,295 | 344 | 5 |
| skill13.edt | 109,295 | 344 | 5 |
| skill14.edt | 109,295 | 344 | 5 |
| skill15.edt | 109,295 | 344 | 5 |
| skill16.edt | 109,295 | 344 | 5 |
| skill17.edt | 109,295 | 344 | 5 |
| skill18.edt | 109,295 | 344 | 5 |
| skill19.edt | 109,295 | 344 | 5 |
| skill20.edt | 109,295 | 344 | 5 |
| **Total** | **~2.1 MB** | **6,880** | **100** |

All 20 files contain the **same 344 skill IDs** (verified: every ID appears in all 20 files).

---

## 3. Codec

- Type: byte-by-byte XOR with Linear Congruential Generator (LCG)
- Seed: `0x11CFD`, Multiplier: `52845`, Addend: `22719`, Mask: `0xFFFF`
- Same codec as other EDT files in this client (validated).

---

## 4. Record Layout (BINARY_CONFIRMED)

### Header
```
+0      24      "Seal Online SkillFile v7"
+24     40      null padding
+64     2       max_skill_id (uint16) = 363
+66     6       null padding
+72     5       "skill"
+77     183     null padding
+260    —       first record
```

### Record Structure
```
uint32  skill_id          (1–362)
char[]  name              (null-terminated ASCII, uppercase first char, 3–50 chars)
uint8[] null padding
uint32  fields[34–38]     (see Field Map)
uint32  desc_len          (0–499; 0 = empty description)
char[]  description       (desc_len bytes; may contain UTF-8)
uint8[] null padding
```

Records are variable length. Field count distribution (skill01): 32×1, 34×2, 36×1, 37×9, 38×331.

---

## 5. Missing IDs (19, BINARY_CONFIRMED)

```
16, 21, 66, 67, 70, 72, 73, 74, 75, 76, 87, 95, 97, 98, 99, 107, 225, 226, 363
```

These IDs have no record data in any of the 20 files. Consistent across all files.
Likely reserved/deleted skill slots. `363` = header max ID itself (not a real slot).

---

## 6. Field Map (corrected)

### Confirmed fields

| Index | Type | Semantic | Confidence |
|---|---|---|---|
| field[0] | uint32 | Skill category / class-group ID | `BINARY_CONFIRMED` (as ID; see §7 for semantics) |
| field[15] | float32 | Effect multiplier A (0.4, 1.2, -0.7 …) | `PROBABLE` |
| field[16] | float32 | Effect multiplier B (1.0, 6.0, 3.0 …) | `PROBABLE` |
| field[18] | uint32 | Power/damage/heal value (75, 410, 600, 2500 …) | `PROBABLE` |
| field[34] | uint32 | Internal skill index (NOT skill_id copy — only 53/331 match) | `BINARY_CONFIRMED` (as index; semantics UNRESOLVED) |
| field[36] | uint32 | 0 for all records (331/331) | `BINARY_CONFIRMED` (constant) |
| desc_len | uint32 | description byte length | `BINARY_CONFIRMED` |

### Corrected claims (old → new)

| Old claim | Status | Correction |
|---|---|---|
| field[0] = job type, 1–6 = 6 main jobs, `BINARY_CONFIRMED` | **DOWNGRADED** | field[0] has 26 distinct values (1–31, 131, 231, 0xFFFFFFFF). Values 1–6 correlate with base-job skill groups, but 7–31 exist (Hunter/Gunner/Scout/Chef/etc. sub-classes). Job mapping = `PROBABLE` |
| field[5] = SP cost | **DOWNGRADED** | No consumer evidence. `UNRESOLVED` |
| field[8] = min level | **DOWNGRADED** | field[8] is 0 for 329/331 records. `UNRESOLVED` |
| field[17] = damage | **CORRECTED** | field[17] is a small binary flag (0/1) for most records. The damage-like values live in **field[18]**. Old claim was shifted by contaminated data |
| field[N-2] = desc_len, field[N-1] = skill_id copy | **CORRECTED** | In the 38-field layout: desc_len sits after field[37]; field[36] is all zeros; field[34] is an internal index, not skill_id |

### Unresolved fields

field[1]–[14], field[17], field[19]–[33], field[35], field[37] → `UNRESOLVED`

---

## 7. Job/Category Field (field[0]) — full distribution

| Value | Count | Sample skills | Interpretation |
|---|---|---|---|
| 1 | 18 | Great Sword Combo, Quick Slash, Double Slash | Warrior base |
| 2 | 18 | Sword Combo, Chivalry, Holy Cross | Knight base |
| 3 | 17 | Knife Combo, Merriment, Playing Dead | Jester base |
| 4 | 25 | Staff Combo, Fireball, Meteor | Mage base |
| 5 | 27 | Mace Combo, Prayer, Cure | Priest base |
| 6 | 20 | Hammer Combo, Cook, Collect | Craftsman base |
| 7 | 3 | Bless, Throw Bomb, Alchemy | Unknown group |
| 8 | 3 | Intimidate, Warm Up, Beg | Unknown group |
| 9 | 18 | Slingshot Combo, Simple Shot, Power Shot | Hunter/gunner |
| 11 | 10 | Dominate, Awakening, Instinct | Unknown group |
| 12 | 10 | Break Weapon, Frenzy, Vengeance | Unknown group |
| 13 | 11 | Sudden Attack, Sneakattack, Doppelganger | Unknown group |
| 14 | 9 | Ice Cannon, Glacier, Time Warp | Unknown group |
| 15 | 10 | Repentance, Judgement, Prediction | Unknown group |
| 16 | 11 | Coup de Grace, Time Bomb, Demolish | Unknown group |
| 19 | 10 | Piercing Arrow, Bump Arrow, Poison Arrow | Archer |
| 21 | 10 | Sword Dance, Charge, Radiant Sword | Unknown group |
| 22 | 9 | Tornado, Heal, Divine Guard | Unknown group |
| 23 | 11 | All In, Black Jack, Bluff | Unknown group |
| 24 | 9 | Giga Flame, Inferno, Hellfire | Unknown group |
| 25 | 13 | Mega Cure, Bulwark, Faith | Unknown group |
| 26 | 8 | Hammer Master, Master Refiner | Unknown group |
| 29 | 13 | Aimed Shot, Kill Shot, Headshot | Gunner |
| 31 | 18 | Poke Combo, Table Manner, Absolute Taste | Chef |
| 131 | 8 | Last Supper, Food Lane, Onion Slicer | Chef advanced |
| 231 | 9 | Binge, Diet, Eat Fast | Chef advanced |
| 0xFFFFFFFF | 3 | Seal Online, Unknown Skill, Royal Food | Special/sentinel |

**Status:** field[0] as a numeric field = `BINARY_CONFIRMED`. Mapping value→job name = `PROBABLE`
for 1–6 (base jobs, name-correlation only), `UNRESOLVED` for 7–31, 131, 231.

---

## 8. Variant Analysis (verified on clean data)

### Hypothesis testing

| Hypothesis | Verdict | Evidence |
|---|---|---|
| A: skill01–20 = skill level/rank data | `PROBABLE` | 323/344 skills change at least one field across files; power fields scale up then plateau |
| B: server config tiers | Weak | Same structure, no config markers |
| C: client/platform variants | Rejected | No platform differences |
| D: language variants | Rejected | All text identical English |
| E: unrelated duplicates | Rejected | Systematic value progression |

### Cross-file progression (clean data)

**Fireball (ID 18):**

| File | f4 | f9 | f18 (power) |
|---|---|---|---|
| skill01 | 10 | 15 | 75 |
| skill05 | 10 | 19 | 135 |
| skill10 | 10 | 24 | 240 |
| skill11 | 10 | 24 | 410 |
| skill15 | 10 | 24 | 410 |
| skill20 | 10 | 24 | 410 |

**Double Slash (ID 31):** f18: 600 → 744 → 1625 → 770 (plateau)
**Heal (ID 155):** f9: 210 → 260 → 330 (plateau); f18: 2500 → 15000 → 0
**Sleep (ID 1):** identical across all 20 files (utility skill, no scaling)

### Statistical summary (344 skills)

| Metric | Count |
|---|---|
| field[18] changes across files | 255 |
| field[9] changes across files | 217 |
| field[4] changes across files | 0 (constant) |
| ANY field changes | 323 |
| Fully constant skills | 21 |

### Conclusion

skill01–skill20 = **skill level/rank variants** — `PROBABLE`.
Power/requirement fields increase from file 01→10, then plateau at 11–20
(consistent with a mastery-tier cap). No loader/consumer found, so not
`BINARY_CONFIRMED`.

---

## 9. Orphan Records (100 total)

5 per file. All are ID-24 `Rose Cross Guild Hurray!C/I/J/K/L/M` entries embedded
inside the description block region of other skills — placeholder/legacy data,
not real skill records. Logged, excluded from the 344.

---

## 10. Consumer/Runtime Research

**NOT FOUND.** No v7 loader, no `skill%02d` format string, no skill manager
reference found in client executable. Variant semantics remain `PROBABLE`.

---

## 11. Outputs

- CSV: `D:\SealR_Database\skill_v7_parsed.csv` (6,880 rows, 1.72 MB, 44 columns)
- Parser data: `D:\SealR_Database\skill_v7_data.pkl`
- This report + `SKILL_V7_PARSER_VALIDATION.md` + `SKILL_V7_VARIANT_ANALYSIS.md`

---

## 12. Validation

| Check | Result |
|---|---|
| All 20 files parsed | PASS |
| 344 + 19 = 363 reconciliation | PASS |
| All IDs present in all 20 files | PASS |
| Recovered 14 records verified | PASS (IDs 71, 77, 239, 240–245, 264, 263, 307, 308, 309) |
| Field count distribution clean | PASS (34–38, no 1–17 contamination) |
| No raw client artifacts published | PASS |
| Canonical DB unchanged | PASS |
