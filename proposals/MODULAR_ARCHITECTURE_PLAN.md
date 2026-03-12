# KindPath Modular Architecture — Full Build Plan
*Drafted: 2026-03-13 by GitHub Copilot (Claude Sonnet 4.6)*  
*Approved: 2026-03-13 by Sam*  
*Status: APPROVED — Phase 0 next*

---

## Where We Are

### Repos with running code

| Module | Repo | Port | Status |
|--------|------|------|--------|
| KindAI (spine/host) | `kindai` | 7860 | ✅ Production |
| DFTE (trading engine) | `kindpath-dfte` | — | ✅ Built |
| BMR (benevolence registry) | `kindpath-bmr` | 8001 | ✅ Built |
| KindPath Analyser | `kindpath-analyser` | — | ✅ 247 tests pass |
| KindPath Q (JUCE plugin) | `kindpath-q` | — | ✅ D1-D4 complete |
| KindSense (field sensing) | `KindSense` | — | ✅ PR#3 merged |
| KindField (field definitions) | `KindField` | — | ✅ Stable |
| Kindfluence (social influence) | `Kindfluence` | 7861 | ✅ Built |
| **KindSocials** | `KindSocials` | 7862 | 🔨 In progress (other session) |
| **KindCoordination Engine** | `kindpath-kce` | 7870 | 🔨 Built last night |
| KPTH (platform prototype) | `KPTH` | 8001+ | 🔨 Prototype |
| KindPath Compass | `kindpath-compass` | — | ✅ Built |
| KindPath Collective Website | `kindpath-collective-website` | 3000 | ✅ Built |
| kindai-audit-job | `kindai-audit-job` | — | ✅ Cloud Run |

### Defined in PRODUCT_ARCHITECTURE.md — not yet repos

| Module | Assets exist in | Missing |
|--------|----------------|---------|
| KiNDIS | `kindai/ndis/` | Standalone repo + module boundary |
| KindCare | `kindai/ndis/` (partial) | Standalone repo + participant UX |
| KindHome | `kindai/missions/` (partial) | Standalone repo + household UX |
| KindWork | `kindai/missions/` (partial) | Standalone repo + work UX |
| KindBiz | `kindai/finance/` `kindai/crm/` | Standalone repo + biz UX |
| KindCreate | `kindai/creativity/` (placeholder) | Full repo — NEW TOP PRIORITY |
| KindLife | — | Bundle definition + orchestration |

### Mentioned by Sam — not documented anywhere yet

| Module | Description | Notes |
|--------|-------------|-------|
| **KindHealth** | Health monitoring + wellbeing tracking | Distinct from KindLife (which is social/safety field); KindHealth = physiological + mental health data |
| **KindStudy** | Learning + education module | Study planning, notes, knowledge retention, skill tracking |
| **KindPath** (standalone) | Research module | UI wrapper over KindSense + KindField + kindpath-analyser — "the scientist's view" |

---

## The Central Gap: No Module Contract

All the individual modules exist or are being built, but there is **no standard
contract** that defines how a module registers with the ecosystem, what it must
expose, and how modules communicate with each other.

The VST analogy is exact:
- **KindAI** = the DAW (host application) — loads and routes modules
- **kindpath-kce** = the plugin manager — knows what's installed, handles routing
- **Each module** = a VST plugin — same interface, different capability
- **KindLife / KindCreate / etc bundles** = preset patches — curated combinations

The missing piece is the **module protocol** — the interface every module signs.

> **Design principle (Sam, 2026-03-13):** "Think of it like a modular VST synth that you can keep adding effects modules into the chain." This is the exact mental model. KCE is the rackmount frame. Each module is a unit. Any combination works. KindLife is a full preset rack."

---

## Architecture: KindPath Module Protocol (KMP)

### Module Identity

Every KindPath module exposes a standard identity endpoint:
```
GET /api/module/identity
```
Returns:
```json
{
  "id": "kindcreate",
  "name": "KindCreate",
  "version": "0.1.0",
  "port": 7863,
  "capabilities": ["audio_analysis", "draft_management", "external_links"],
  "requires": [],
  "optional_peers": ["kindsocials", "kindpath-analyser"],
  "doctrine": "creativity",
  "health_endpoint": "/api/health",
  "bundle_tags": ["kindlife", "creator_bundle"]
}
```

### Module Health

Every module exposes:
```
GET /api/health
```
Returns `{ "status": "ok"|"degraded"|"offline", "components": {...} }`.

### Module Events (KCE bus)

