# Seal Return Walkthrough / Player Progression Guide

## Purpose

This repository provides **practical walkthrough guidance** for players of **Seal Online Return v5** (sealrv5.com private server).

**Goal:** Answer the question:
> "I'm level X, job Y. What should I do now?"

## What This Repository Contains

- Player progression guide (level 1-100)
- Walkthrough steps
- Map progression candidates
- Equipment milestones
- Monster classification for hunting
- Known gaps and limitations

## Research Source

All data in this repository was extracted and validated from:

**`D:\SealR_Database`** — Full research environment containing:
- Raw client binary analysis (SPAK/EDT)
- SQLite database (9,999 monsters, 16,318 items, 717 quests)
- Binary schemas and parsers
- Research documentation and methodology
- Wiki cross-references

The full research environment remains at `D:\SealR_Database`. This repository contains only curated, player-facing outputs.

## Project Structure

```
Walktrhough-Retrun/
├── README.md              # This file
├── WALKTHROUGH.md         # Main walkthrough (Level 1-100 progression)
├── CHANGELOG.md           # Project changelog
├── .gitignore             # Excludes large/legacy files
└── docs/
    ├── STATUS.md          # Current validation status
    ├── PROJECT_SCOPE.md   # Project scope and methodology
    ├── EVIDENCE_POLICY.md # Evidence hierarchy and confidence
    ├── RESEARCH_FINDINGS.md # Confirmed facts
    ├── KNOWN_GAPS.md      # Unresolved areas
    ├── PLAYER_PROGRESSION_MODEL.md  # Progression architecture
    ├── PLAYER_PROGRESSION_GAPS.md   # Remaining gaps
    ├── LEVELING_AREA_ANALYSIS.md    # Leveling zone analysis
    ├── MAP_PROGRESSION_REPAIRED.md  # Map progression
    ├── MONSTER_CLASSIFICATION.md    # Monster categories
    ├── EQUIPMENT_PROGRESSION.md     # Equipment milestones
    ├── EQUIPMENT_CLASSIFICATION_FINAL.md # Item types
    ├── NPC_IDENTITY_REPAIR.md       # NPC names
    ├── NPC_IDENTITY_EXECUTION.md    # NPC execution report
    ├── ITEM_ACQUISITION.md          # How to get items
    ├── SKILL_JOB_PROGRESSION.md     # Job/skill info
    └── WALKTHROUGH_ARCHITECTURE.md  # Walkthrough structure
```

## Key Facts

| Fact | Value | Evidence |
|---|---|---|
| MonsterID 1 | Piya (Level 1) | `BINARY_CONFIRMED` |
| MonsterID 22 | Rascal Rabbit (Level 8) | `BINARY_CONFIRMED` |
| MonsterID 23 | Rascal Rabbit (Level 8) | `BINARY_CONFIRMED` |
| NPC Joan | ID 4429 | `PROBABLE` |
| NPC Arus | ID 4441 | `PROBABLE` |
| NPC Duran | ID 5288 | `PROBABLE` |
| NPC Hanaiel | ID 5690 | `PROBABLE` |

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

## Not Included

- Raw client files (SPAK/EDT)
- Extracted/decoded binary data
- SQLite database (>100MB)
- Bulk CSV datasets
- Wiki raw responses
- 7z snapshots

These remain in `D:\SealR_Database`.

## License

Research project for Seal Online Return v5 private server.
