# Skill Tree Research

## Executive Summary

Research ini membangun dan memvalidasi prerequisite graph dari dataset Skill v7 menggunakan 362 skill records yang tersedia.

Hasil akhirnya:

* **362 nodes**
* **284 prerequisite edges**
* **344 named records**
* **18 unnamed records**
* **77 named root skills**
* **178 leaf skills**
* **Maximum graph depth: 9**
* **0 cycles**
* **0 broken references**
* **0 self references**

Seluruh 18 unnamed records tetap dipertahankan di dalam graph dan tidak dibuang dari dataset.

Struktur prerequisite menghasilkan **directed acyclic graph (DAG)** yang bersih. Tiga referensi menuju unnamed records ditemukan pada chain crafting/placeholder:

* `98 → 97`
* `99 → 98`
* `226 → 225`

Makna gameplay beberapa field tetap diklasifikasikan secara konservatif. Struktur graph merupakan hasil derivasi langsung dari data; semantic interpretation untuk prerequisite, category, level, dan variant tetap **PROBABLE** karena direct v7 runtime consumer belum ditemukan.

---

## 1. Dataset

| Metric            |              Result |
| ----------------- | ------------------: |
| Nodes             |                 362 |
| Edges             |                 284 |
| Named records     |                 344 |
| Unnamed records   |                  18 |
| Root skills       | 77 named / 78 total |
| Leaf skills       |                 178 |
| Maximum depth     |                   9 |
| Cycles            |                   0 |
| Broken references |                   0 |
| Self references   |                   0 |

Semua 362 skill records dipertahankan dalam graph, termasuk 18 unnamed records.

### Data Integrity

Dataset berasal dari parser Skill v9 yang sebelumnya telah memvalidasi:

* 362 records per file
* ID `1–362`
* fixed 38-field record layout
* exact EOF
* total 7,240 records across `skill01.edt`–`skill20.edt`

Skill tree research menggunakan dataset tersebut secara read-only.

---

## 2. Prerequisite Graph

Graph dibangun berdasarkan hubungan prerequisite yang berasal dari:

* `skill_id`
* `field[2]` → prerequisite skill ID
* `field[3]` → prerequisite skill level

Semantic mapping field tersebut diklasifikasikan sebagai **PROBABLE**, sedangkan struktur edge yang dihasilkan dari data diklasifikasikan sebagai **DERIVED**.

### Graph Validation

Hasil validasi graph:

```text
Cycles:             0
Broken references:  0
Self references:    0
```

Dengan demikian graph hasil parsing membentuk DAG tanpa cycle, tanpa referensi ke skill ID yang tidak tersedia, dan tanpa self-reference.

### Unnamed Prerequisite References

Terdapat tiga hubungan prerequisite yang berakhir pada unnamed records:

```text
98  → 97
99  → 98
226 → 225
```

Chain tersebut dipertahankan karena unnamed record tetap merupakan bagian dari dataset.

Interpretasi gameplay terhadap unnamed records belum ditentukan.

### Cross-Category Edges

Terdapat 30 cross-category edges:

* 28 bersifat struktural (job trees berakar pada utility category 0: Sleep, Martial Combo)
* 1: Bless (cat 7) → Encourage (cat 5)
* 1: unnamed #70 (sentinel) → Sleep (cat 0)

Tidak ada yang merupakan error referensi.

---

## 3. Root Skills

Graph menghasilkan:

```text
77 named root skills
78 root nodes including unnamed
```

Root berarti node yang tidak mempunyai prerequisite edge di dalam graph.

Root tidak selalu berarti "skill pertama yang dibeli pemain". Status tersebut hanya menunjukkan posisi node pada prerequisite graph.

---

## 4. Leaf Skills

Jumlah leaf nodes:

```text
178
```

Leaf merupakan skill yang tidak menjadi prerequisite bagi node lain di dalam graph.

Leaf status adalah property struktural graph dan tidak secara otomatis berarti skill tersebut adalah skill akhir, skill terbaik, atau skill wajib.

---

## 5. Category / Job Groups

