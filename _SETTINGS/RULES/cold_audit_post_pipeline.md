# Cold Audit — mandatory after any multi-batch pipeline

> **Status:** MANDATORY cross-project for any editorial/refactor pipeline with ≥3 batches
> **Origin:** real incident — work declared "resolved" that a fresh audit proved only partially real
> **Parent:** `_SETTINGS/RULES/INDEX.md`

## The rule

**After any pipeline with ≥3 divergent-convergent batches (or ≥20 applied changes):** launch a "COLD" audit with 3 INDEPENDENT agents that have NO prior session context.

Never declare "resolved" until the cold audit confirms it.

## Triggers

1. ≥3 divergent-convergent batches on the same pipeline (word audit, style sweep, etc.)
2. Pipeline with ≥20 applied changes
3. Status about to be declared "COMPLETE" / "SATURATED" / "CANONICAL" / "VALIDATED"
4. Before any major rebuild / export / release
5. User asks "did you really fix it?" / "audit this honestly"

## The 3 mandatory cold wings (divergent)

### Wing A_READER (naive reader / consumer)
- Reads 5-8 random units (paragraphs / functions / entries) with zero session context
- Flags REAL problems for the consumer (fatigue, jargon, register breaks, redundancy, obscure passages)
- Blunt verdict, no diplomacy

### Wing A_REGRESSION (backup comparison)
- Compares 5 random files: current state vs pre-session backup (`archive/_backup_<date>_<context>/`)
- Flags substance LOSSES, OVERLOADS introduced, cosmetic-only vs real changes
- Computes an improvement/degradation ratio

### Wing A_RULES_STRICT (mechanical rule check)
- Greps the project's codified anti-patterns (template connectors, forbidden punctuation, banned phrasings)
- Samples 5 files for density / rhythm / structure rules
- Compares % violations detected vs % declared "resolved"

## Honest convergent (mandatory)

The synthesis is never cosmetic. The report must split findings into:

1. **Confirmed-real claims** (all 3 wings unanimously KEEP)
2. **Partially/false claims** (≥1 wing found problems)
   - Problems INTRODUCED by the pipeline (regression)
   - PRE-EXISTING problems left uncleaned
   - OVERLOADS introduced

## Forbidden anti-patterns

- ❌ Declaring "COMPLETE/CANONICAL" without a cold audit
- ❌ Auditing with agents that carry session context (automatically biased toward KEEP)
- ❌ Skipping the regression check — ignoring the pre-session backup
- ❌ Cosmetic claims without numbers ("it's canonical" without violation percentages)
- ❌ Grep-only validation (semantic issues escape regex)

## Empirically validated

A 14-batch editorial pipeline was declared "canonical, validated in 3 rounds". The cold audit found: 0 regex violations (the mechanical claim was true), but 5 REAL semantic problems remaining (3 pre-existing + 2 introduced by the pipeline itself), 1 conceptual substance loss, and 1 overload. **Lesson: "regex-clean" ≠ "semantically clean". The cold audit covers that gap.**

## Applicability

| Domain | Application |
|---|---|
| Long-form books / dense text | MANDATORY |
| Encyclopedic / reference corpora | MANDATORY |
| Spoken-audio scripts | MANDATORY |
| Massive code refactor | MANDATORY (compare with backup) |
| Technical articles | RECOMMENDED (large sample) |

## See also

- [[multi_agent_divergent_convergent]] (general procedure)
- [[protocol_paired_mesh_agents]] (paired waves)