Modules publish events to `kindpath-kce` at port 7870:
```
POST http://localhost:7870/api/events
{
  "source_module": "kindcreate",
  "event_type": "draft_completed",
  "payload": { ... },
  "field_time": "...",
  "residue": "..."
}
```

KindAI subscribes to event streams from KCE. When KindCreate completes a draft,
KindAI sees it and can offer commentary without KindCreate ever needing to know
KindAI exists.

### Module Registry in KCE

`kindpath-kce` maintains a `ModuleRegistry` table. On startup, each module
POSTs its identity. KCE tracks health, last-seen, and event streams.

KindAI discovers active modules by querying:
```
GET http://localhost:7870/api/modules
```

---

## Port Assignment (canonical, never change without updating all)

| Port | Module |
|------|--------|
| 7860 | KindAI workspace |
| 7861 | Kindfluence |
| 7862 | KindSocials |
| 7863 | KindCreate |
| 7864 | KiNDIS |
| 7865 | KindCare |
| 7866 | KindHome |
| 7867 | KindWork |
| 7868 | KindBiz |
| 7869 | KindHealth |
| 7870 | kindpath-kce (Coordination Engine) |
| 7871 | KindStudy |
| 7872 | KindPath (research module) |
| 7873 | KindLife (when running as bundle orchestrator) |
| 8001 | kindpath-bmr |
| 3000 | Website |
| 3100 | Paperclip |

---

## Module Specs

### KindCreate (TOP PRIORITY — new repo)
**Codename:** `kindcreate`  
**Port:** 7863  
**Repo:** `kindpath-kce` sibling → new repo `KindCreate`

**What it is:** Creativity hub. Open to ANY form of creativity — music, writing,
visual art, code, film, craft. Not socials-forward. Socials are optional and
handled by KindSocials as a peer.

**Core capabilities:**
- **Project workspace** — creative projects with drafts, versions, notes, media
- **KindPath Analyser integration** — spectral/frequency analysis of audio work
- **External tool linking** — deep links to any creative software (Ableton,
  Logic, Final Cut, Premiere, Figma, Procreate, VS Code, etc.) — we don't replace
  them, we coordinate alongside them
- **LSII + authenticity reading** — real-time creative signal analysis for artists
- **Draft management** — drafts at any fidelity, not just finished work
- **Inspiration corpus** — seedbank integration (kindpath-analyser seedbank)
- **Mood/energy tracking** — creative state journal (feeds KindHealth if active)
- **Sharing bridge** — when the creator WANTS to share, push to KindSocials;
  KindSocials never sees anything until the creator explicitly pushes

**KindAI integration:**
- KindAI can monitor creative session state (with toggle — off by default)
- KindAI can provide support, feedback, encouragement when toggled on
- KindAI can monitor KindSocials communications about creative work and surface
  support signals (separate toggle — off by default)

**Relationship to KindSocials:**
KindCreate is a peer of KindSocials — not below it, not inside it. They sit at the same level on the KCE module bus. Creativity happens first. Sharing is an opt-in action (`POST /api/share → KindSocials post queue`). KindSocials never sees anything until the creator explicitly pushes. KindAI can monitor the response to shared work (via KindSocials `/api/feed`) on a separate toggle — supporting the creator around what comes back, not surveilling the work itself.

**What it is NOT:**
- Not a DAW — it wraps them
- Not a social platform — KindSocials is
- Not an analyser — kindpath-analyser is; KindCreate is the UI layer

---

### KindHealth (new, not yet documented)
**Codename:** `kindhealth`  
**Port:** 7869

**What it is:** Physiological and mental health data hub. Distinct from KindLife
(which is about the social field and safety). KindHealth is about:
- Sleep quality, energy levels, pain/symptom journalling
- Medication tracking (opt-in, local only, encrypted)
- Movement / exercise
- Mental health check-ins (not clinical — supportive self-monitoring)
- Mood journalling
- GP appointment prep (KindAI can help you prepare)
- Wearable integration (Apple Health, Garmin, etc) via export import

**Key constraint:** All health data stays local. No cloud sync by default.
Health data is the most sensitive data a person holds.

**Integration:**
- KindCare workers can be granted read-access to relevant health signals by participant
- KindLife can aggregate health signals alongside social wellbeing

---

### KindStudy (new, not yet documented)
**Codename:** `kindstudy`  
**Port:** 7871

**What it is:** Learning and education support module.
- Active study sessions with KindAI as tutor
- Course and subject tracking
- Spaced repetition (note → flashcard pipeline)
- Reading list management
- Essay and assignment drafting (with Kindfluence-aware feedback)
- Focus mode integration (pomodoro, distraction blocking)
- KindPath framework learning — built-in intro to the KindPath doctrine

