# Seal Return Walkthrough — Player Progression Guide

> **Tujuan:** Jawab pertanyaan "Karakter saya level X, job Y. Sekarang harus ngapain?"
> 
> **Source:** `D:\SealR_Database` — ekstrak binary dari client Seal Online Return v5 (sealrv5.com)
> 
> **Update:** 2026-09-26

---

## 1. Progression Band

| Level | Band | Fokus |
|---|---|---|
| 1–9 | **BEGINNER** | Belajar dasar, hunting monster lewat, kumpulin equip awal |
| 10–19 | **EARLY** | Job change pertama, quest exp, map baru |
| 20–29 | **DEVELOPING** | Equip upgrade, map challenge pertama |
| 30–39 | **MID** | Dungeon pertama, grinding efisien |
| 40–49 | **PROGRESSION** | High monster, map menengah-atas |
| 50–59 | **LATE** | Endgame preparation, rare monster |
| 60–74 | **ENDGAME** | Map premium, monster kuat |
| 75–100 | **LEGEND** | Map tertinggi, monster elite |

**Evidence:** `DERIVED` — progression band dihitung dari distribusi monster level (5,161 valid progression monsters, level dari `monster.edt` field2). Band ditentukan oleh kluster level monster yang tersedia di map.

---

## 2. Level 1–9 (BEGINNER)

### Map

| Map | Nama | Level Monster | Keterangan |
|---|---|---|---|
| 10 | Silon Forest | L1–L27 | **RECOMENDED** — monster lengkap L1-L18 |
| 18 | Upstream of Mimir River | L1–L17 | Dekat Elim, monster dasar |
| 24 | Trevia Valley | L1–L16 | Alternatif, dekat Adel Monastery |
| 110 | Candy Village | L262–L271 | **JANGAN** — terlalu tinggi |
| 123 | Mt. Pionur Entrance | L300 | **JANGAN** — terlalu tinggi |

**Evidence:** `CLIENT_FACT` — `map_info.edt` field Bale Level

### Monster yang Dibur

| Level | Monster | Map | Variant |
|---|---|---|---|
| L1 | Piya | 10, 12, 18 | [Weak], [Naive], [Happy] |
| L1 | Flora | 9, 15, 16 | — |
| L1 | Beanie | 18, 24, 31 | [Wet], [Naive], [Happy] |
| L3 | Treasure Chest | 10, 18, 24 | — |
| L6 | Moo Moo | 10, 12, 18 | — |
| L6 | Worm-O | 17, 18 | — |
| L8 | Rascal Rabbit | 1, 18, 24 | — |
| L9 | Servant Mushroom | 17, 24 | — |

**Variant note:** `[Weak]`, `[Naive]`, `[Happy]` = versi lebih lemah. `[Land Type]`, `[Wet]` = versi spesifik map.

**Evidence:** `DERIVED` — monster_progression table (calculated from `monster.edt` field2 + `map_spawns`). Individual monster name/level: `BINARY_CONFIRMED`.

### Equipment Milestone

| Level | Item | Slot |
|---|---|---|
| 0–6 | Priest's Practice Mace | ACCESSORY |
| 0–6 | Game Master's Bastard | ARMOR |
| 0–6 | Mage's Practice Staff | BOOTS |
| 0–6 | Jester's Practice Dagger | HELMET |
| 0–6 | Rolled-up Newspaper | SHIELD |
| 5–15 | Wooden Bludgeon.G | ACCESSORY |
| 5–15 | Japan's Festival Fan | ARMOR |

**Evidence:** `BINARY_CONFIRMED` — `item_classification` table, binary type field dari `iteminfo.edt` (type=9→ACCESSORY, type=4→ARMOR, type=11→BOOTS, type=7→HELMET, type=6→SHIELD)

---

## 3. Level 10–19 (EARLY)

### Map

| Map | Nama | Level Monster | Keterangan |
|---|---|---|---|
| 12 | Outer Lines of Lime | L1–L36 | **RECOMENDED** — hunting L10-L18 |
| 17 | Adel Monastery | L6–L26 | Quest hub, monster L12-L18 |
| 9 | Midstream of Mimir River | L25–L39 | Mulai L16+ |
| 42 | Blue Eye | L210–L275 | **JANGAN** |