Distribusi category yang berhasil dipetakan:

| Category              | Records | Roots | Edges |
| --------------------- | ------: | ----: | ----: |
| 0 (utility)           |      12 |     4 |     8 |
| 1 Warrior             |      18 |     1 |    17 |
| 2 Knight              |      18 |     1 |    17 |
| 3 Jester              |      17 |     1 |    16 |
| 4 Mage                |      25 |     1 |    24 |
| 5 Priest              |      27 |     4 |    23 |
| 6 Craftsman           |      20 |     1 |    19 |
| 9 Hunter-related      |      18 |     1 |    17 |
| 19 Archer-related     |      11 |     5 |     6 |
| 29 Gunner-related     |      13 |     4 |     9 |
| 31/131/231 Chef       |      35 |     5 |    30 |
| Cat 7,8,11–16,21–26   |    ~100 |   ~30 |   ~70 |
| `0xFFFFFFFF` sentinel |      21 |     2 |    18 |

Category IDs yang sudah memiliki mapping dikenal tetap digunakan sebagai referensi data.

Untuk category:

```text
7–8
11–16
21–26
```

nama job belum dikonfirmasi dan tidak diberi nama baru berdasarkan asumsi.

Category `31/131/231` dikelompokkan sebagai Chef sesuai mapping yang digunakan pada research dataset.

---

## 6. Important Skill Chains

Berikut beberapa prerequisite chain yang telah diverifikasi dari graph.

### Mage — Fire

```text
Sleep Lv1
→ Martial Combo Lv5
→ Source of Universe Lv5
→ Fireball Lv1
→ Firestorm Lv1
→ Hell Burn Lv5
→ Meteor Lv10
```

Maximum depth pada chain ini menunjukkan rangkaian prerequisite bertingkat hingga skill Meteor.

### Priest — Heal

```text
Sleep
→ Martial Combo Lv5
→ Prayer Lv1
→ Self Cure Lv5
→ Cure Lv5
→ Mass Cure Lv5
→ Cleansing Lv5
→ Revival
```

Chain ini memiliki depth 8.

### Knight — Holy

```text
Sleep
→ Martial Combo Lv5
→ Sword Combo Lv3
→ Chivalry Lv3
→ Holy Cross Lv3
→ Grand Cross Lv5
→ Holy Punishment Lv10
→ Deadly Cross
```

Chain ini juga mencapai depth 8.

### Hunter

```text
Sleep
→ Martial Combo Lv1
→ Slingshot Combo Lv5
→ Training Lv5
→ Simple Shot Lv5
→ Point Shot Lv5
→ Power Shot Lv5
→ Eye Sight Lv5
→ Sharp Eye
```

Chain Hunter merupakan salah satu chain terdalam dengan:

```text
Maximum depth = 9
```

### Chef

```text
Sleep Lv1
→ Martial Combo Lv3
→ Poke Combo Lv3
→ Table Manner Lv3
→ Absolute Taste Lv1
→ Appetizer Soup Lv5
→ Korean Dish Lv5
→ Japanese Dish Lv3
→ Chinese Dish – Fire Taste
```

Chain Chef juga mencapai depth 9.

### Craftsman — Production

```text
Blacksmiths Ingenuity
→ Weaponry Lv1
→ Refine Weapon Lv5
```

`Refine Weapon` memiliki `max_level = 20`.

---

## 7. Universal Prerequisite Structure

Dari graph ditemukan dua root yang memiliki branching sangat besar:

```text
Sleep
Martial Combo
```

Observasi graph menunjukkan:

* Sleep memiliki **16 children**
* Martial Combo memiliki **13 children**

Pola ini menunjukkan adanya dua node prerequisite bersama sebelum banyak branch job-specific.

Dari sisi graph structure, kedua skill tersebut berfungsi sebagai shared prerequisite nodes.

Makna gameplay universal atau alasan desain sistem belum ditetapkan di luar evidence graph.

---

## 8. Prerequisite Level

Nilai prerequisite level yang ditemukan berada pada lima nilai:

