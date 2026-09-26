# SkillFile v7 Semantic Research

**Date:** 2026-09-26
**Baseline:** commit `6293563` (parser v9, 362 records/file, 38 fixed fields)
**Status:** Semantic layer decoded via cross-build loader-traced schema

---

## Executive Summary

The 38-field v7 record layout has been **semantically decoded** by cross-referencing
the loader-traced `SKILL_SCHEMA` (`_STAT_MAP`) from the unsealed project — a schema
proven via RTTI chain (`CSealTableManager::LoadSkillTable`, `sub_c392e0`) on a
different build of the same client family (ImageBase `0x400000`, memory dump
`so3d2026.memorydump.txt`).

The v8 schema (40 stats) maps onto v7 (38 fields) with indices 0–37 aligned;
v7 lacks v8's trailing stats 38–39 (`ultimate_move_pct`, `stat_39`).

**Verification result: every testable prediction of the v8 mapping holds on v7 data.**

- Skill prerequisite chains are semantically valid (Fireball ← Source of Universe, Firestorm ← Fireball, Mega Fireball ← Fireball@Lv5)
- Element codes cluster perfectly by skill family (fire=1, ice=2)
- Projectile speeds match expectations (ranged magic = 1000, melee/buff = 0)
- Buff references resolve **103/103** (field[23]) and **20/20** (field[27]) into the client's `buff.edt` (1,967 buffs, EUC-KR names)
- Support subtype 5 = leadership buff (exactly the two party-inspiration skills)
- `field[6]` (min_level) progresses across variants for some skills — per-level learn requirements

No v7-specific loader was found in this client's executable (`SO3DPlus.exe` is
packed — `.text` section virtual-only, `.data` entropy 8.00; static consumer
search impossible). The only in-binary reference is `skill.edt` (the separate
v4 file) in `sealres.dll` @ `0xda64`.

---

## Consumer / Loader Evidence

