# Rule — RAG auto-query on doubt / agent non-consensus (tie-breaker)

> **Status:** MANDATORY cross-project wherever a RAG corpus exists (NotebookLM or equivalent)
> **Parent:** `_SETTINGS/RULES/INDEX.md`

## Default rule

**When DOMAIN DOUBT or NON-CONSENSUS between agents appears → query the RAG corpus AUTOMATICALLY, without asking the user for permission.**

Auth is a one-time bootstrap (see `nlm_anti_suspicion.md`); normal usage is free. Never block on "I have no confirmation to use the corpus".

## Auto-query triggers (any one)

1. **2/3 non-consensus between divergent agents** — no verdict reaches majority
2. **1-1-1 split** between agents (each wing proposes something different)
3. **The conservative wing says KEEP but two wings propose changes** — doubt on canonicity
4. **Concept ambiguous across traditions/frameworks/sources**
5. **Term with multiple senses in the current context**
6. **Editorial/architectural decision with impact across ≥3 chapters/modules**
7. **User asks for a definitive verdict on a rule/concept**

## Notebook routing (pattern)

Maintain a routing table per workspace, one corpus per axis. Example shape (fill with your own notebook IDs in a PRIVATE file, never commit IDs):

| Axis | Corpus | Used for |
|---|---|---|
| **DOCTRINE/DOMAIN** | `<DOCTRINE_NOTEBOOK_ID>` | canonicity decisions, mechanism verification, source citations |
| **TERMINOLOGY/LANGUAGE** | `<GLOSSARY_NOTEBOOK_ID>` | term verification, etymology, fluent equivalents |
| **RESEARCH/SCIENCE** | `<RESEARCH_NOTEBOOK_ID>` | cross-domain convergences, empirical parallels |

## Every RAG consult is itself divergent-convergent

**Never a single-shot query.** Each consultation is a process:
- **Divergent:** questions from distinct angles (domain-strict / reader / structural), then 2/3 synthesis
- **Iterative:** repeat rounds until the result is maximal for the given direction — don't stop at the first answer if there is more depth to extract
- **Multi-zoom:** ask at the right level — maha (role in the whole) → macro (chapter/module) → mezzo (section) → micro (paragraph/function) → nano (word/symbol)
- **Non-leading:** phrase questions neutrally; do not embed the desired answer in the question

## Query algorithm (4 steps)

1. **Detect trigger** (list above)
2. **Build the query — ALWAYS include the FULL source fragment** (never just an abstract question; generic answers and missing citations follow otherwise)
3. **Route to the correct corpus** (domain → doctrine corpus; language/style → glossary corpus; science → research corpus)
4. **Apply the RAG verdict as TIE-BREAKER** — the corpus is the 4th wing; its verdict breaks agent splits (effective consensus: 2/4 when RAG agrees with one agent)

## When RAG is NOT called automatically

- Single edit (1-2 words, 1 file)
- Typographic fix
- Version bump / build / export
- Structural reorganization (moving paragraphs/functions)
- Free creative editing with no canonicity controversy

## Throttle (anti-bot-detection, see nlm_anti_suspicion.md)

- ≥3s between consecutive queries
- max ~10 queries/session
- auth check max 1×/session — refresh ONLY on an actual 401
- never 2 simultaneous sessions on the same RAG account

## Channel note (validated)

If the RAG provider has both an MCP server and a CLI, and the MCP shows connect/disconnect churn → **prefer the CLI channel** (clean process per call, structured `--json` output, explicit `--timeout`). An unstable MCP in the loop is worse than no MCP.

## Forbidden anti-patterns

- ❌ Asking the user for permission per query (auth is already bootstrapped)
- ❌ Query without the full source fragment
- ❌ Applying the RAG verdict without passing it through the project's own style/rule filter first
- ❌ Wrong routing (language question sent to the doctrine corpus)
- ❌ Refresh-hammering the auth

## See also

- [[nlm_anti_suspicion]] (throttle + bootstrap)
- [[multi_agent_divergent_convergent]] (RAG as 4th wing)
- [[llm_words_eliminate]] (frequent context where the tie-breaker is needed)
