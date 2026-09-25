# Changelog

## 2026-09-26

### Walkthrough Expansion
- Expanded WALKTHROUGH.md from early-game only to full L1-L100 progression
- Added evidence labels (`BINARY_CONFIRMED`, `CLIENT_FACT`, `DERIVED`, `PROBABLE`, `EXTERNAL_REFERENCE`, `UNRESOLVED`, `DEFERRED`)
- Added equipment milestone tables per level band
- Added monster hunting tables per band with map references

### Evidence Repair (2026-09-26)
- Fixed evidence terminology across all docs to canonical labels only
- Removed old labels: `CONFIRMED`, `CLIENT`, `EXTERNAL`, `LIKELY`, `RECOMMENDED` as evidence
- Fixed: Progression Band was `BINARY_CONFIRMED` → now `DERIVED` (calculated from monster levels)
- Fixed: Monster tables was `BINARY_CONFIRMED` → now `DERIVED` (band assignment is calculated)
- Fixed: Equipment tables was `CLIENT_FACT` → now `BINARY_CONFIRMED` (binary type data)
- Fixed: Job Progression was `CLIENT_FACT` → now `EXTERNAL_REFERENCE` (source: game_knowledge, not binary)
- Fixed: NPC locations was `UNRESOLVED` → now data EXISTS as `CLIENT_FACT` (1,300 placements)
- Fixed: NPC names evidence is `PROBABLE` (from monsters.edt, no consumer runtime)
- Updated STATUS.md with canonical numbers and evidence labels
- Updated README.md with evidence legend
- Updated EVIDENCE_POLICY.md with canonical vocabulary only
- Updated KNOWN_GAPS.md with evidence labels
- Updated NPC_IDENTITY_REPAIR.md with evidence legend
- Updated PLAYER_PROGRESSION_MODEL.md with evidence labels

## 2026-09-25

### Player Progression
- Phase C2 validation COMPLETE
- Canonical MonsterID mappings verified (1=Piya, 22=Rascal Rabbit)
- Equipment classification repaired
- NPC identity repaired: 1,928 total, 293 resolved, 1,635 unresolved

### GitHub Sync
- Published NPC execution results via GitHub API
- Git push blocked by credential manager timeout
- Used direct GitHub API for lightweight text files only