**Evidence:** `CLIENT_FACT` — `map_info.edt` Bale Level

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 10 | Moo Moo the Great | 12, 18 |
| 11 | Red Plumber | 10, 16, 76 |
| 12 | Queen Mushroom | 17, 24 |
| 12 | Flying Pig | 18 |
| 13 | Afro Tree | 4, 9, 17 |
| 14 | Mage Piya | 10, 12 |
| 14 | Cleric Piya | 10, 12 |
| 16 | Giant Rascal Rabbit | 4, 24 |
| 16 | Bah Bah | 4, 82 |
| 16 | Knight Piya | 10, 12 |
| 16 | Stiff Horse | 18 |
| 18 | Wood Golem | 17, 31 |

**Evidence:** `DERIVED` — monster_progression + map_spawns

### Equipment Milestone

| Level | Item | Slot |
|---|---|---|
| 5–15 | Wooden Bludgeon.G | ACCESSORY |
| 5–15 | Japan's Festival Fan | ARMOR |
| 5–15 | Nekomaru | BOOTS |
| 5–15 | Wooden Shield.DG | CAPE |
| 9–19 | Bastard Sword.G | SHIELD |
| 10–20 | Slasher.G | HELMET |
| 15–25 | Cleric Piya's Syringe.G | ACCESSORY |
| 15–25 | Guitar | ARMOR |
| 15–25 | Keyboard | BOOTS |
| 15–25 | Piya's Sword.G | SHIELD |
| 16–26 | Aubergine Dagger.G | HELMET |

**Evidence:** `BINARY_CONFIRMED` — item_classification binary type

---

## 4. Level 20–29 (DEVELOPING)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 9 | Midstream of Mimir River | L25–L39 |
| 17 | Adel Monastery | L6–L26 |
| 31 | Upstream of Glasis River | L1–L51 |
| 4 | Outside Crude Dungeon | L13–L48 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 21 | Servant Mushroom | 17, 24 |
| 22 | Nixie | 10, 31 |
| 23 | Windy | 4, 31 |
| 23 | Silky Joe the Boxer | 10, 17 |
| 24 | Warrior Piya | 10, 12 |
| 25 | Happy Bah Bah | 4, 9 |
| 26 | Joe the Kick Boxer | 10, 16, 17 |
| 27 | Blue Plumber | 10, 16 |
| 28 | Mage Piya | 10, 12 |
| 29 | Cleric Piya | 10, 12 |

### Equipment Milestone

| Level | Item | Slot |
|---|---|---|
| 21–31 | Crystalline Shield.G | CAPE |
| 25–35 | Wagon Driver's Staff.G | BOOTS |
| 25–35 | Ghost's Shield.G | CAPE |
| 25–35 | Organic Carrot | SHIELD |

---

## 5. Level 30–39 (MID)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 16 | Eastern Laywook Forest | L26–L47 |
| 9 | Midstream of Mimir River | L25–L39 |
| 4 | Outside Crude Dungeon | L13–L48 |
| 1 | Land's End | L51–L82 |
| 31 | Upstream of Glasis River | L1–L51 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 30 | Knight Piya | 10, 12 |
| 30 | Pumpkeener | 62 |
| 31 | Rascal Rabbit | 1, 18, 24 |
| 32 | Gloomy Humbug | 4, 9 |
| 33 | Mama Bear | 16 |
| 33 | Skullo | 61, 62 |
| 34 | Crow | 9 |
| 34 | Tarantula | 62 |
| 35 | Giant Rascal Rabbit | 4, 24 |
| 36 | Warrior Piya | 10, 12 |
| 39 | Traveling Cat in Boots | 9 |
| 39 | Abyss | 61, 62 |

**Note:** Map 62 (Crude Dungeon) muncul sebagai map efektif untuk L30+.

---

## 6. Level 40–49 (PROGRESSION)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 16 | Eastern Laywook Forest | L26–L47 |
| 15 | Western Laywook Forest | L35–L47, L65–L81 |
| 31 | Upstream of Glasis River | L124+ |
| 4 | Outside Crude Dungeon | L85–L99 |
| 61 | Clement Mine | L51–L109 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 40 | Rascal Rabbit(C) | 1, 18, 24 |
| 41 | Windy | 4, 31 |
| 42 | Head of Dullahan | 4, 14, 15 |
| 42 | Ghost Mage | 62 |
| 45 | Ghost Knight | 62 |
| 45 | Pumpisto | 66 |
| 47 | Forest Ogre | 9, 15, 16 |
| 48 | Tarantula Ben | 62 |

