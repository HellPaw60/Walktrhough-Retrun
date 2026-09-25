# Project Scope

## Goal

Build a player progression knowledge base for Seal Online Return v5 that answers:
> "I'm level X, job Y. What should I do now, where do I go, what do I hunt, and what's next?"

## Data Source

- **Primary:** Client binary files (SPAK archives, EDT data files)
- **Method:** Known-plaintext attack (bkcrack) → unsealed extraction → manual binary parsing
- **External reference:** Wiki API (cross-reference only, not source of truth)

## Architecture

```
RAW CLIENT DATA (SPAK/EDT)
    ↓
FORMAT IDENTIFICATION (binary schemas)
    ↓
DECODE (EDT codec: _MULT=52845, _ADD=22719)
    ↓
PARSER (31×int64 for monsters, 85-col for items)
    ↓
VALIDATION (cross-check against Wiki API)
    ↓
NORMALIZED DATABASE (SQLite + CSV + Excel)
    ↓
3-LAYER PROGRESSION MODEL
    ↓
WALKTHROUGH OUTPUT
```

## Confidence Levels

| Level | Definition |
|---|---|
| BINARY_CONFIRMED | Proven by client binary structure |
| CLIENT_FACT | Extracted from client data |
| DERIVED | Calculated from client facts |
| PROBABLE | Equality observation, not consumer-tested |
| EXTERNAL_REFERENCE | Wiki/legacy correlation |
| UNRESOLVED | No evidence found |
| DEFERRED | Not investigated |
