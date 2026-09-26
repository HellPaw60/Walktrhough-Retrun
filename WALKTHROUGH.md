# Seal Return Walkthrough — Player Progression Guide

> **Tujuan:** Jawab pertanyaan "Karakter saya level X, job Y. Sekarang harus ngapain?"
> 
> **Update:** 2026-09-26

---

## Level 1–9 (BEGINNER)

### Tujuan
Belajar dasar, hunting monster lewat, kumpulin equipment pertama.

### Pergi ke
- **Map 10** — Silon Forest (L1–L27, **CANDIDATE** — monster lengkap L1-L18 berdasarkan map_spawns)
- **Map 18** — Upstream of Mimir River (L1–L17, **CANDIDATE**)
- **Map 24** — Trevia Valley (L1–L16, **CANDIDATE**)

### Hunting
| Level | Monster | Map |
|---|---|---|
| L1 | Piya | 10, 12, 18 |
| L1 | Flora | 9, 15, 16 |
| L1 | Beanie | 18, 24, 31 |
| L6 | Moo Moo | 10, 12, 18 |
| L8 | Rascal Rabbit | 1, 18, 24 |
| L9 | Servant Mushroom | 17, 24 |

### Equipment
| Level | Item | Slot |
|---|---|---|
| 0–6 | Priest's Practice Mace | ACCESSORY |
| 0–6 | Game Master's Bastard | ARMOR |
| 0–6 | Mage's Practice Staff | BOOTS |
| 0–6 | Jester's Practice Dagger | HELMET |
| 0–6 | Rolled-up Newspaper | SHIELD |

### Job
Belum ada job change di client binary. Konfirmasi ke sumber gameplay untuk requirement first job.

### Next Step
Naik ke L10. Lanjut ke band EARLY.

### Evidence
- Map: `CLIENT_FACT` — `map_info.edt` Bale Level
- Monster: `DERIVED` — `monster_progression` + `map_spawns`
- Equipment: `BINARY_CONFIRMED` — `iteminfo.edt` type field

---

## Level 10–19 (EARLY)

### Tujuan
Job change pertama (L10, dari gameplay), map baru, quest exp.

### Pergi ke
- **Map 12** — Outer Lines of Lime (L1–L36, **CANDIDATE** — hunting L10-L18)
- **Map 17** — Adel Monastery (L6–L26, **CANDIDATE**)
- **Map 9** — Midstream of Mimir River (L25–L39, **CANDIDATE**)

### Hunting
| Level | Monster | Map |
|---|---|---|
| 10 | Moo Moo the Great | 12, 18 |
| 11 | Red Plumber | 10, 16, 76 |
| 12 | Queen Mushroom | 17, 24 |
| 13 | Afro Tree | 4, 9, 17 |
| 14 | Mage Piya | 10, 12 |
| 14 | Cleric Piya | 10, 12 |
| 16 | Giant Rascal Rabbit | 4, 24 |
| 16 | Knight Piya | 10, 12 |
| 18 | Wood Golem | 17, 31 |

### Equipment
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

### Job
First job change di L10 (dari gameplay/external knowledge):
- Warrior (melee DPS)
- Knight (tank)
- Magician (caster)
- Cleric (healer)
- Hunter (ranged)
- Jester (stealth)
- Craftsman (crafter)

**Evidence:** `EXTERNAL_REFERENCE` — `game_knowledge`, tidak ada job change data di client binary. Tidak ada `change_job_id` atau quest binary yang berhasil dipetakan ke nama job.

### Item Acquisition
Shop item tersedia tapi banyak unresolved. Cek shop di setiap map untuk item yang dijual NPC.

### Next Step
Lanjut ke L20+. Band DEVELOPING.

---

## Level 20–29 (DEVELOPING)

### Tujuan
Equip upgrade, map challenge pertama, perluas hunting ground.

### Pergi ke
- **Map 9** — Midstream of Mimir River (L25–L39, **CANDIDATE**)
- **Map 17** — Adel Monastery (L6–L26, **CANDIDATE**)
- **Map 31** — Upstream of Glasis River (L1–L51, **CANDIDATE**)
- **Map 4** — Outside Crude Dungeon (L13–L48, **CANDIDATE**)

### Hunting
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

### Equipment
| Level | Item | Slot |
|---|---|---|
| 21–31 | Crystalline Shield.G | CAPE |
| 25–35 | Wagon Driver's Staff.G | BOOTS |
| 25–35 | Ghost's Shield.G | CAPE |
| 25–35 | Organic Carrot | SHIELD |