**Key user:** Students, self-directed learners, practitioners doing CPD.

---

### KindPath (standalone research module)
**Codename:** `kindpath-research`  
**Port:** 7872

**What it is:** The scientist's view. A clean UI wrapping KindSense +
KindField + kindpath-analyser + DFTE signals for researchers and practitioners
who want to run field analysis without the full KindAI suite.

- KindSense metric field explorer
- KindField schema browser
- kindpath-analyser: batch audio analysis UI
- DFTE signal dashboard (read-only)
- BMR profile comparison
- Export: signed JSON bundles

**User:** Practitioners, researchers, educators.
This is what you show a university research partner.

---

### KiNDIS (extract from kindai)
**Codename:** `kindis`  
**Port:** 7864  
**Assets:** Extract from `kindai/ndis/`

Already fully specced in PRODUCT_ARCHITECTURE.md. The extraction work is:
1. Create `KiNDIS/` repo
2. Move `kindai/ndis/*.py` → `kindis/core/`
3. Build FastAPI server at 7864
4. KindAI loads KiNDIS panel when module is active

---

### KindCare (extract + extend)
**Port:** 7865 — builds on NDIS participant-facing tools.

---

### KindHome / KindWork / KindBiz
**Ports:** 7866 / 7867 / 7868  
**Status:** Extract relevant kindai missions/tools, build domain-specific servers.

---

---

## Practitioner Use Case — Targeted & Coordinated Support

*Sam's explicit design intent (2026-03-13): the modular architecture is particularly powerful for practitioners in health and allied health, who can configure targeted module combinations to support clients with specific life stressors — maximising self-directed outcomes.*

### The core insight

Different life situations call for different module combinations. A person navigating acute anxiety triggered by financial stress needs different tools than someone managing a creative block or a student under exam pressure. The modules themselves don't change — the combination and the KindAI doctrine overlay do.

