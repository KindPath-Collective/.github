# KindPath Collective — Master Build To-Do

> Living document. Updated as work progresses.
> Last reconciled: 12 March 2026

---

## Gemini List — Reconciliation Against What's Already Built

### ✅ Already built (code exists — some wiring gaps remain)

| Gemini Item | Status | Notes |
|---|---|---|
| `tab_scout.py` | EXISTS (283 lines) | Chrome tab reader + keyword relevance scoring. **Gap**: no ν-coefficient connection to DFTE KPRE engine |
| `opportunity_scout.py` | EXISTS (181 lines) | Basic version built. **Gap**: Agent-Skills/t3code ingestion not wired |
| `code_reviewer.py` | EXISTS (241 lines) | Built — git diff + ruff + LLM review pipeline complete |
| `visual_explainer.py` | EXISTS (229 lines) | Mermaid diagram generation built — includes DFTE signal flow templates |
| `orchestrator.py` | EXISTS (320 lines) | Pipeline orchestrator built — Steps, retry, feed emission. CoPaw/Junie patterns would layer on top |
| `memory.py` | EXISTS (195 lines) | Episodic → semantic memory consolidation. **Gap**: BM25 + vector hybrid search not implemented |
| `forgazi_sentiment.py` | EXISTS | RSS/Yahoo Finance sentiment, entity linking, contradiction detection. **Gap**: not wired into kindpath_run orchestrator call |
| `relational_timestamp.py` | EXISTS | In kindpath-dfte root. **Gap**: not integrated into AnalysisEngine call chain |
| `wisdom_engine.py` | EXISTS | Multi-source ν. **Gap**: single-symbol only — no SPY/BTC/Gold correlation |
| FRED API | Partial | Referenced in wisdom_engine comments (lines 13, 164, 880) but NOT wired into `backtest/fetch_sim_data.py` |

---

## Comprehensive To-Do List

### 🔥 Blocked / Active

| # | Item | Repo | Status | Notes |
|---|---|---|---|---|
| A1 | Complete Cloud Build GitHub trigger | kindai | BLOCKED (user) | Needs browser OAuth at `console.cloud.google.com/cloud-build/triggers/connect?project=kindpath-bmr` then run `setup_github_trigger.sh` |
| A2 | Verify kindai Cloud Run deployment end-to-end | kindai | BLOCKED (A1) | Cloud SQL + VPC + alembic migration path — needs a test build |
| A3 | Unarchive kindpath-bmr + kindpath-starter | GitHub | [ ] | `gh api repos/KindPath-Collective/kindpath-bmr --method PATCH --field archived=false` then push 15+ uncommitted local changes |

---

### 🧠 KindAI Agent Intelligence

| # | Item | Repo | Status | Notes |
|---|---|---|---|---|
| B1 | Connect `tab_scout.py` to DFTE ν-coefficient scorer | kindai | [ ] | Replace keyword-match scoring with actual `syntropic_coeff` from KEPE — makes tab relevance physically meaningful |
| B2 | Wire tab_scout → `db/knowledge_base.db` RAG injection | kindai | [ ] | Inject high-signal pages into KB for RAG context in every chat session |
| B3 | `opportunity_scout` — ingest Agent-Skills + t3code signals | kindai | [ ] | Add GitHub repo scanner to pull README/issues from strategic repos into KB |
| B4 | `memory.py` — RAG 2.0: BM25 + vector hybrid search | kindai | [ ] | Add `rank_bm25` + `sentence-transformers` for hybrid retrieval; current impl is plain file injection |
| B5 | `file_tool.py` — "everything to text" Google extraction | kindai | [ ] | Integrate `markitdown` (Microsoft) or `google-generativeai` document parsing for PDF/DOCX/images |
| B6 | Wire `daily_briefing.py` to consume tab_scout high-signal output | kindai | [ ] | Current briefing doesn't pull from tab scout — connect the feed |
| B7 | Multi-agent orchestrator: port CoPaw/Junie distributed patterns into `orchestrator.py` | kindai | [ ] | Add parallel step execution + agent-to-agent handoff patterns |
| B8 | Ollama + DuckDuckGo "Research Mode" — local Perplexity clone | kindai | [ ] | `kindai --research "query"` → Ollama + DDG SERP → synthesise answer offline |

---

### 📈 DFTE: Syntropic Trading Stack

| # | Item | Repo | Status | Notes |
|---|---|---|---|---|
| C1 | Wire FRED API into `fetch_sim_data.py` | kindpath-dfte | [ ] | Macro weighting (yield curve, CPI, Fed rate) into backtest harness — currently only in wisdom_engine comments |
| C2 | `wisdom_engine.py` multi-symbol resonance | kindpath-dfte | [ ] | SPY/BTC/Gold correlation analysis in Syntropic Field — current engine is single-symbol |
| C3 | Wire `forgazi_sentiment.py` → main kindpath_run signal pipeline | kindpath-dfte | [ ] | Forgazi exists but isn't called from `orchestrator.py` — just standalone |
| C4 | Relational Timestamping at signal level | kindpath-dfte | [ ] | `relational_timestamp.py` exists but needs integration into AnalysisEngine call chain |

---

### 🎵 KindPath Q: C++ Plugin

| # | Item | Repo | Status | Notes |
|---|---|---|---|---|
| D1 | Build analysis engine (`SpectralAnalyser`, `DynamicAnalyser`, `HarmonicAnalyser`, `TemporalAnalyser`, `SegmentBuffer`, `AnalysisEngine`) | kindpath-q | [ ] | 8-step build order in CLAUDE.md — not started. `core/` must have zero JUCE dependency. |
| D2 | Build `core/fingerprints/` (`EraDetector`, `TechniqueDetector`, `InstrumentDetector`) | kindpath-q | [ ] | Depends on D1 |
| D3 | Build JUCE plugin wrapper (`shared/KindPathProcessor`, `plugins/kindpath-q`) | kindpath-q | [ ] | Depends on D2 |
| D4 | Build UI (`LsiiPanel`, `TrajectoryPanel`, `ElderPanel`, etc.) | kindpath-q | [ ] | Depends on D3 |
| D5 | Spectral Coherence → BMR scale mapping | kindpath-q + kindpath-bmr | [ ] | New: map audio frequency field features onto BMR benevolence metrics |
| D6 | Visual Resonance Overlay in kindai Electron app | kindai | [ ] | Real-time audio analysis dashboard tab in the existing kindai-app wrapper |

---

### 📚 Documentation / Infrastructure

| # | Item | Repo | Status | Notes |
|---|---|---|---|---|
| E1 | Self-hosting Docker-compose docs for Pi cluster | KPTH | [ ] | Document the full kindai + kindpath-bmr + postgres stack for local Pi deployment |
| E2 | Nano-Relational Whitepaper update | kindpath-fieldkit / kindpath-canon | [ ] | Add week-2 farm realisations (collective trauma + willing consent) to Universal Incursion doc |
| E3 | Sydney data centre latency research | kindai | [ ] | Anthropic `australia-southeast1` endpoint investigation — add to config.yaml docs |
| E4 | Run full kindpath-analyser test suite | kindpath-analyser | [ ] | Verify all new modules pass after commits — background run only confirmed first 2 tests |
| E5 | Set up org-level GitHub Actions for automated test runs | .github | [ ] | Add a reusable workflow so all Python repos run pytest on push |

---

## Progress Legend

- `[ ]` Not started
- `[~]` In progress
- `[x]` Complete
- `BLOCKED` — requires external action (browser OAuth, unarchive, etc.)