### Next Step
Lanjut ke L30+. Band MID — mulai masuk dungeon.

---

## Level 30–39 (MID)

### Tujuan
Dungeon pertama (Crude Dungeon), grinding efisien, equipment upgrade signifikan.

### Pergi ke
- **Map 62** — Crude Dungeon (L30–L52, **CANDIDATE** — berdasarkan monster/map data)
- **Map 16** — Eastern Laywook Forest (L26–L47, **CANDIDATE**)
- **Map 4** — Outside Crude Dungeon (L13–L48, **CANDIDATE**)
- **Map 1** — Land's End (L51–L82, **CANDIDATE**)
- **Map 31** — Upstream of Glasis River (L1–L51, **CANDIDATE**)

### Hunting
| Level | Monster | Map |
|---|---|---|
| 30 | Knight Piya | 10, 12 |
| 30 | Pumpkeener | 62 |
| 31 | Rascal Rabbit | 1, 18, 24 |
| 32 | Gloomy Humbug | 4, 9 |
| 33 | Mama Bear | 16 |
| 33 | Skullo | 61, 62 |
| 34 | Tarantula | 62 |
| 35 | Giant Rascal Rabbit | 4, 24 |
| 36 | Warrior Piya | 10, 12 |
| 39 | Abyss | 61, 62 |

### Equipment
| Level | Item | Slot |
|---|---|---|
| 27–37 | Grizzly Cleaver.G | ARMOR |
| 30–40 | Cat's Mace.G | ACCESSORY |
| 30–40 | Curved Knife.G | HELMET |
| 35–45 | Bouquet | ACCESSORY |
| 35–45 | Ghost's Sword.G | ARMOR |
| 39–49 | Ogre's Pick.G | SHIELD |

### Next Step
Lanjut ke L40+. Band PROGRESSION — map menengah-atas.

---

## Level 40–49 (PROGRESSION)

### Tujuan
High monster, map menengah-atas, equipment yang lebih kuat.

### Pergi ke
- **Map 15** — Western Laywook Forest (L35–L47, L65–L81, **CANDIDATE**)
- **Map 16** — Eastern Laywook Forest (L26–L47, **CANDIDATE**)
- **Map 61** — Clement Mine (L51–L109, **CANDIDATE**)

### Hunting
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

### Equipment
| Level | Item | Slot |
|---|---|---|
| 41–51 | Staff of Awakening.G | BOOTS |
| 43–53 | Dusk Shield.DG | CAPE |
| 45–55 | Neptune Mace.G | ACCESSORY |
| 45–55 | Dusk Blade.G | ARMOR |
| 45–55 | Golden Bowie.G | HELMET |
| 45–55 | Dusk Great Sword.G | SHIELD |

### Next Step
Lanjut ke L50+. Band LATE — endgame preparation.

---

## Level 50–59 (LATE)

### Tujuan
Endgame preparation, rare monster, equipment premium.

### Pergi ke
- **Map 62** — Crude Dungeon (L30–L52, **CANDIDATE**)
- **Map 61** — Clement Mine (L51–L109, **CANDIDATE**)
- **Map 33** — Mt. Trevia (L52–L61, **CANDIDATE**)
- **Map 66** — Aleph Silon (L39–L80, **CANDIDATE**)
- **Map 75** — West of Sealed Island (L74–L80, **CANDIDATE**)

### Hunting
| Level | Monster | Map |
|---|---|---|
| 50 | Tarantula Queen | 62 |
| 51 | Steel Golem | 1, 31 |
| 52 | Samba Cactus | 33 |
| 52 | Servanguy | 36 |
| 55 | Knight of Darkness | 1 |
| 55 | Super Steel Golem | 66 |
| 58 | Sandy Windy | 33, 75 |

### Equipment
| Level | Item | Slot |
|---|---|---|
| 48–58 | Large Pumpkin's Shield | CAPE |
| 55–65 | Sieve | ACCESSORY |
| 55–65 | Cool Guy's Stick.G | BOOTS |
| 55–65 | Shooting Star.DG | CAPE |
| 55–65 | Korean Yut | HELMET |
| 55–65 | Spiked Bat | SHIELD |

### Next Step
Lanjut ke L60+. Band ENDGAME — map premium, monster kuat.

---

## Level 60–74 (ENDGAME)

### Tujuan
Map premium, monster kuat, equipment endgame.

