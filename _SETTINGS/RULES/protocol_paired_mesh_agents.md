# Paired Mesh Agents Protocol (N divergent + N convergent) — cross-project rule

> **Status:** MAXIMUM PRIORITY — applies BEFORE any action that requires understanding/investigation
> **Companion of:** `BEST_PRACTICES.md` PATTERN-009 (this file is the full procedure)

---

## The rule

**Before any action that involves investigation / understanding / a complex decision, spawn in parallel:**
- **N divergent mesh agents** (wide brainstorm, orthogonal hypotheses, different angles)
- **N convergent mesh agents** (targeted root-cause solutions, concrete patches)

Typical N = 10-15 per wave. **All agents in a SINGLE message (Agent tool, multiple invokes).**

Answer with your own text ONLY if you are not certain after reading the agents' reports.

---

## When it applies

- Complex bug / hang / freeze with no immediately visible cause
- Multi-file refactor
- Architectural decision
- Deep audit of a subsystem
- Any situation where you would be tempted to iterate serially ("try fix A, build, test, try fix B, build, test...")

## When it does NOT apply

- Trivial task (1-3 lines of code with an obvious cause)
- Purely informational writing (one MD file, a commit message)
- Point change with a predictable result
- Direct continuation on a path already decided with the user

## How to structure the two waves

### Divergent wave (orthogonal brainstorm)
- Each agent gets an EXCLUSIVE scope (different file, subsystem, layer)
- Each agent gets a distinct lens (enneagram wing: zoom level / perspective / modality)
- None of them modifies code — report under 250 words each
- Coverage examples: static code, live device, comparison with a reference implementation, DI graph, threading, IO races

### Convergent wave (targeted patches)
- Each agent receives one already-prioritized hypothesis from the divergent wave
- Output strictly: unified DIFF ready to apply
- Zero extra prose (telegraphic)
- Goal: have N fix candidates ready for the most probable cause

## Convergent synthesis

After collecting all reports:
1. Cross-reference: causes reported by ≥3 agents → high confidence
2. Pick top-1 cause + max 3 candidate patches
3. Apply patches in order: lowest risk → highest impact
4. Build → install → test
5. If unresolved → another paired round with NEW hypotheses (never surface patches in a loop)

## Forbidden anti-patterns

- ❌ Serial iteration of small fixes (build → test → try another → repeat) = credit burned without understanding
- ❌ Asking the user "what should I do?" when agents can produce the answer
- ❌ Spawning only 3-5 agents with vague scope → generic reports
- ❌ Convergent wave without a divergent wave → superficial fix

## Empirically validated

On a production Android app hang (chat stuck on "..."), ~6 hours were lost on serial iterative fixes across 3 builds. One paired-mesh round isolated the cause in a single pass (uncaught timeout cancellation + mutex hold + silent prompt truncation) and produced 3 convergent patches covering ~80% of the failure surface. Decision reached in 1 round instead of 4.

## Related

- `enneagram_multi_zoom_protocol.md` — multi-zoom + multi-lens, the basis for divergent wing distribution
- `swarm_preset.md` — hierarchical-mesh preset
- `multi_agent_divergent_convergent.md` — the general divergent-convergent procedure
