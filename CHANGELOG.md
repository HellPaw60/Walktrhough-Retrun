# Changelog

## 2026-09-25

### Player Progression Validation
- Completed Phase C2 validation.
- Verified canonical MonsterID mappings:
  - MonsterID 1 = Piya
  - MonsterID 22 = Rascal Rabbit
- Verified NodeIndex/QuestID separation.
- Verified progression/map/equipment outputs.
- Repaired equipment classification conflict (type=1 items correctly sub-classified).
- Kept unresolved quest/NPC/drop semantics explicitly unresolved.

### GitHub Knowledge Sync
- Synced lightweight research findings directly to GitHub via API.
- Did NOT upload raw client, SPAK, SQLite, snapshots, or bulk datasets.
- Local git push remains blocked by historical LFS budget constraints.
- GitHub repository is intentionally a lightweight research knowledge base.