### Pergi ke
- **Map 14** — Herakus Forest (L42–L91, **CANDIDATE**)
- **Map 15** — Western Laywook Forest (L65–L81, **CANDIDATE**)
- **Map 61** — Clement Mine (L51–L109, **CANDIDATE**)
- **Map 66** — Aleph Silon (L70–L105, **CANDIDATE**)
- **Map 34** — Lake Cross (L64–L73, **CANDIDATE**)
- **Map 68** — Forest of Death (L153–L174, **CANDIDATE**)

### Hunting
| Level | Monster | Map |
|---|---|---|
| 60 | Soul Collector | 68, 93 |
| 61 | Meow-gician | 33 |
| 64 | Saloa | 34, 76 |
| 65 | Computer Freak | 15 |
| 66 | Succubus | 14, 15, 76 |
| 69 | Solar | 29, 30 |
| 70 | Aleph | 66, 74, 98 |
| 72 | Bubble Dragon | 29, 30, 82 |

### Equipment
Data tidak tersedia di band ini. Equipment progression tercatat sampai L55–65.

### Next Step
Lanjut ke L75+. Band LEGEND — map tertinggi, monster elite.

---

## Level 75–100 (LEGEND)

### Tujuan
Map tertinggi, monster elite, endgame content.

### Pergi ke
- **Map 74** — Sealed Cave (L81–L91, **CANDIDATE**)
- **Map 76** — East of Sealed Island (L63–L72, **CANDIDATE**)
- **Map 29** — Downstream of Glasis River (L83–L108, **CANDIDATE**)
- **Map 30** — Glasis Plains (L80–L105, **CANDIDATE**)
- **Map 43** — Mt. Cross (L92–L120, **CANDIDATE**)
- **Map 69** — Catacombs (L120–L190, **CANDIDATE**)
- **Map 98** — Esdelron Lake (L184–L220, **CANDIDATE**)
- **Map 93** — Dungeon of Death (L194–L212, **CANDIDATE**)

### Hunting
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
| 92 | Afro Rock | 43 |
| 95 | Pedron | 69, 98 |
| 96 | Notorsonia | 43 |
| 98 | Ghost Midnight | 66 |

### Equipment
Data tidak tersedia di band ini. Equipment progression tercatat sampai L55–65.

---

## Known NPCs

| NPC ID | Name | Evidence |
|---|---|---|
| 4429 | Joan | `PROBABLE` — from `monsters.edt` |
| 4441 | Arus | `PROBABLE` — from `monsters.edt` |
| 5288 | Duran | `PROBABLE` — from `monsters.edt` |
| 5690 | Hanaiel | `PROBABLE` — from `monsters.edt` |

**Total:** 1,928 NPC identities, 293 named, 1,635 unresolved.
**Locations:** 1,300 placements across 98 maps, 51 unique NPC IDs.

NPC placement ≠ quest giver. Hubungan Quest → NPC = `PROBABLE` (3,674 candidates dievaluasi; 1,235 valid rows; 94 NPC IDs dengan textual evidence; multi-layer corroboration untuk 10 NPC). Runtime consumer belum ditemukan — player-facing walkthrough belum menggunakannya sebagai fakta absolut.

---

## Skill System

SkillFile v7 berhasil diparse dan dipetakan secara semantic.

Status (dengan evidence level):
- 362 skill records per variant file (344 named + 18 unnamed) — `BINARY_CONFIRMED`
- 20 variant files — structure `BINARY_CONFIRMED`; variant meaning (level/rank progression) `PROBABLE`
- prerequisite relationships (field[2]/field[3]) — `PROBABLE`
- max skill level (field[4]) — `PROBABLE`
- minimum level requirements (field[6]) — `PROBABLE`
- skill point values (field[5]) — `PROBABLE`
- buff links field[23]/field[27] → buff.edt — `BINARY_CONFIRMED` cross-table joins

Detail lengkap: `docs/SKILL_V7_SEMANTIC_RESEARCH.md`

---

## Skill Progression

Data client menunjukkan skill saling terhubung melalui prerequisite: sebagian besar skill hanya bisa dipelajari setelah skill tertentu mencapai level tertentu. Struktur ini membentuk skill tree per class/job.

### Cara Membaca

Setiap chain dibaca dari atas (skill pembuka) ke bawah. Angka di samping panah adalah level prerequisite-nya — skill di bawah baru terbuka setelah skill di atasnya mencapai level tersebut.

Contoh (Mage):

```
Source of Universe Lv5
→ Fireball Lv1
→ Firestorm Lv1
→ Hell Burn Lv5
→ Meteor
```

