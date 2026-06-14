# CCDEW — Claude Code Efficient Workspace

[![tests](https://img.shields.io/badge/tests-147%2F147%20PASS-brightgreen?style=flat-square)]()
[![audit](https://img.shields.io/badge/audit-38%2F38%20PASS-brightgreen?style=flat-square)]()
[![version](https://img.shields.io/badge/version-3.8.2-blue?style=flat-square)]()
[![platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey?style=flat-square)]()
[![license](https://img.shields.io/badge/license-MIT-green?style=flat-square)]()

> A drop-in workspace for Claude Code that cuts token cost by ~76% per prompt, routes every task to the right agent, blocks secret leaks before they ship, and audits itself continuously — without slowing you down.

---

## What it is

CCDEW is a configuration layer that sits on top of Claude Code. Clone it, open Claude Code inside it, and the system self-activates: 13 hooks wire into Claude Code's lifecycle, 19 slash commands become available, and every session is automatically filtered, routed, monitored, and archived.

No manual setup per-project. Clone once, use everywhere.

---

## What it does

| Layer | Component | Effect |
|---|---|---|
| Context | **SSA** (Jaccard trigram filter) | Loads 14 relevant files instead of 130+ — **−76% tokens/prompt** |
| Routing | **Enneagram** (9 nodes) + **SAFLA** (adaptive) + **Instincts** (pattern memory) | Picks the right agent for every prompt — wrong-agent rate drops from ~30% to <10% |
| Cost | **codeburn** + native fallback engine | Live cost in statusline, daily budget alerts at 75% and 100% |
| Security | **secret-scan** (11 patterns) | Blocks edits containing AWS/Anthropic/OpenAI keys, RSA keys, JWTs, and 8 sensitive path patterns |
| Quality | **5-zoom audit** (Maha → Macro → Mezzo → Micro → Nano) | Finds structural, module, function, line-level drift — runs in < 1 s |
| Git | **auto-/verify** + **auto-/quality-gate** | Blocks `git commit` and `git push` if checks fail |
| Memory | **session-snapshot** | Archives every session to JSON + Obsidian Markdown |
| Stability | **cross-process file-lock** | 200/200 concurrent writes survive without corruption |

---

## Numbers

```
Tokens / prompt      ~10,000  →  ~2,400     (−76%)
Memory at start       130+ files →  14        (−85%)
Session-end overhead   8.5 s   →  117 ms     (−98.6%)
SSA filter latency                 0.31 ms
SAFLA record latency               0.85 ms
Pre-edit secret scan              ~30 ms
Full audit (/infer)               < 1 s
Full test suite                   < 2 s
Hook timeout (hard limit)          5 s
```

```
22 test suites · 147 / 147 PASS
38 audit checks · 0 WARN · 0 FAIL
~8,000 LOC · 49 helpers · 34 skills
19 slash commands · 13 hook events
10 architectural decisions documented
1 optional external dependency (codeburn)
```

---

## What it looks like

**Statusline injected into every prompt:**
```
💰 $12.40/$100/d · 63c   │   🤖 71% ok·14fb   │   📂 CCDEW
```

**Routing hint on every prompt (`inject-workflow`):**
```
[AUTO-SWARM DIRECTIVE] refactor the auth module across 5 files
Node 7 (Innovator) | HEXAD | SSA:MAHA SAFLA:+0.05
SPAWN: reviewer → researcher → backend-dev → sparc-orchestrator → analyst → architecture
[INSTINCT] you usually route this to node 7 (83% confidence over 6 similar prompts)
```

**Secret blocked before edit:**
```
[BLOCKED] Secret leak risk: 1 pattern detected: AWS Access Key
```

**5-zoom audit (`npm run infer`):**
```
[INFER] 10 findings · HIGH:2 WARN:7 INFO:1

MAHA:  [⚠] <tests> — Only 22 suites — below 50 expected for this LOC
MACRO: [✗] hook-handler.cjs — 1211 lines — exceeds hard cap 500
MICRO: [⚠] intelligence.cjs:325 — Function init() is 111 lines (>75)
NANO:  (clean)
```

---

## Installation

### Prerequisites

| Tool | Minimum | How to install |
|---|---|---|
| **Claude Code CLI** | latest | `npm install -g @anthropic-ai/claude-code` |
| **Node.js** | 18+ | [nodejs.org](https://nodejs.org) |
| **Git** | any | [git-scm.com](https://git-scm.com) |
| **Python** | 3.8+ | [python.org](https://python.org) — optional, Obsidian helpers only |
| **codeburn** | latest | `npm install -g codeburn` — optional, canonical cost data |

`ANTHROPIC_API_KEY` must be set in your shell environment.

---

### Clone and verify

```bash
# Linux / macOS
git clone https://github.com/Hermeneuticus-of-things/claude-code-eficient-workspace.git CCDEW
cd CCDEW
npm test          # expect: 147/147 PASS
npm run audit     # expect: 38/38 PASS
```

```powershell
# Windows (PowerShell)
git clone https://github.com/Hermeneuticus-of-things/claude-code-eficient-workspace.git CCDEW
cd CCDEW
npm test
npm run audit
```

Or use the bootstrap scripts (clone + prereq check + tests in one command):

```bash
bash bootstrap-ccdew.sh        # Linux / macOS
.\bootstrap-ccdew.ps1          # Windows PowerShell
```

---

### Start

```bash
cd CCDEW
claude
```

Claude Code reads `.claude/settings.json` on startup — all 13 hooks activate automatically. No manual wiring.

**Verify hooks are live:** type any prompt and you should see a `system-reminder` block with routing output. If the statusline (`💰 ... 🤖 ...`) appears, everything is running.

---

## Configuration

Edit `.claude/helpers/feature-flags.json` to enable or disable components and tune thresholds:

```json
{
  "components": {
    "enneagram":   true,
    "ssa":         true,
    "codeburn":    true,
    "safla":       true,
    "secret_scan": true,
    "instincts":   true,
    "graphify":    true,
    "red_hat":     true
  },
  "ssa": {
    "top_k":     12,
    "min_score": 0.15
  },
  "codeburn": {
    "daily_budget_usd":  100.0,
    "warn_at_pct":       0.75,
    "alert_at_pct":      1.0,
    "cost_per_call_warn": 0.05
  },
  "safla": {
    "weight_success": 0.05,
    "weight_failure": -0.10,
    "weight_clamp":    0.5
  }
}
```

### Environment variables

| Variable | Required | Effect |
|---|---|---|
| `ANTHROPIC_API_KEY` | **Yes** | Your Anthropic API key |
| `HOOKS_SKIP=1` | No | Emergency bypass for git commit/push auto-checks |
| `GITHUB_TOKEN` | No | Raises `/skills-propose` rate limit from 60 to 5000 req/h |
| `PYTHON_BIN` | No | Explicit Python path if auto-detect fails on Windows |
| `CCDEW_LANG=ro` | No | Romanian UI strings (partial) |

---

## Commands

### npm scripts (run from CCDEW/)

```bash
npm test                           # run all 22 test suites
npm run audit                      # full evaluate-setup (38 checks)
npm run audit:fix                  # auto-resolve drift
npm run infer                      # 5-zoom audit (Maha→Nano)
npm run verify                     # quick pre-commit sweep
npm run quality-gate               # strict pass/fail gate (used before push)
npm run burn                       # live cost report
npm run bench                      # hot-path benchmarks
npm run skills-propose -- "<tag>"  # find Claude Code skills matching a keyword
npm run exit                       # manual session snapshot (JSON + Obsidian MD)
npm run sessions-compare -- 5      # compare last 5 sessions
npm run optimize -- nano           # auto-fix NANO-level drift
```

### Slash commands (inside Claude Code)

```
/evaluate-setup    Full workspace audit
/verify            Pre-commit check (types + tests + lint + secrets)
/quality-gate      Strict gate before merge/push
/infer             5-zoom audit (Maha → Nano)
/review            Code review on current diff
/diff-explain      Explain git diff in plain language
/research          Deep research on a topic
/optimize          Fix drift at specified zoom level
/skills-propose    Find skills matching a keyword from GitHub
/skills-active     List currently active skills
/instincts         Show routing pattern memory
/cost              Current and monthly cost summary
/bench             Run hot-path benchmarks
/mcp-health        Check MCP server connectivity
/platform          Show platform / Python / Node detection results
/exit              Session snapshot + archive
/sessions-compare  Diff last N sessions
/errors            Last 20 logged errors (PII-redacted)
/safla-clean       Fix corrupt safla.json (pre-v3.0 migration)
```

---

## Auto-triggers

These fire without any manual command:

| Trigger | Action | Bypass |
|---|---|---|
| `git commit` | Auto-runs `/verify` — blocks on fail | `HOOKS_SKIP=1 git commit ...` |
| `git push` | Auto-runs `/quality-gate` — blocks on fail | `HOOKS_SKIP=1 git push ...` |
| Any file edit | `secret-scan` — blocks edit if leak detected | disable in `feature-flags.json` |
| Every prompt | `inject-workflow` → routing hint in `system-reminder` | — |
| SessionStart (24h) | Auto-runs `/evaluate-setup`, alerts on FAIL/WARN | — |
| SessionEnd | Session snapshot to JSON + Obsidian MD + benchmark | — |

---

## How it works internally

### SSA — Semantic Scope Approximation

At every `SessionStart` and `UserPromptSubmit`, SSA computes Jaccard trigram similarity between the incoming prompt and all memory entries. Top-K results (default: 12) are injected into context. The rest are silenced. Result: ~76% fewer tokens without losing relevant context.

### Enneagram routing

Every prompt is scored against 9 node archetypes (Reformer, Helper, Achiever, Individualist, Investigator, Loyalist, Innovator, Challenger, Peacemaker). The best-matching node determines the agent chain: TRIANGLE (3 agents, fast) or HEXAD (6 agents, deep). SAFLA adjusts node weights based on outcome feedback — routing improves over time.

### SAFLA — Self-Adaptive Feedback Loop Architecture

After every task, SAFLA records `success` or `failure` and adjusts per-node weights (±0.05 by default, clamped at ±0.5). After ~20 prompts, routing accuracy converges. Weights persist in `.claude-flow/data/safla.json`.

### 5-zoom audit

`/infer` (or `npm run infer`) scans the workspace at 5 levels:

| Level | Checks |
|---|---|
| **Maha** | Total LOC, test coverage ratio, helper count |
| **Macro** | File line counts vs hard/soft caps |
| **Mezzo** | Function counts and responsibility balance |
| **Micro** | Function length, complexity indicators |
| **Nano** | TODO/FIXME density, dead code, naming drift |

### Secret scan

Pre-edit hook runs 11 regex patterns on file content before any write:
AWS Access Key, Anthropic Key, OpenAI Key, RSA private key, JWT, GitHub token, and 8 sensitive path patterns (`.env`, `credentials`, `id_rsa`, etc.).

---

## How the hooks connect

All hook logic lives in `.claude/helpers/hook-handler.cjs`. The hooks registered in `.claude/settings.json`:

| Event | Hook | What it does |
|---|---|---|
| `UserPromptSubmit` | `inject-workflow` | SSA filter + Enneagram route + Skills suggest |
| `UserPromptSubmit` | `route` | SAFLA scoring, Instincts lookup |
| `PreToolUse: Write/Edit` | `pre-edit` | Secret scan — blocks if leak found |
| `PreToolUse: Bash` | `pre-bash` | Detects `git commit`/`push`, fires auto-verify/gate |
| `PostToolUse: Write/Edit` | `post-edit` | SAFLA outcome record, auto-learn update |
| `PostToolUse: Bash` | `post-bash` | Cost estimate update |
| `SessionStart` | — | SAFLA init, auto-audit (24h cadence), statusline |
| `SessionEnd` | — | Snapshot, benchmark, Obsidian archive |
| `Stop` | — | SAFLA session close |
| `PreCompact` | — | Summarize context before compaction |

`.claude/settings.json` is committed to the repo — hooks activate for anyone who clones.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `[CODEBURN] CLI unavailable` | codeburn not installed globally | `npm install -g codeburn` (native fallback works without it) |
| `Python was not found` (Windows) | Windows Store alias intercepts `python` | CCDEW auto-probes with `-V`; confirm with `npm run audit` |
| Hooks don't fire | settings.json not picked up | Restart Claude Code (settings read at launch only) |
| `git commit` blocked | `/verify` failed | Run `npm run verify` to see why; bypass: `HOOKS_SKIP=1 git commit ...` |
| `[BLOCKED] Secret leak risk` on legit file | False-positive on hex/JWT-like string | Disable temporarily: `feature-flags.json → secret_scan: false` |
| `safla.json` shows `[object Object]` | Pre-v3.0 corrupt state | Run `node .claude/helpers/hook-handler.cjs safla-clean` |
| `ToolSearch +swarm_init` returns nothing | MCP claude-flow not loaded | Restart Claude Code; `.mcp.json` must be present in the workspace root |
| Routing hint never appears | `inject-workflow` hook not firing | Check `npm run audit` — hook config section must show PASS |

For detailed diagnostics:
```bash
node .claude/helpers/hook-handler.cjs errors   # last 20 errors, PII-redacted
npm run audit                                  # full 38-point check
```

---

## Privacy

- **Nothing leaves your machine.** `codeburn` reads `~/.claude/projects/` locally for cost data — it is never transmitted.
- **GitHub API** is called only when you explicitly run `/skills-propose <keyword>`. No auth required; `GITHUB_TOKEN` is optional and only raises the rate limit.
- **All error logs** (`errors.jsonl`) are auto-redacted via `lib/redact.cjs` — emails, keys, and home paths are replaced with `~` before writing.
- **Session snapshots** stay local and are git-ignored (listed in `.gitignore`).
- **Secret-scan** is entirely local regex — no external service involved.

---

## Stability

| Test | Result |
|---|---|
| 200 concurrent fork() writes to safla.json | 200/200 correct |
| 50 concurrent in-process atomic writes | 0 orphans |
| 1000 serial SAFLA outcomes | 0 corrupt keys |
| 30+ fuzz inputs (malformed JSON, empty, binary) | all rejected cleanly |
| BOM / CRLF / RTL / emoji in file content | handled |
| Disk full / read-only / corrupt JSON | graceful degradation |
| Linux + macOS + Windows | all supported |

---

## Roadmap

| Item | Status |
|---|---|
| Split `hook-handler.cjs` (1211L) into focused modules | Technical debt — see `_MEMORY/decisions/008` |
| Split `intelligence.cjs` (979L) | Technical debt — same ADR |
| ONNX local vector embeddings | Blocked on ruflo upstream |
| TypeScript migration via JSDoc | In exploration — `lib/jsdoc-validator.cjs` validates exports |
| Full `CCDEW_LANG` propagation | Low priority |
| MCP self-test after restart | Feasible via ToolSearch probe |

---

## Documentation

| File | Contents |
|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | Module map, data flow diagram, hook lifecycle, 5-zoom design |
| [CHANGELOG.md](CHANGELOG.md) | Full version history v1.0 → v3.8.2 |
| [MIGRATION.md](MIGRATION.md) | Upgrade guide v2.x → v3.x |
| [CREDITS.md](CREDITS.md) | Attribution per dependency (direct, inspired, original) |
| [_MEMORY/decisions/INDEX.md](_MEMORY/decisions/INDEX.md) | 10 architectural decisions with context and rationale |

---

## Comparison

| Feature | CCDEW | [ECC](https://github.com/affaan-m/everything-claude-code) | [setup-evaluator](https://github.com/redhat-community-ai-tools/claude-code-setup-evaluator) | [ruflo](https://github.com/ruvnet/claude-flow) |
|---|---|---|---|---|
| Token reduction (context filter) | ✅ −76% | ✅ | — | — |
| Adaptive agent routing | ✅ Enneagram + SAFLA + Instincts | ✅ | — | ✅ |
| Real-time cost tracking | ✅ codeburn + native fallback | — | — | — |
| Secret-leak pre-edit block | ✅ 11 patterns | — | ✅ | — |
| 5-zoom self-audit | ✅ original | — | — | — |
| Auto-verify on git commit | ✅ | — | partial | — |
| Cross-process race safety | ✅ | — | — | — |
| Session archival (JSON + Obsidian) | ✅ | — | — | — |
| Cross-platform | ✅ | ✅ | — | — |

CCDEW is a **depth-first integrator**: it combines ideas from all three and adds original layers (5-zoom audit, file-lock, session archival, skill-proposing). Attribution in [CREDITS.md](CREDITS.md).

---

## License

MIT — see [LICENSE](LICENSE).
