# Evidence Policy

## Hierarchy

1. **Binary field with proven schema** → BINARY_CONFIRMED
2. **Parser/schema with proven semantic** → CLIENT_FACT
3. **Client consumer reference** → DERIVED
4. **Structural correlation** → PROBABLE
5. **Name heuristic** → DERIVED (lower confidence)
6. **External reference** → EXTERNAL_REFERENCE

## Rules

- **"Best" terminology avoided.** Use "candidate", "available", "relevant".
- **Historical values preserved** with clear marking (INVALIDATED/SUPERSEDED).
- **Empty results documented** as NO EVIDENCE, not hidden.
- **Stale values in historical docs** must be in historical context only.

## Canonical IDs

| ID | Name | Source |
|---|---|---|
| 1 | Piya | monsters.edt field0=ID |
| 22 | Rascal Rabbit | monsters.edt field0=ID |
| 23 | Rascal Rabbit | monsters.edt field0=ID |

## Confidence Vocabulary

| Term | Meaning |
|---|---|
| CONFIRMED | Binary evidence |
| PROBABLE | Equality/observation |
| DERIVED | Calculated |
| EXTERNAL | Wiki/legacy |
| UNRESOLVED | No evidence |
| DEFERRED | Not investigated |
