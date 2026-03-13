# KindPath Collective — Cross-Team Communication Protocol

**Version:** 1.0  
**Effective:** 2026-03-15  
**Authority:** DevOps Lead / Sam  
**Review cycle:** Quarterly

---

## Purpose

This protocol governs how agents and human contributors communicate across
team boundaries within the KindPath Collective. It applies to proposals,
escalations, concerns, cross-domain insights, and any communication that
moves from one team's scope into another's decision space.

Its foundation is the **Four Pillars of Holistic Perception** — the same
framework that governs KindAI reasoning and KindPress corpus calibration.
A communication that only activates one or two pillars is an **inference**.
A communication that activates three or four is an **insight**. Only
insights warrant cross-team action.

---

## The Four Pillars Applied to Communication

Before raising a cross-team communication, the originating agent or person
runs it through four verification channels:

| Pillar | Verification question | Signal that it's engaged |
|---|---|---|
| **Somatic** | Does this feel right in operational context? | The concern is grounded in observed behaviour, not hypothetical. Something concrete happened or is happening. |
| **Intellectual** | Does this hold up to evidence and logic? | There is data, a traceable reasoning chain, or a reproducible pattern supporting the claim. |
| **Emotional** | What are the relational stakes? | The communication names its impact on relationships, trust, or team cohesion rather than presenting only technical content. |
| **Spiritual** | Does this align with KindPath doctrine? | The communication is benevolent in intent, sovereignty-respecting in form, and syntropic in direction. It is not extractive (does not shift a problem onto another team without co-ownership). |

---

## Communication Tiers

### Tier 1: Inference (1–2 pillars activated)

A communication that only engages one or two pillars is inference-grade.
It may be shared as **early signal**, but must be labelled as such.

**Format:**
```
[INFERENCE] <domain> → <receiving domain>
Pillar signal: [Somatic / Intellectual / Emotional / Spiritual] (circle engaged ones)
Content: ...
Unverified: [name which pillars are not yet confirmed]
Action requested: None — hold as signal. Will escalate when verified.
```

Inference-grade communications do NOT require a response. The receiving team
logs them and monitors for convergent evidence.

---

### Tier 2: Partial Insight (3 pillars activated)

A communication that engages three pillars is ready for **collaborative
discussion**. It warrants a response but not yet a decision.

**Format:**
```
[PARTIAL INSIGHT] <domain> → <receiving domain>
Pillars engaged: [list three]
Missing: [name the unverified pillar and what would confirm it]
Content: ...
Action requested: Review and respond — does this resonate from your domain?
                  Can you confirm or challenge the missing pillar?
```

---

### Tier 3: Insight (4 pillars activated)

A communication with all four pillars engaged is **insight-grade**. It
warrants immediate consideration and may initiate action or change.

**Format:**
```
[INSIGHT] <domain> → <receiving domain>
Pillars engaged: Somatic, Intellectual, Emotional, Spiritual
Evidence summary:
  Somatic:     [what concrete behaviour was observed]
  Intellectual: [what the data or reasoning shows]
  Emotional:   [what the relational impact is]
  Spiritual:   [what doctrine alignment or tension is present]
Content: ...
Action requested: [specific proposed response or decision needed]
Urgency: [routine | priority | blocking]
```

---

## Practical Templates

### Raising a Proposal

```markdown
## Proposal: [title]

**From:** [agent/domain]  
**To:** [receiving domain or Sam]  
**Tier:** [Inference / Partial Insight / Insight]

### Pillar Audit
- [ ] Somatic: [observed basis]
- [ ] Intellectual: [evidence or reasoning]
- [ ] Emotional: [relational consideration]
- [ ] Spiritual: [doctrine alignment]

### Proposal
[Content]

### Action Requested
[What response, decision, or acknowledgement is needed]
```

---

### Raising a Blocker

```markdown
## Blocker: [title]

**From:** [agent/domain]  
**To:** [domain that can unblock]  
**Tier:** [Inference / Partial Insight / Insight]

### Pillar Audit
- [ ] Somatic: [what is concretely stuck]
- [ ] Intellectual: [what has been tried, what failed, what is understood]
- [ ] Emotional: [impact on team morale, relationships, or trust]
- [ ] Spiritual: [whether this blocker is systemic or situational]

### What Is Blocked
[Clear description]

### What Would Unblock It
[Specific ask — as precise as possible]
```

---

### Escalation to Sam

Escalations to Sam must always be **Insight-grade** (four pillars engaged)
unless there is an operational emergency.

Before escalating:
1. Confirm all four pillars are at least partially evidenced
2. Try resolution within the team or with adjacent teams first
3. Prepare a clear **action requested** — not just a problem description

Sam is not a filter for inferences. Surface inferences to the team.
Bring insights to Sam.

---

## Communication Health Monitoring

The KindPress `interact.py` tracks `pillar_balance_score` per agent session.
Aggregate pillar_balance across teams surfaces in the `community_pillar_balance`
metric in the governance synthesis report.

A team with chronically low pillar_balance_score is communicating in a way
that is imbalanced — likely over-weighted on intellectual (pure technical)
or somatic (reactive pattern-matching) without the relational and doctrinal
channels.

**Target:** Community pillar_balance_score ≥ 0.65 across all active teams.

---

## Forbidden Communication Patterns

These patterns are not aligned with the Four Pillars and must be named
and corrected:

1. **Demand without relational check**: Intellectually sound request that ignores relational impact → label as Inference, add Emotional pillar before escalating
2. **Urgency inflation**: Framing an inference as a blocker to bypass the tier system → name the actual evidence level
3. **Silent compliance**: Receiving a cross-team insight and not responding → breaks the collaboration loop; respond even if only to acknowledge
4. **Extractive escalation**: Passing a problem upward without proposing a path or co-owning the solution → proposals require an **Action Requested** that the originator is also prepared to participate in

---

## Relationship to KindPress

Every cross-team interaction is a signal in the KindPress interaction model.
High `pillar_balance_score` in interaction packets means teams are reasoning
holistically before communicating. Low scores reveal which teams are producing
inference-grade outputs without recognising them as such.

The doctrine is: an insight requires convergence across all four pillars.
This protocol is the structural implementation of that doctrine in the
communication layer of the organisation.

---

*This document is the living protocol for cross-team communications in
the KindPath Collective. It is maintained by the DevOps Lead and should
be reviewed whenever the charter, doctrine, or team structure evolves.*