Artinya: Fireball membutuhkan Source of Universe Lv5. Firestorm membutuhkan Fireball Lv1. Hell Burn membutuhkan Firestorm Lv5. Meteor membutuhkan Hell Burn Lv10.

Interpretasi prerequisite berstatus `PROBABLE` (semantic mapping dari cross-build schema); struktur graph-nya sendiri adalah hasil pembacaan data client secara langsung.

### Prerequisite Bersama

Beberapa branch class menggunakan **Sleep** dan **Martial Combo** sebagai prerequisite bersama sebelum masuk ke branch skill masing-masing. Hampir seluruh job tree bermula dari kedua skill ini — Sleep memiliki 16 skill turunan langsung, Martial Combo 13. Setelah itu, setiap branch berjalan di dalam category-nya sendiri.

### Skill Path per Class

Chain berikut berasal langsung dari prerequisite graph (satu chain representative per class; chain tambahan hanya untuk branch yang berbeda secara material):

**Warrior**
```
Sleep
→ Martial Combo Lv1
→ Great Sword Combo Lv5
→ Concentration Lv3
→ Quick Slash Lv3
→ Double Slash Lv3
→ Deadly Slash Lv10
```

**Knight**
```
Sleep
→ Martial Combo Lv1
→ Sword Combo Lv5
→ Chivalry Lv3
→ Holy Cross Lv3
→ Grand Cross Lv3
→ Holy Punishment Lv5
→ Deadly Cross Lv10
```

**Jester**
```
Sleep
→ Martial Combo Lv1
→ Knife Combo Lv5
→ Passion Lv5
→ Single Jumping Dagger Lv3
→ Dual Jumping Dagger Lv10
```

**Mage — cabang fire**
```
Sleep
→ Martial Combo Lv1
→ Source of Universe Lv5
→ Fireball Lv1
→ Firestorm Lv1
→ Hell Burn Lv5
→ Meteor Lv10
```

**Mage — cabang ice**
```
Sleep
→ Martial Combo Lv1
→ Source of Universe Lv5
→ Frostbolt Lv1
→ Ice Drill Lv1
→ Ice Dew Lv5
→ Blizzard Lv10
```

**Priest — jalur healing**
```
Sleep
→ Martial Combo Lv1
→ Prayer Lv5
→ Self Cure Lv1
→ Cure Lv5
→ Mass Cure Lv5
→ Cleansing Lv5
→ Revival Lv5
```

**Craftsman — jalur tempur**
```
Sleep
→ Martial Combo Lv1
→ Blacksmiths Ingenuity Lv5
→ Deadly Blow Lv1
→ Deadly Smash Lv5
→ Demolition Lv5
```

**Craftsman — jalur produksi**
```
Sleep
→ Martial Combo Lv1
→ Blacksmiths Ingenuity Lv5
→ Weaponry Lv1
→ Refine Weapon Lv5
```

Skill produksi (Weaponry, Armory, Accessory Production, dan refine-nya) memiliki maximum level 20 — tertinggi di dataset.

**Hunter**
```
Sleep
→ Martial Combo Lv1
→ Slingshot Combo Lv1
→ Training Lv5
→ Simple Shot Lv5
→ Point Shot Lv5
→ Power Shot Lv5
→ Eye Sight Lv5
→ Sharp Eye Lv5
```

Chain Hunter merupakan salah satu chain terdalam di game (depth 9).

**Archer**
```
Piercing Arrow
→ Bump Arrow Lv3
→ Broken Arrow Lv3
→ Enhance Bow Lv3
→ Triple Arrow Lv1
```

**Gunner**
```
Aimed Shot
→ Kill Shot Lv3
→ Encounter Shot Lv3
→ Reload Lv3
```

Cabang lain: Aimed Shot → Kill Shot Lv3 → Headshot Lv3 → Enhance Gun Lv3.

**Chef**
```
Sleep
→ Martial Combo Lv1
→ Poke Combo Lv3
→ Table Manner Lv3
→ Absolute Taste Lv3
→ Appetizer Soup Lv1
→ Korean Dish - Spicy Taste Lv5
→ Japanese Dish - Wasabi Onigiri Lv5
→ Chinese Dish - Fire Taste Lv3
```

Chain Chef juga mencapai depth 9. Cabang lain (Great Delicacies: Foie Gras → Caviar → Truffle) berpisah dari Appetizer Soup.

Sebagian category skill (7–8, 11–16, 21–26) belum memiliki nama class yang terkonfirmasi — chain-nya tersedia di `docs/SKILL_TREE_RESEARCH.md` tanpa penamaan job.

### Level Requirement Skill