```text
0
1
3
5
10
```

Empat nilai utama (1, 3, 5, 10) membentuk gating prerequisite yang sangat konsisten.

Satu pengecualian: **Throw Bomb** (ID 83) memiliki `prereq_level = 0` terhadap Blacksmiths Ingenuity — skill yang sama juga membawa `SP = 9999` (lihat §12), sehingga keduanya kemungkinan bagian dari konfigurasi khusus yang sama.

Nilai ini berasal dari interpretasi `field[3]` sebagai prerequisite skill level dan tetap berstatus **PROBABLE**.

---

## 9. Minimum Level Requirement

Analisis `field[6]` dilakukan lintas seluruh 20 variant files.

Hasil distribusi:

```text
117 skills  = constant
176 skills  = progress → plateau
69 skills   = progress → reset-lower
```

### Constant

Sebagian skill mempertahankan minimum level yang sama sepanjang variant.

### Progress → Plateau

Sebagian skill mengalami peningkatan minimum level selama progression awal kemudian mencapai plateau.

### Progress → Reset-Lower

Sebanyak **69 skills** menunjukkan pola:

```text
minimum level naik pada files 01–10
↓
reset ke nilai lebih rendah pada files 11–20
```

Contoh:

```text
Quick Slash
11 → 38 → 10

Double Slash
25 → 50 → 23
```

Pola reset-lower merupakan salah satu bukti struktural terkuat untuk keberadaan **second progression track**.

Interpretasi gameplay pasti dari track tersebut masih belum dikonfirmasi.

---

## 10. Maximum Skill Level

Distribusi `field[4]`:

| Max Level | Number of Skills |
| --------: | ---------------: |
|         1 |               62 |
|         5 |              200 |
|        10 |               83 |
|        20 |                8 |

### Max Level 1

Sebanyak 62 skill memiliki maximum level 1 dan terutama muncul pada skill/utility yang tidak memakai progression multi-level seperti skill biasa.

### Max Level 5

Kategori terbesar:

```text
200 skills
```

### Max Level 10

Sebanyak 83 skills mempunyai maximum level 10.

Nilai tersebut selaras secara struktural dengan:

```text
skill01.edt
...
skill10.edt
```

yang digunakan sebagai progression awal.

### Max Level 20

Hanya 8 records yang mempunyai `max_level = 20`.

Records tersebut berkorelasi dengan craft production family:

* Weaponry
* Refine Weapon
* Refine Equipment
* Refine Accessory
* Accessory Production
* Cure
* unnamed record #95

Hubungan dengan `uskill01–20.edt` menunjukkan korelasi terhadap extended 20-level family, tetapi runtime usage belum terbukti.

---

## 11. Skill Variant Structure

Dataset terdiri dari 20 variant files:

```text
skill01.edt
...
skill20.edt
```

Interpretasi variant saat ini:

```text
Files 01–10
= base progression

Files 11–20
= second progression track

uskill01–20
= extended family
```

Status semantic:

```text
PROBABLE
```

### Files 01–10

Pada track pertama ditemukan beberapa pola:

* minimum level meningkat
* power/damage meningkat
* skill progression mengikuti level variant

### Files 11–20

Tidak semua record melanjutkan progression secara linear.

Sebagian skill mengalami:

```text
progression
→ reset
→ lower requirement
→ renewed progression
```

Disertai sejumlah perubahan parameter lain.

Karena itu files 11–20 tidak diperlakukan sebagai sekadar "level 11–20".

Istilah seperti:

```text
mastery
awakening
rebirth
second class
```

tidak digunakan sebagai fakta karena belum ada runtime evidence yang mengonfirmasi terminologi tersebut.

### uskill01–20

Cross-build evidence menunjukkan family ini memiliki karakteristik:

* `max_level = 20`
* SP lebih murah pada data tertentu
* offset record berbeda (mulai @261, bukan @260)
* tetap menggunakan 362 skill IDs

Makna gameplay exact dari `uskill` family belum dikonfirmasi.

---

## 12. Skill Point Cost