---

## 7. Level 50–59 (LATE)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 62 | Crude Dungeon | L30–L52 |
| 61 | Clement Mine | L51–L109 |
| 33 | Mt. Trevia | L52–L61 |
| 66 | Aleph Silon | L39–L80 |
| 75 | West of Sealed Island | L74–L80 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 50 | Tarantula Rabbit | 62 |
| 51 | Steel Golem | 1, 31 |
| 52 | Samba Cactus | 33 |
| 52 | Servanguy | 36 |
| 55 | Knight of Darkness | 1 |
| 55 | Super Steel Golem | 66 |
| 58 | Sandy Windy | 33, 75 |

---

## 8. Level 60–74 (ENDGAME)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 14 | Herakus Forest | L42–L91 |
| 15 | Western Laywook Forest | L65–L81 |
| 61 | Clement Mine | L51–L109 |
| 62 | Crude Dungeon | L30–L52 |
| 66 | Aleph Silon | L70–L105 |
| 34 | Lake Cross | L64–L73 |
| 68 | Forest of Death | L153–L174 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 60 | Soul Collector | 68, 93 |
| 61 | Meow-gician | 33 |
| 63 | Red Plumber | 10, 16, 76 |
| 64 | Saloa | 34, 76 |
| 65 | Computer Freak | 15 |
| 66 | Succubus | 14, 15, 76 |
| 69 | Solar | 29, 30 |
| 70 | Aleph | 66, 74, 98 |
| 72 | Bubble Dragon | 29, 30, 82 |

---

## 9. Level 75–100 (LEGEND)

### Map

| Map | Nama | Level Monster |
|---|---|---|
| 74 | Sealed Cave | L81–L91 |
| 76 | East of Sealed Island | L63–L72 |
| 29 | Downstream of Glasis River | L83–L108 |
| 30 | Glasis Plains | L80–L105 |
| 43 | Mt. Cross | L92–L120 |
| 69 | Catacombs | L120–L190 |
| 98 | Esdelron Lake | L184–L220 |
| 93 | Dungeon of Death | L194–L212 |

### Monster yang Dibur

| Level | Monster | Map |
|---|---|---|
| 75 | AquaKing Yamok | 40, 79 |
| 75 | Dude | 4 |
| 80 | Queen Meroa | 30 |
| 80 | Cool Guy | 66 |
| 80 | Titan Skull | 98 |
| 81 | Aleph | 66, 74, 98 |
| 85 | High Nixie | 1, 30, 75 |
| 85 | Dullahan | 14 |
| 86 | Platinum Plumber | 14, 66 |
| 88 | Cankura | 36, 37, 66 |
| 90 | Bell | 99 |
| 90 | CZ Boy | 66 |
| 92 | Afro Rock | 43 |
| 92 | Creepy Humbug | 66 |
| 95 | Pedron | 69, 98 |
| 95 | Lizzard Man | 29 |
| 96 | Notorsonia | 43 |
| 98 | Ghost Midnight | 66 |

---

## 10. Job Progression

| Job | Level Change | Detail |
|---|---|---|
| Warrior | L10 | Heavy melee DPS |
| Knight | L10 | Defensive tank |
| Magician | L10 | Elemental caster |
| Cleric | L10 | Healer |
| Hunter | L10 | Ranged DPS |
| Jester | L10 | Stealth DPS |
| Craftsman | L10 | Crafter |

**Evidence:** `EXTERNAL_REFERENCE` — source `game_knowledge`, bukan dari binary client. Tidak ada job change data di `flag.edt` atau table lain di client. Level requirement (L10 first job) adalah knowledge dari bermain game.

**Catatan:** Job change L30/L60 (second/third job) tidak tercatat di client binary. Silakan konfirmasi ke sumber gameplay.

---

## 11. Known NPCs

### NPC Identity

| Metric | Value |
|---|---|
| Total NPC identities | 1,928 |
| Resolved names | 293 |
| Unresolved names | 1,635 |

### NPC Names (Sample)

| NPC ID | Name | Source |
|---|---|---|
| 4429 | Joan | monsters.edt |
| 4441 | Arus | monsters.edt |
| 5288 | Duran | monsters.edt |
| 5690 | Hanaiel | monsters.edt |