Minimum character level untuk mempelajari skill bisa berubah antar variant file. Data menunjukkan tiga pola:

- **Tetap** — requirement tidak berubah (contoh: Fireball selalu Lv10)
- **Naik lalu berhenti** — requirement naik dari file 01–10 lalu plateau
- **Naik lalu reset lebih rendah** — requirement naik di file 01–10, lalu turun kembali di file 11–20

Contoh pola reset (data client):

```
Quick Slash:  11 → 14 → 17 → ... → 38  (file 01–10)
              kembali ke 10             (file 11–20)

Double Slash: 25 → 28 → 31 → ... → 50  (file 01–10)
              kembali ke 23             (file 11–20)
```

Struktur ini mengindikasikan adanya **second progression track** pada file 11–20 — interpretasi ini masih `PROBABLE`; makna gameplay persisnya belum dikonfirmasi.

### Maximum Skill Level

Skill memiliki maximum level yang berbeda:

| Max Level | Jumlah Skill |
|---:|---:|
| 1 | 62 |
| 5 | 200 |
| 10 | 83 |
| 20 | 8 |

Mayoritas skill bermaks level 5 atau 10. Hanya skill produksi Craftsman (dan satu record tanpa nama) yang mencapai 20. Data ini berasal dari client; korelasi dengan file `uskill01–20.edt` masih `PROBABLE` dan belum dikonfirmasi sebagai runtime source.

### Skill Point

Setiap skill memiliki nilai skill point (range 0–54, terbanyak 6 SP). Dua skill — Throw Bomb dan Alchemy — membawa nilai khusus 9999 yang kemungkinan merupakan konfigurasi khusus, bukan nilai normal. Mekanisme spending SP (per level atau per purchase) belum terkonfirmasi.

Detail lengkap graph, semua chain, dan evidence classification: `docs/SKILL_TREE_RESEARCH.md`

Player-facing build/recommendation belum dibuat. Walkthrough saat ini menampilkan dependency/progression structure, bukan rating atau recommended build.

---

## Unresolved / Deferred

| Area | Status | Reason |
|---|---|---|
| Quest chain | `UNRESOLVED` | No flag transition logic found in binary |
| Quest → NPC semantic | `PROBABLE` | Multi-layer corroboration (text + dialog + location); runtime consumer not found — not used as absolute fact in walkthrough |
| Quest → Monster | `UNRESOLVED` | No exact reference in quest.edt binary |
| Drop table | `UNRESOLVED` | `drop.py` schema exists, no actual file in client |
| DropRate | `DEFERRED` | Not investigated |
| Map connection | `UNRESOLVED` | No teleporter/warp data in binary |
| Quest objective | `UNRESOLVED` | action_id semantics partial |
| Skill binary structure | COMPLETE | 362 records/file × 20 files; fixed 38-field layout; chain parse exact EOF 20/20 |
| Skill semantic mapping | SUBSTANTIALLY DECODED | buff.edt joins 103/103 + 20/20 `BINARY_CONFIRMED`; prereq/element/projectile/variant `PROBABLE` (cross-build v8 schema; direct v7 loader NOT FOUND) |
| Skill tree integration | COMPLETE | Prerequisite graph built (362 nodes, 284 edges, 0 cycles); representative chains integrated into WALKTHROUGH.md |
| Player skill recommendations | NOT YET BUILT | Chain structure available in walkthrough; build/rating recommendations require gameplay evidence |
| 1,635 NPC names | `UNRESOLVED` | Tidak ditemukan di `monsters.edt` atau string table |

---

## Evidence Legend

| Label | Meaning |
|---|---|
| `BINARY_CONFIRMED` | Langsung dari client binary |
| `CLIENT_FACT` | Dari client, parsed facts |
| `DERIVED` | Dihitung dari data lain |
| `PROBABLE` | Correlation, no consumer runtime |
| `EXTERNAL_REFERENCE` | Wiki/community/game knowledge |
| `UNRESOLVED` | Tidak ada evidence |
| `DEFERRED` | Tidak dikerjakan |

---

## Canonical Numbers

| Table | Count |
|---|---|
| monsters | 9,999 |
| items | 16,318 |
| quest_identity | 717 |
| quest_dialog_nodes | 39,950 |
| npc_identity | 1,928 |
| resolved NPC | 293 |
| unresolved NPC | 1,635 |
| npc_locations | 1,300 |
| npc_dialog | 793 |
| monster_progression | 5,161 |
| map_progression_candidates | 40 |
| map_progression_graph | 1,247 |
| equipment_progression | 125 |