`field[5]` dianalisis sebagai kandidat skill point cost.

Nilai tersebut digunakan sebagai **PROBABLE** karena semantic mapping field berasal dari cross-build evidence dan belum memiliki direct v7 loader.

Distribusi umum:

```text
Range:     0–54 (excluding sentinels)
Mode:      6 SP (57 skills)
Typical:   1–10 SP untuk mayoritas skill; 12–54 untuk advanced/production
```

### Special Case: SP = 9999

Ditemukan:

```text
Throw Bomb
Alchemy
```

dalam category 7 dengan:

```text
SP = 9999
```

Nilai tersebut dapat menunjukkan status khusus seperti skill yang tidak intended untuk normal progression, tetapi penyebab pastinya belum dikonfirmasi.

Karena itu:

```text
SP = 9999
```

tidak diberi interpretasi "disabled" sebagai fakta.

Catatan tambahan: Throw Bomb juga merupakan satu-satunya skill dengan `prereq_level = 0` (§8) — dua anomali pada record yang sama memperkuat kemungkinan konfigurasi khusus, tetapi tetap tidak dinaikkan menjadi fakta.

---

## 13. Craftsman Production Root

`Blacksmiths Ingenuity` merupakan salah satu root production node dengan branching besar.

Node tersebut memiliki:

```text
11 children
```

dan menjadi dependency bagi chain crafting berikutnya, termasuk Weaponry dan Refine Weapon.

Karena struktur tersebut berasal langsung dari prerequisite graph, hubungan ini diklasifikasikan sebagai **DERIVED**.

---

## 14. Sentinel / Special Records

Dataset memiliki:

```text
21 records with field[0] = 0xFFFFFFFF
```

Tetapi kategori sentinel tersebut tidak seluruhnya unnamed.

Komposisinya:

```text
18 unnamed records
3 named special records
```

Tiga named special records tersebut antara lain:

* Seal Online
* Unknown Skill
* Royal Food

Semua records tersebut tetap dipertahankan dalam dataset dan graph.

---

## 15. Cycles and Broken References

Validation menghasilkan:

```text
Cycles:            0
Broken references: 0
Self references:   0
```

Tidak ditemukan prerequisite chain yang kembali ke node sebelumnya.

Tidak ditemukan edge yang mengarah ke skill ID di luar dataset.

Tidak ditemukan skill yang menunjuk dirinya sendiri sebagai prerequisite.

Hasil ini memperkuat bahwa prerequisite graph hasil parsing konsisten secara struktural.

---

## 16. Evidence Classification

Research menggunakan klasifikasi evidence berikut:

### BINARY_CONFIRMED

Digunakan ketika struktur atau nilai dapat dibuktikan langsung dari binary/data.

Contoh:

* 362 records per skill file
* 38-field fixed layout
* skill IDs `1–362`
* exact EOF
* field values yang dibaca langsung

### DERIVED

Digunakan untuk hasil komputasi dari dataset.

Contoh:

* graph node count
* edge count
* root detection
* leaf detection
* maximum depth
* cycle detection
* broken-reference detection
* parent frequency
* chain construction

### PROBABLE

Digunakan ketika makna field atau relationship didukung oleh cross-build schema dan data corroboration tetapi belum memiliki direct v7 runtime consumer.

Contoh:

* prerequisite semantic
* category/job mapping
* minimum level meaning
* maximum skill level meaning
* skill point interpretation
* variant interpretation
* `uskill` extended family

### UNRESOLVED

Digunakan untuk hal yang belum dapat ditentukan dengan evidence yang cukup.

Contoh:

* exact gameplay meaning files 11–20
* exact purpose of `SP = 9999`
* direct runtime usage of `uskill01–20`
* exact job names for unresolved categories
* direct v7 consumer

---

## 17. Remaining Unknowns

Research masih memiliki beberapa unresolved areas.

### 1. Job names

Nama job untuk:

```text
Category 7–8
Category 11–16
Category 21–26
```

belum dikonfirmasi.

Ada kemungkinan category tersebut berkaitan dengan advanced classes, tetapi hal tersebut belum ditetapkan sebagai fakta.

