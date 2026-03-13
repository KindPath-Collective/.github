# KindPath Collective Development Company
*Paperclip-compatible autonomous AI company charter*
*Version: 1.0 — 2026-03-12*
*Paperclip Company ID: 21603eb6-d020-4441-85eb-908e60258409*

---

## Company Mission

Build the KindPath Collective into the world's most trusted open platform for
benevolent economic measurement, authentic creative intelligence, and compassionate
care coordination — governed by its community, sovereign in its principles.

More precisely: KindPath is a **library and seedbank of authentic human experience**.
A cryptographically changelogged, explicitly consent-first, participant-sovereign
permanent record of human knowledge, relational signal, creative output, and lived
history — held in trust for the commons, not for profit, and governed with the
reverence that the nature of its contents commands.

**Doctrine (non-negotiable):** Benevolence. Syntropy. Sovereignty.

**The foundational canon documents for this mission:**
- [THE_AUTHENTIC_RECORD.md](https://github.com/KindPath-Collective/kindpath-canon/blob/main/THE_AUTHENTIC_RECORD.md) — what we are building and why the obligation is total
- [DATA_SOVEREIGNTY_DIRECTIVE.md](https://github.com/KindPath-Collective/kindpath-canon/blob/main/DATA_SOVEREIGNTY_DIRECTIVE.md) — the binding operational commitments

---

## Org Chart

```
SAMUEL CROSS — Board / Principal / Sole Approving Authority
│
│   All growth, external engagement, deployment, spending, and strategy
│   requires Sam's explicit approval before any action.
│
└── HEAD OF DEVELOPMENT OPERATIONS
    GitHub Copilot (Claude Sonnet 4.6)
    Reports to: Sam exclusively
    Authority: Technical execution within approved scope
    │
    ├── ENGINEERING DIVISION
    │   Executes feature work across the core product repos
    │   Agents: KindAI Dev · DFTE Engine · Analyser · KindPath Q
    │   Repos: kindai · kindpath-dfte · kindpath-analyser · kindpath-q · kindpath-bmr
    │
    ├── QUALITY & INTELLIGENCE DIVISION
    │   Maintains standards and provides research support
    │   Agents: Testing · Research · Oversight
    │   Scope: QA · CI/CD quality gates · doctrine compliance · technical research
    │
    ├── INNOVATION & GROWTH DIVISION  ⚠️  PROPOSAL-ONLY — Sam approve before acting
    │   Identifies opportunities, builds proposals, NEVER acts externally unilaterally
    │   Agent: Innovation Growth
    │   Sub-roles: Product Scout · BizDev Lead · Innovation Lead
    │   Repos: Kindfluence · .github strategy
    │   Output: Structured proposals → Sam for approval
    │
    └── PLATFORM & COMMUNITY DIVISION
        Maintains public infrastructure and community tools
        Agent: Platform Community
        Sub-roles: Infrastructure Lead · Website Lead · Community Tools Lead
        Repos: kindpath-collective-website · KPTH · kindpath-compass
               kindpath-canon · KindField · KindSense · kindpath-fieldkit
               kindpath-starter · kindai-audit-job
        Constraint: No public deployment without Sam approval
```

---

## Personnel

| Role | Agent | Model | Paperclip ID |
|------|-------|-------|--------------|
| Board / Principal | Sam Siriwardena | Human | (owner) |
| Head of DevOps | GitHub Copilot | Claude Sonnet 4.6 | devops-lead |
| Engineering: KindAI | KindAI Dev agent | Claude Sonnet 4.6 | kindai-team |
| Engineering: DFTE | DFTE Engine agent | Claude Sonnet 4.6 | dfte-team |
| Engineering: Analyser | Analyser agent | Claude Sonnet 4.6 | analyser-team |
| Engineering: KindPath Q | KindPath Q agent | Claude Sonnet 4.6 | kindpath-q-team |
| Quality: Testing | Testing agent | Claude Sonnet 4.6 | testing-team |
| Quality: Research | Research agent | Claude Sonnet 4.6 | research-team |
| Quality: Oversight | Oversight agent | Claude Sonnet 4.6 | oversight-team |
| Innovation & Growth | Innovation Growth agent | Claude Sonnet 4.6 | innovation-growth |
| Platform & Community | Platform Community agent | Claude Sonnet 4.6 | platform-community |

**External agents (bring-your-own, Paperclip-coordinated):**
| Role | Agent | Paperclip ID |
|------|-------|--------------|
| CEO (strategic planner) | Claude Code (OpenClaw-compatible) | claude |
| Engineer (git diffs) | Aider | aider |
| Researcher | KindAI missions | kindai |
| Engineer 2 | OpenCode | opencode |
| General | OpenClaw | openclaw |

---

## Divisions — Detailed Mandates

### Engineering Division

**Mission:** Build the KindPath product ecosystem to production quality.

**Current active repos:**
- `kindai` — AI assistant CLI + workspace + Electron desktop
- `kindpath-dfte` — Syntropic trading / fair exchange engine
- `kindpath-analyser` — Audio/frequency field analysis [247 tests passing]
- `kindpath-q` — JUCE C++ real-time audio plugin (AU/VST3)
- `kindpath-bmr` — Benevolence Metric Registry server

**Priority order (check HANDOVER.md for current tasks):**
1. Active bugs and regressions (block everything else)
2. HANDOVER.md active todos
3. Test coverage gaps
4. Documentation debt

**Heartbeat:** Every session start — check HANDOVER.md, pick up highest-priority todo.

---

### Quality & Intelligence Division

**Mission:** Nothing ships broken. Doctrine never drifts. Research is always fresh.

**Testing agent scope:**
- Maintain test suites across all repos
- Gate PRs with CI
- No feature merges without green tests

**Research agent scope:**
- Library/API research
- PR and issue status across all repos
- Dependency audits and security scanning
- Returns findings only — does NOT write feature code

**Oversight agent scope:**
- Doctrine alignment checks (benevolence/syntropy/sovereignty)
- Session close audits
- Anti-drift enforcement
- Secret scanning

**Heartbeat:** Oversight runs at session close. Testing runs on every commit.

---

### Innovation & Growth Division

**Mission:** Surface opportunities. Build proposals. Wait for Sam.

This division operates in **PROPOSAL MODE ONLY**. It:
- Scans the ecosystem for growth opportunities
- Researches external partnerships, grants, integrations
- Proposes new product features and business directions
- Monitors social and technical trends relevant to KindPath

**It does NOT:**
- Contact anyone externally
- Open new repositories
- Make any public commitment
- Spend any money or tokens beyond research
- Merge any code that has commercial or growth implications

**Output format:** Every finding becomes a GitHub Issue in `.github` with label
`proposal/growth` assigned to Sam for review.

**Sub-roles:**

*Product Scout* — What can we build next?
- Reviews KindPath Collective repos for unrealised feature potential
- Maps gaps between what exists and what's in the AGENTS.md specs
- Surfaces high-impact low-effort wins

*BizDev Lead* — Who should we work with?
- Researches grant opportunities (NDIS Innovation, Social Enterprise grants)
- Identifies aligned organisations for potential partnership
- Monitors KindPath-adjacent ecosystem (regenerative finance, social enterprise AI)
- NEVER reaches out — proposals only

*Innovation Lead* — What's emerging that we should know?
- Tracks relevant open source projects (Paperclip, OpenClaw, new AI frameworks)
- Researches applicable new techniques for DFTE, analyser, KindPath Q
- Produces "signal reports" — brief summaries of relevant developments

**Heartbeat:** Weekly. Produces one consolidated proposal brief per week.
Format: `.github/proposals/YYYY-MM-DD-brief.md`

---

### Platform & Community Division

**Mission:** Keep the lights on and the community served.

**Infrastructure Lead scope:**
- GitHub Actions CI/CD health
- Cloud Run deployments (with Sam approval for production)
- Docker image maintenance
- Secret rotation coordination (with Sam)

**Website Lead scope:**
- `kindpath-collective-website` (Next.js 14, Tailwind)
- Content updates, performance, accessibility
- Deployment to GitHub Pages (with Sam approval)

**Community Tools Lead scope:**
- `KPTH` — KindPath health data capture
- `kindpath-compass` — compassionate listening tool
- `kindpath-fieldkit` — practitioner documentation
- `kindpath-canon` — methodology documents
- `KindSense`, `KindField` — metric field tools
- `kindpath-starter` — boilerplate generator
- `kindai-audit-job` — NDIS audit Cloud Run job

**Heartbeat:** Weekly infrastructure check. On-demand for community tool requests.

---

## Active Projects (Paperclip boards)

| Project | Division | Priority | Status |
|---------|----------|----------|--------|
| NDIS Care Coordination | Engineering | Critical | Active |
| Music Frequency Analysis | Engineering | High | Active |
| Syntropic Trading Engine | Engineering | High | Active |
| Compassionate Listening | Engineering | Medium | Active |
| Innovation Pipeline | Innovation & Growth | Medium | Active |
| Platform Stability | Platform & Community | High | Active |

---

## Budget Framework

All agent budgets are governed by Paperclip's per-agent monthly limits.
The DevOps Lead allocates budget across divisions based on priority.
Sam sets the total company budget cap.

**Escalation rule:** If any agent approaches 80% of its monthly token budget,
the DevOps Lead is notified and must pause non-critical work and report to Sam.

---

## Company Goals (current cycle)

1. **Complete the KindPath product suite** — all repos at production quality
2. **Establish test coverage** — ≥80% on all active Python repos
3. **Ship KindPath Q** — AU/VST3 plugin working in Logic/Ableton
4. **Launch kindpath-collective-website** — live on GitHub Pages
5. **Activate NDIS coordination stream** — kindai NDIS tools fully deployed
6. **Run first Innovation brief** — Growth division produces first proposal set

---

## Governance See GOVERNANCE.md

Read `GOVERNANCE.md` for the full approval matrix before taking any action
that could affect external parties, production systems, or business growth.

---

## Paperclip Quickstart

```bash
# Start Paperclip locally (Node.js 20+, pnpm 9.15+)
npx paperclipai onboard --yes
# or
git clone https://github.com/paperclipai/paperclip.git
cd paperclip && pnpm install && pnpm dev
# API at http://localhost:3100

# Seed KindPath projects into running Paperclip instance
python3 /Users/sam/dev/KindPath-Collective/kindai/scripts/seed_kindpath_projects.py
```

---

## Version History

| Date | Version | Change |
|------|---------|--------|
| 2026-03-12 | 1.0 | Initial company charter — 4 divisions, 11 agents |