**Evidence:**
- Identity exists: `CLIENT_FACT` — `npc_locations` (1,300 placements), `npc_dialog` (793 records), `quest_dialog_nodes` (group_id references)
- Names resolved: `PROBABLE` — name dari `monsters.edt` (NPC = MonsterID tertentu, tapi tidak ada consumer runtime yang mengkonfirmasi semantic NPC)
- Names unresolved: `UNRESOLVED` — 1,635 NPC ID tidak ditemukan di `monsters.edt` atau string table

### NPC Locations

| Metric | Value |
|---|---|
| Total placements | 1,300 |
| Unique NPC IDs placed | 51 |
| Unique maps with NPCs | 98 |
| NPCs with both dialog AND location | 16 |

**Evidence:** `CLIENT_FACT` — `npc_locations` table, data placement diekstrak dari `map/npc*.edt` files

---

## 12. Unresolved / Deferred

| Area | Status | Reason |
|---|---|---|
| Quest chain | `UNRESOLVED` | No flag transition logic found in binary |
| Quest → NPC semantic | `UNRESOLVED` | ID equality only, no consumer runtime |
| Quest → Monster | `UNRESOLVED` | No exact reference in quest.edt binary |
| Drop table | `UNRESOLVED` | `drop.py` schema exists, no actual file in client |
| DropRate | `DEFERRED` | Not investigated |
| Skill semantics | `UNRESOLVED` | 14,402 skill records parsed, effect mapping unknown. Skills table di SQLite = 0 (belum di-parse) |
| Map connection | `UNRESOLVED` | No teleporter/warp data in binary |
| Quest objective | `UNRESOLVED` | action_id semantics partial |
| Job progression detail | `EXTERNAL_REFERENCE` | Tidak ada di client binary, source dari gameplay knowledge |
| 1,635 NPC names | `UNRESOLVED` | Tidak ditemukan di `monsters.edt` atau string table |

---

## 13. Evidence Legend

| Label | Meaning | Contoh |
|---|---|---|
| `BINARY_CONFIRMED` | Langsung dari client binary | item type → slot, monster level (field2) |
| `CLIENT_FACT` | Dari client, parsed facts | map name, NPC locations, quest descriptions |
| `DERIVED` | Dihitung dari data lain | progression band, monster_per_map assignment |
| `PROBABLE` | Equality/correlation, tidak ada consumer runtime | NPC name dari monsters.edt |
| `EXTERNAL_REFERENCE` | Wiki/community/game knowledge | job names, level requirements |
| `UNRESOLVED` | Tidak ada evidence | quest chain, drop table, 1,635 NPC names |
| `DEFERRED` | Tidak dikerjakan | drop rate, skill effect |

---

## 14. Canonical Numbers

| Table | Count | Evidence |
|---|---|---|
| monsters | 9,999 | `BINARY_CONFIRMED` |
| items | 16,318 | `BINARY_CONFIRMED` |
| quest_identity | 717 | `CLIENT_FACT` |
| quest_dialog_nodes | 39,950 | `CLIENT_FACT` |
| npc_identity | 1,928 | `CLIENT_FACT` |
| resolved NPC | 293 | `PROBABLE` |
| unresolved NPC | 1,635 | `UNRESOLVED` |
| npc_locations | 1,300 | `CLIENT_FACT` |
| npc_dialog | 793 | `CLIENT_FACT` |
| monster_progression | 5,161 | `DERIVED` |
| map_progression_candidates | 40 | `DERIVED` |
| map_progression_graph | 1,247 | `DERIVED` |
| equipment_progression | 125 | `BINARY_CONFIRMED` |
| skills | 0 | `UNRESOLVED` |

---

## 15. Research Source

Semua data dari **`D:\SealR_Database`**:
- `extracted/_extracted/` — 29,988 files dari client SPAK/EDT
- `output/Seal_Return_Database.sqlite` — 65 tables, 110MB
- Monster: 9,999 records (248 bytes, 31×int64 schema)
- Item: 16,318 records (85 columns)
- Quest: 717 identity + 39,950 dialog nodes
- NPC: 1,928 identities (293 named, 1,635 unresolved)

**Jangan tanya "di tanya ke Discord". Semua data di atas diekstrak langsung dari file client.**