### 2. Gameplay meaning of files 11–20

Structural evidence mendukung keberadaan second progression track, terutama melalui 69 records dengan reset-lower minimum level.

Gameplay terminology dan fungsi persis track tersebut masih unresolved.

### 3. Runtime usage of max_level = 20

Belum diketahui dengan pasti apakah seluruh craft skills dengan `max_level = 20` menggunakan `uskill01–20.edt` secara langsung pada runtime.

### 4. Skill point spending mechanic

Dataset menunjukkan SP-related values, tetapi mekanisme spending:

```text
per level
per purchase
atau mekanisme lain
```

belum terkonfirmasi.

### 5. SP = 9999

Alasan penggunaan `9999` pada Throw Bomb dan Alchemy belum diketahui.

### 6. Direct v7 Consumer

Direct runtime consumer untuk Skill v7 belum ditemukan karena executable terkait packed dan direct runtime inspection belum tersedia.

---

## 18. Method

Research dilakukan dari dataset Skill v9 yang sebelumnya telah melalui fixed-layout parser validation.

Tahapan utama:

1. Load 362 skill records.
2. Preserve named dan unnamed records.
3. Read prerequisite relationship dari skill record.
4. Build directed graph.
5. Resolve prerequisite names.
6. Detect roots dan leaves.
7. Calculate graph depth.
8. Detect cycles.
9. Validate broken references.
10. Detect self references.
11. Group records berdasarkan category.
12. Analyze prerequisite levels.
13. Analyze `field[6]` minimum-level progression lintas 20 variants.
14. Analyze `field[4]` maximum skill level.
15. Analyze `field[5]` skill point values.
16. Compare files 01–10 dan 11–20.
17. Preserve uncertain semantics as PROBABLE / UNRESOLVED.
18. Export graph edge dataset.

Graph export:

```text
skill_tree_edges.csv
```

dengan satu row untuk setiap skill node yang dianalisis.

---

## 19. Research Artifacts

### Published Research

```text
docs/SKILL_TREE_RESEARCH.md
```

### Local Research Export

```text
research/skill_tree_edges.csv
```

File tersebut tetap mengikuti repository policy dan berada dalam gitignored research output.

### Canonical Local Export

```text
D:\SealR_Database\skill_tree_edges.csv
```

### Synchronized Research Copy

```text
D:\SealR_Database\skill_tree_research.md
```

---

## 20. Integrity Validation

Hasil integrity check:

```text
Parser v9 unchanged:          PASS
skill_v9_data.pkl unchanged:  PASS
skill_v7_parsed.csv unchanged: PASS
Canonical SQLite unchanged:   PASS
WALKTHROUGH unchanged:        PASS
Other project docs unchanged: PASS
```

---

## 21. Git / Repository Validation

Commit awal research:

```text
4b0639b74675e11b34288ad72a6f9de96a3edbc2
```

Push:

```text
SUCCESS
```

GitHub API verification:

```text
11 / 11 content checks: PASS
```

---

## 22. Conclusion

Skill Tree Research berhasil menghasilkan prerequisite graph lengkap dari seluruh 362 skill records.

Hasil struktural utama:

```text
362 nodes
284 edges
77 named roots
178 leaves
max depth 9
0 cycles
0 broken references
0 self references
```

Graph mempertahankan seluruh 18 unnamed records dan menghasilkan prerequisite chains yang konsisten lintas kategori.

Evidence terkuat dari research adalah struktur DAG itu sendiri serta pola progression minimum-level pada 69 skills yang mengalami **progress → reset-lower** di files 11–20.

Namun semantic gameplay tetap dibatasi oleh belum ditemukannya direct v7 runtime consumer. Karena itu interpretasi seperti prerequisite meaning, job/category mapping, two-track progression, dan `uskill` relationship tetap dipertahankan sebagai **PROBABLE**, bukan fakta runtime.

Research ini menjadi dasar data untuk tahap berikutnya: **integrasi skill tree ke player-facing walkthrough**.