The KindAI spine is always present. It monitors (only what's toggled on), provides support, and can shift its doctrine based on the active bundle configuration. This means a practitioner can configure a client's environment to be precisely calibrated to their presenting situation — without exposing modules that are irrelevant or potentially triggering.

### Example: Acute financial anxiety
```
Active modules: KindHome + KindWork + KindBiz + KindHealth
KindAI doctrine: financial-stress overlay (gentle, non-judgemental, practical)
KindSocials: OFF (reduce comparison pressure)
KindCreate: OFF (unless creative expression is indicated)
KindStudy: OFF
KindAI social monitor: OFF
```

### Example: Isolation + low social confidence
```
Active modules: KindSocials + KindCreate + KindCare + KindHealth
KindAI doctrine: connection-support overlay
KindSocials social monitor: ON (KindAI supports responses, boundary-setting)
KindCreate: ON (creative expression as confidence foundation)
KindBiz / KindWork: OFF (reduce performance pressure)
```

### Example: Practitioner supporting NDIS participant
```
Active modules: KiNDIS + KindCare + KindPath (research view) + KindHealth
KindAI doctrine: KindCare overlay — trauma-informed, sovereignty-first
Practitioner view: read-access to participant-consented health signals
Participant view: their own KindCare + KindHealth modules only
Shared field: KindPath research view for practitioner-participant session review
```

### Example: Student managing exam stress
```
Active modules: KindStudy + KindHealth + KindCreate (for expression/release)
KindAI doctrine: academic-support overlay
KindSocials: limited (scheduled social time only, no passive scroll)
KindWork / KindBiz: OFF
```

### Example: Creator in full production
```
Active modules: KindCreate + KindPath (research/field analysis) + KindSocials
Kindfluence: ON (audience strategy)
KindAI: creative-support doctrine, social monitor ON during release windows only
KindHealth: ON (protect energy/sleep during production)
KindWork / KindBiz: minimal (protect creative time)
```

### What makes this possible

1. **Module isolation** — each module is independent; none knows about the others except through KCE events
2. **Doctrine overlays** — KindAI's behaviour shifts based on bundle configuration
3. **Practitioner visibility controls** — modules can expose read-only views to consented practitioners
4. **Togglability** — every KindAI monitoring feature is off by default; the user/practitioner activates only what's appropriate
5. **Self-directed** — the participant/user always has sovereign control of their data and which modules run

---

## Bundle Definitions

### KindLife Bundle
```
KindLife =
  KindBiz    (if self-employed / business owner)
  KindWork   (employment / freelance)
  KindHome   (household)
  KindCreate (creative practice)
  KindSocial (social presence — optional, toggleable)
  KindHealth (wellbeing)
  KindStudy  (if in study)
  KindAI     (the spine — always present)
```
A bundle is a KCE configuration: a named set of active modules + their
preset configuration + a KindAI doctrine overlay.

KindLife instances can have any subset. The user adds modules like plugins.

### Practitioner Bundle (suggested name: KindPractice)
```
KindPractice =
  KiNDIS    (NDIS business compliance)
  KindCare  (participant relationship)
  KindPath  (research / field analysis)
  KindAI    (AI support + documentation)
```

Practitioner uses KindPath to analyse field signals, KiNDIS for compliance,
KindCare for the participant relationship. KindAI monitors and suggests.

### Creator Bundle
```
KindCreate  (primary workspace)
KindSocials (when sharing)
Kindfluence (audience strategy / attention capital)
KindPath    (audience/signal field analysis)
KindAI      (creative support + socials monitoring — toggleable)
```

---

## KindAI Social Communication Monitoring (toggleable)

Sam's requirement: KindAI should have the **toggled** ability to monitor social
communications — comments, DMs, mentions — and provide support.

**Implementation:**
1. KindSocials exposes a `/api/feed` endpoint: incoming social signals
   (comments, mentions, DMs — aggregated, anonymised)
2. KindAI workspace has a `[SOCIAL MONITOR]` toggle in the KindSocials panel
3. When ON: KindAI missions daemon polls `/api/feed` at configurable interval
4. If a signal crosses a stress threshold (negative sentiment, abuse pattern,
   high volume), KindAI surfaces it in the briefing with support framing
5. User can ask KindAI for help composing responses, setting boundaries, etc.
6. Full data sovereignty: this toggle is OFF by default, no cloud processing.

---

## KindCoordination Engine Role

`kindpath-kce` currently handles:
- Tree-ring field model (replaces Paperclip task tracking for dev work)
- BEC detection (emergent patterns across repos)
- LSII/resonance scoring
- Temporal sovereignty (UTC + circadian context)

**Extended role under this architecture:**
- Module registry (all active KindPath modules register here)
- Module health monitor (polls all active modules)
- Event bus (modules publish events, KindAI subscribes)
- Bundle configuration store (which modules are active for which bundle)
- Paperclip bridge optional (events can sync to Paperclip or not)

**Does this replace Paperclip?**
**Yes — KCE is the primary coordination engine for all KindPath agent work** (Sam confirmed 2026-03-13). Paperclip can run as an optional external view or be decommissioned once KCE module orchestration is complete. The tree-ring/field model is more aligned with KindPath doctrine than ticket-based task systems. No new Paperclip seeds or issues should be created once Phase 0 is live — route everything through KCE ring deposits instead.

---

## Build Action Plan

### Phase 0: Module Protocol (1–2 sessions) — ARCHITECTURE GATE

Before any new module is built, the contract must exist.

- [ ] **P0.1** — Write `kindpath-kce/src/modules/module_registry.ts` — ModuleRegistry schema + REST endpoints (`GET/POST /api/modules`, `GET /api/modules/:id/health`)
- [ ] **P0.2** — Write `kindpath-kce/src/events/event_bus.ts` — event publish (`POST /api/events`), event subscribe (`GET /api/events/stream` SSE)
- [ ] **P0.3** — Write `kindpath-kce/docs/MODULE_CONTRACT.md` — the interface specification all module builders follow
- [ ] **P0.4** — Add module registration to KindSocials (retrofit — it exists, just add identity + health endpoints)
- [ ] **P0.5** — Add module registration to Kindfluence (retrofit)
- [ ] **P0.6** — KindAI module discovery: poll `localhost:7870/api/modules` on startup; surface active modules in toolbar

---

### Phase 1: KindCreate (1–2 sessions) — NEW REPO

- [ ] **P1.1** — Create `KindCreate/` repo with AGENTS.md, README.md per KindPath conventions
- [ ] **P1.2** — FastAPI server at port 7863: projects, drafts, media registry
- [ ] **P1.3** — External tool link registry: deep-link to Ableton, Logic, Figma, etc (launch from UI)
- [ ] **P1.4** — kindpath-analyser integration: drop audio path → get LSII + psychosomatic profile
- [ ] **P1.5** — Seedbank: save/load creative profiles
- [ ] **P1.6** — KindSocials bridge: `POST /api/share` → pushes to KindSocials post queue
- [ ] **P1.7** — KCE registration: module identity + health + event publishing
- [ ] **P1.8** — KindAI panel: `🎨 Create` panel in workspace (mirrors KindSocials pattern)

---

### Phase 2: KCE Module Orchestration (1 session) — INFRASTRUCTURE

- [ ] **P2.1** — Module registry schema + API in KCE
- [ ] **P2.2** — Event bus (SSE stream) in KCE  
- [ ] **P2.3** — Bundle definitions table in KCE database
- [ ] **P2.4** — Bundle activation endpoint: `POST /api/bundles/activate` → starts/stops relevant modules
- [ ] **P2.5** — KindAI workspace: bundle picker UI (which modules are active)

---

### Phase 3: KindHealth + KindStudy (1 session each) — NEW REPOS

- [ ] **P3.1** — `KindHealth/` repo: health journal, symptom tracking, medication, mood
- [ ] **P3.2** — `KindStudy/` repo: study sessions, spaced repetition, assignments

---

### Phase 4: KindPath Research Module (1 session) — CONSOLIDATION

- [ ] **P4.1** — `kindpath-research/` or surface within `kindpath-collective-website`
- [ ] **P4.2** — Field analysis UI: KindSense + KindField + analyser in one view
- [ ] **P4.3** — Export: signed JSON bundles for practitioners/researchers

---

### Phase 5: KiNDIS + KindCare Module Extraction (2 sessions) — EXTRACTION

- [ ] **P5.1** — `KiNDIS/` repo: extract from `kindai/ndis/`
- [ ] **P5.2** — `KindCare/` repo: participant-facing subset
- [ ] **P5.3** — Both register with KCE + get KindAI panels

---

### Phase 6: KindHome / KindWork / KindBiz (1 session each) — EXTRACTION + BUILD

- [ ] **P6.1** — `KindHome/`: household calendar, bills, tasks, grocery
- [ ] **P6.2** — `KindWork/`: task management, meetings, email triage, project sync
- [ ] **P6.3** — `KindBiz/`: cashflow, invoicing, CRM, opportunities

---

### Phase 7: Bundle Orchestration + KindLife (1 session) — INTEGRATION

- [ ] **P7.1** — KindLife bundle definition in KCE
- [ ] **P7.2** — Bundle UI in KindAI: activate/deactivate modules, see status
- [ ] **P7.3** — Practitioner bundle config
- [ ] **P7.4** — Creator bundle config

---

### Phase 8: Social Communication Monitoring (1 session) — KindAI + KindSocials

- [ ] **P8.1** — KindSocials `/api/feed` endpoint: incoming social signals
- [ ] **P8.2** — KindAI social monitor toggle (off by default)
- [ ] **P8.3** — Stress signal detection in feed stream
- [ ] **P8.4** — KindAI briefing integration: surfaces social stressors with support framing
- [ ] **P8.5** — Response drafting: KindAI helps compose boundary-holding responses

---

## What's Missing From Existing Documentation

1. **PORT MAP** — no canonical port assignment list exists anywhere. Fixed above.
2. **KindHealth** — completely absent from PRODUCT_ARCHITECTURE.md
3. **KindStudy** — completely absent from PRODUCT_ARCHITECTURE.md
4. **KindPath (standalone research module)** — conflated with the broader KindPath name; needs distinction
5. **Module contract / interface spec** — the most critical missing piece
6. **Bundle definitions** — KindLife mentioned but never specced
7. **KCE as module orchestrator** — AGENTS.md says "not replacing Paperclip" but Sam wants it to
8. **Social monitor toggle spec** — never documented
9. **KindCreate sharing bridge to KindSocials** — the connection is intuited but not specced
10. **Practitioner bundle** — completely missing; this is a key commercial use case

---

## Files to Update

- `kindai/PRODUCT_ARCHITECTURE.md` — add KindHealth, KindStudy, KindPath Research; update KindCreate with sharing bridge spec; add port map
- `kindpath-kce/AGENTS.md` — update role to include module orchestration (not just KCE)
- `KPTH/ROADMAP.md` — update to reflect new modular strategy
- `HANDOVER.md` — set Phase 0 as top priority

---

## Priority Order (recommended)

1. **Phase 0** — Module contract (do this first, everything else depends on it)
2. **KindCreate** — Sam's explicit top priority
3. **Phase 2** — KCE module orchestration (enables bundle composition)
4. **Social monitoring** — Phase 8 (low-effort add to existing KindSocials/KindAI work)
5. **KindHealth + KindStudy** — Phase 3 (new modules, moderate effort)
6. **KindPath Research** — Phase 4 (consolidation, lower risk)
7. **Extractions** — Phases 5 + 6 (extract existing code into proper modules)
8. **KindLife bundle** — Phase 7 (caps it all off)

---

*Awaiting Sam's approval to begin Phase 0.*