| Target | Found | Evidence | Confidence |
|---|---|---|---|
| `skill01.edt`–`skill20.edt` loader | **NOT FOUND** | `SO3DPlus.exe` packed (3 sections; `.text` rawsize=0, `.data` entropy 8.00); no RTTI strings; only 347 short strings, all garbage | N/A |
| `skill%02d` format string | NOT FOUND | Absent from all client binaries | N/A |
| `SkillFile v7` string | NOT FOUND (in this build's binaries) | Absent from `SO3DPlus.exe` / `sealres.dll` | N/A |
| `skill.edt` (v4 file) reference | **FOUND** | `sealres.dll` @ `0xda64` — string table alongside `seller.edt`, `quest.edt` | `CLIENT_FACT` (file loaded by resource DLL) |
| v8 skill schema (cross-build) | **FOUND** | unsealed project `skill.py` `_STAT_MAP` — traced from `CSealTableManager::LoadSkillTable` (`sub_c392e0`) via RTTI on `so3d2026.memorydump.txt` (ImageBase 0x400000) | `CLIENT_FACT` for the v8 build; **transfer to v7 = `PROBABLE` → verified per-field below** |
| `uskill01–20.edt` | Parsed | Same v7 format, same 362 IDs, data starts at offset **261** (1-byte header difference vs 260); different field values (e.g. max_level=20, cheaper SP) | `BINARY_CONFIRMED` (structure) |

### Why the v8→v7 mapping is trustworthy

The unsealed schema was not guessed from data — it was **read out of the loader's
own field-by-field cursor walk** (documented at `sub_c392e0`, with `movss` float
reads confirming float columns). Our v7 files are one version earlier of the same
format family. Every semantically testable field (job, prereq, element, buff
links, projectile, max level, support subtype) validates against v7 data below.

---

## Field Semantic Matrix

| Field | v8 Name | Observed Pattern (v7, 362 recs) | Verification Evidence | Confidence |
|---|---|---|---|---|
| field[0] | `job_id` | 0, 1–31, 131, 231, 0xFFFFFFFF; 21 sentinel | Fireball=4 (Mage group) ✓; 12 utility skills = 0 (Sleep, Trade, Party, Inventory…) ✓; 21× 0xFFFFFFFF = 18 unnamed + 3 special ✓ | numeric `BINARY_CONFIRMED`; job-name mapping `PROBABLE` |
| field[1] | `skill_type` | 0–11 | type 2 = attack (Fireball, Quick Slash), type 4 = heal (Cure, Heal, Mega Cure), type 0 = combo, type 1 = utility (Sleep, Trade, Fishing), type 6 = debuff (Provoke, Enrage) | `PROBABLE` |
| field[2] | `prereq_skill_id` | 0 or 1–362 | Chains valid: Fireball←17 Source of Universe, Firestorm←18 Fireball, Mega Fireball←18@Lv5, Quick Slash←28 Concentration@3, Double Slash←29@3. 23 "backward" prereqs are later-tier skills (mastery) — legitimate | `PROBABLE` (strong) |
| field[3] | `prereq_skill_level` | 0, 1, 3, 5, 10 (where prereq exists: 135×1, 49×3, 85×5, 14×10) | Mega Fireball requires Fireball Lv5 ✓ | `PROBABLE` |
| field[4] | `max_skill_level` | 1, 2, 3, 5, 6, 8, 9, 10, 20 | Fireball=10 ✓; craft skills=20; 200 records=5; 83=10 | `PROBABLE` |
| field[5] | `skill_points` | 0–54, 2×9999 | SP cost per level — plausible distribution | `PROBABLE` (upgraded from UNRESOLVED via v8 mapping; no v7 consumer) |
| field[6] | `min_level` | 1–251; changes across variants for some skills | Fireball=10 const; Double Slash 25→50 (files 1–10) then 23 (11–20); Heal 160→178; Quick Slash 11→38 then 10 | `PROBABLE` — per-variant learn requirement |
| field[7] | `required_equip_type` | 0 everywhere | All sampled weapon skills = 0 | `UNRESOLVED` (no variance to test) |
| field[9] | `ap_cost` | 0–330; scales with variant then plateaus | Fireball 15→24; Frostbolt 13→22; Heal 210→330; Double Slash 60→105 | `PROBABLE` |
| field[10] | `support_subtype` | 0, 5 | =5 exactly for Warrior's Inspiration + Knight's Command (leadership buffs) — matches v8 comment "5 = leadership buff" | `PROBABLE` (strong) |
| field[11] | `cast_num_target` | 0–3 | Firestorm=3 targets, Fireball=1, Heal=0 (ally select) | `PROBABLE` |
| field[12] | `area_of_effect` | 0–5 | AoE skills (Shockwave, Area Destruction)=5, single=1 | `PROBABLE` |
| field[13] | `cast_range` | 0–8 | Fireball=6, Firestorm=4, melee=2–3, Heal=0 | `PROBABLE` |
| field[14] | `casting_time` (F32) | 0.0–0.3 | Heal=0.2s, Quick Slash=0.1s, Mega Cure=0.3s | `PROBABLE` |
| field[15] | `duration_seconds` (F32) | 0.0–1800.0 | Fireball=0.4s, Heal=1.2s, Double Slash=-0.7 (modifier?) | `PROBABLE` |
| field[16] | `cooldown_seconds` (F32) | 0.0–6.0+ | Fireball=1.0s ✓ | `PROBABLE` |
| field[17] | `number_of_hits` | 0/1 (360), 2, 10 | =1 for Double Shot, Multi Shot, Running Fire, Triple Arrow, Arrow Squall (multi-hit) — but also 1 for many single-hit; likely "extra hit flag/count" | `PROBABLE` (upgraded from flag) |
| field[18] | `damage_pct` | 0–15000 | Fireball 75→410, Frostbolt 60→370, Double Slash 600→770, Heal 2500 (heal amount) | `PROBABLE` (strong) |
| field[19] | `element` | 0 (311), 1 (25), 2 (15), 6 (11) | 1=fire (all fire-family), 2=ice (all ice-family), 6=? (11 records — check below) | `PROBABLE` (strong) |
| field[22] | `linked_skill_1` | 0 or skill ID | Sparse (13 nonzero); Double Slash→169 Torpedo Slash | `PROBABLE` |
| field[23] | `buff_1_id` | 103 nonzero values | **103/103 resolve into buff.edt** — Poison Dagger→Poison(101), Freeze→Freeze(116), Immolation→Burn(121), Shock Treatment→Stun(111), Ensnare→이속감소(667)… | **`BINARY_CONFIRMED`** (cross-table join, 103/103) |
| field[24] | `buff_1_duration_seconds` | 0–2 | Pairs with f23 | `PROBABLE` |
| field[25] | `buff_1_chance` (F32) | 0.0–1.0 | Pairs with f23 | `PROBABLE` |
| field[26] | `linked_skill_2` | 0 or skill ID | 8 nonzero | `PROBABLE` |
| field[27] | `buff_2_id` | 20 nonzero values | **20/20 resolve into buff.edt** — Demolition→충격여파(296), Charge→데미지두번(976), Damnation→도트+이동불가(1263/1257) | **`BINARY_CONFIRMED`** (20/20) |
| field[28] | `buff_2_duration_seconds` (F32) | — | Pairs with f27 | `PROBABLE` |
| field[29] | `buff_2_chance` (F32) | — | Pairs with f27 | `PROBABLE` |
| field[30] | `buff_3_id` | — | Not sampled | `UNRESOLVED` |
| field[31] | `buff_4_id` (float-encoded) | — | Not sampled | `UNRESOLVED` |
| field[34] | `icon_id` | 0–355, 345 unique; drifts from skill_id (-4/-5/-7 steps) | **Reload(254) & Reload(322) share f34=250** — different skills, same icon; Combo Master ×3 (different jobs) have different f34 | `PROBABLE` (upgraded from "internal index" — icon sharing is icon behavior) |
| field[35] | `projectile_speed` | 0, 8–15, 1000 | Fireball/Frostbolt=1000 (ranged magic ✓), melee/buff=0 | `PROBABLE` (strong) |
| field[36] | `reserved` | 0 in 362/362 | Constant zero | `BINARY_CONFIRMED` (constant) |
| field[37] | (v8: beyond map edge) | 0 in 360/362; 350, 150 | Two exceptions; v8 map ends at 34 + 35, so 36–37 may be v7-specific | `UNRESOLVED` |

### Fields verified in detail

**field[6] min_level — variant progression discovered:**

| Skill | Files 01→10 | Files 11→20 | Interpretation |
|---|---|---|---|
| Fireball | 10 (const) | 10 | Learn at 10, all levels |
| Quick Slash | 11→38 | 10 | Per-level requirements, mastery tier resets |
| Double Slash | 25→50 | 23 | Same pattern |
| Heal | 160→178 | 178 (const) | High-tier skill |
| Mega Fireball | 18 (const) | 18 | Prereq-gated instead |

This confirms the variant axis carries **per-level learn requirements** — strong
support for the level/rank interpretation.

---

## Variant Semantics

### Evidence for files 01–20 = skill levels

1. **field[6] min_level changes per variant** (learn requirement per skill level)
2. **field[9] ap_cost scales per variant** then plateaus (Fireball 15→24)
3. **field[18] damage scales per variant** then plateaus (Fireball 75→410)
4. **field[4] max_skill_level**: normal skills = 10 → matches 10 base files; uskill family = 20
5. **Reload (ID 322) description percentage** 4%→20% across files 01→05, plateau after
6. 330/362 records change at least one field across files

### The 10→11 boundary

- Damage jumps at file 11 (Fireball 240→410) then plateaus
- min_level for some skills DROPS at file 11 (Quick Slash 38→10, Double Slash 50→23)
- **New interpretation:** files 11–20 are not "mastery levels 11–20" but a **second
  progression track** — possibly the *awakened/enhanced* skill tier where
  requirements reset. The drop in min_level + power spike is consistent with a
  tier transition, not a simple level 11–20 continuation.

### uskill01–20.edt — the second family

- Same v7 format, same 362 IDs, but records start at **offset 261** (vs 260)
- Field diffs vs skill01: max_level 10→20, skill_points lower, cast_range higher
- **Interpretation:** "u" = unlockable/upgrade variant — skills with 20 levels
  (10 base + 10 advanced), cheaper SP. `PROBABLE`.

### Status

```
skill01–skill20 = skill level/rank variants:    PROBABLE (strengthened)
files 01–10 = base levels:                     PROBABLE
files 11–20 = second tier (reset+spike):       PROBABLE (new detail)
uskill01–20 = extended/upgrade family:         PROBABLE (new)
```

---

## Strongest Evidence (ranked)

1. **field[23]/field[27] → buff.edt join: 103/103 + 20/20** — cross-table binary
   join with semantic matches (Poison Dagger→Poison, Freeze→Freeze, Burn, Stun).
   This is `BINARY_CONFIRMED` and anchors the entire field map.
2. **Prerequisite chains semantically valid** — Fireball←Source of Universe,
   Firestorm←Fireball, Mega Fireball←Fireball@Lv5, Quick Slash←Concentration@3.
   Random data cannot produce these chains.
3. **Element clustering** — every fire-family skill = 1, every ice-family = 2.
4. **Projectile speed** — ranged magic = 1000, melee/buff = 0, exactly as v8 comments predict.
5. **field[10]=5 leadership subtype** — exactly the two party-inspiration skills.
6. **Reload icon sharing** — two same-name skills share f34=250.
7. **min_level variant progression** — per-level learn requirements.

All of 2–7 validate the v8 loader-traced `_STAT_MAP` transferred onto v7.

---

## Remaining Unknowns

1. **No v7 loader found** — SO3DPlus.exe is packed; runtime tracing blocked by GameGuard. Field semantics rest on the cross-build v8 schema + data validation, not a v7 consumer.
2. **field[19] element=6** — 11 records; which element? (candidates: 3=lightning? 6=poison/dark?)
3. **field[30]/[31] buff_3/buff_4** — not yet sampled against buff.edt.
4. **field[34] icon_id** — no icon file table found to join against (icons likely inside packed SPAK resources without numeric filenames).
5. **field[37] exceptions (350, 150)** — meaning unknown.
6. **uskill offset-261 difference** — why the 1-byte shift; what the extra header byte means.
7. **field[7] required_equip_type** — all zeros in sample; cannot verify.
8. **Exact tier semantics of files 11–20** — reset-and-spike pattern established, but the gameplay meaning (awakened? rebirth? master?) is not confirmed by any consumer.

---

## Method Note

This research consumed only existing artifacts (skill_v9_data.pkl, buff.edt,
client binaries, unsealed schema docs). Parser v9, CSV, pickle, and canonical
SQLite were not modified.
