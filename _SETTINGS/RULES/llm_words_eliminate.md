# Rule — ELIMINATE generic LLM words → replace with CONCRETE directional words

> **Status:** MANDATORY cross-project (literary + technical + editorial text)
> **Origin:** author feedback on a long-form book project — "don't write abstract 'pure'; use words that give the understanding a direction from context"
> **Parent:** `_SETTINGS/RULES/INDEX.md`

## Principle

LLMs leave a fingerprint in **abstract-decorative-evaluative** words that **add no direction** of understanding — they fill space with generic positive evaluation.

**The abstract LLM word:**
- Is polysemous (can mean 5 different things in different contexts)
- Does not name the concrete operation of the mechanism
- Is added as "emotional accent", not as "semantic precision"

**The concrete directional word:**
- Names ONE specific mechanism
- Is verifiable (the reader can observe the phenomenon)
- If replaced with a synonym, the paragraph changes meaning

## Forbidden families (when decorative)

| Family | Members | Concrete direction instead |
|---|---|---|
| **purity** | pure, purity | "without veil", "without mixture with X", "without identification with Y" |
| **absolute/total/complete** | absolute, complete, total, entirety | "without exception", "without residue", "from the root", "on all N levels" |
| **essential/fundamental** | essential, fundamental, intrinsic, inherent | "from the root", "before any addition", "as the first moment", "preceding any X" |
| **real/reality** (abstract) | real, reality, truly | "observable", "verifiable through Y", "present as sensation/operation" |
| **profound/deep** | profound, deep, depth | "at level X", "beyond layer Y", "below threshold Z" |
| **ultimate/finality** | ultimate, ultimately, finality | "at stage X", "after Y dissolves", "when Z has fully unwound" |
| **genuine/authentic/true** | genuine, authentic, true (non-technical) | "which leaves a trace", "with verifiable effect", "without residue of claim" |
| **vast/immense** | vast, immense, "at large scale" | "across N instances", "with duration X", "over Y stages" |

**Exception:** canonical/technical terms of the domain keep their word (a doctrinal term, a defined metric). The ban applies to DECORATIVE use only.

## Replacement algorithm

For every occurrence of a generic LLM word:

1. **Identify the mechanism context** (paragraph by paragraph)
2. **Ask:** what concrete operation does this word describe here?
3. **Answer with a directional phrase:**
   - What is the state before? (X)
   - What is the state after the described action? (Y)
   - The replacement names the X→Y transition in concrete vocabulary
4. **Validation:** can the word be swapped with a synonym without changing the meaning? YES → it was decorative, replace it. NO → it was specific, keep it.

## Cross-domain examples

| Before (LLM-generic) | After (concrete directional) |
|---|---|
| "a profound transformation" | "a transformation at the root of the habit loop" |
| "complete knowledge" | "knowledge with no remainder of X" |
| "the code is clean and correct" | "latency under X ms, coverage Y%, zero lint warnings" |
| "a truly authentic connection" | "a connection that leaves silence after it" |
| "real progress" | "progress observable in metric X" |

## Forbidden anti-patterns

- ❌ Abstract evaluative adjective with no direction ("pure", "absolute", "total")
- ❌ Replacing one generic LLM word with another ("pure" → "authentic" substitutes nothing)
- ❌ Deletion without addition (the paragraph goes flat; the mechanism's precision is lost)
- ❌ Applying the rule to canonical/technical terms

## Validation via divergent-convergent (mandatory for bulk)

For replacement on a large batch (≥10 occurrences):
- **Agent A1** (domain mechanism): is the word canonical or decorative?
- **Agent A9** (reader assimilation): does the word help the reader catch the direction?
- **Agent A5** (structural): does the word add new information or is it noise?
- **Convergent**: 2/3 majority decides KEEP / REPLACE / REMOVE

## See also

- [[multi_agent_divergent_convergent]] (procedure)
- [[cold_audit_post_pipeline]] (verify the batch afterwards)
