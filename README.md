# 2uit

Connecting clients with highly skilled engineers — a platform built around one idea: **evidence should be earned, not computed, and never bought.**

This repo is a collection of React frontend prototypes. There is no backend yet — see [Enterprise Readiness](#enterprise-readiness) below for what that means in practice.

---

## Core concept

Engineers post **blueprints** (their work and reasoning), earn **intents** (colleagues backing the work), and prove it with **evidence** — which only locks after a human reviewer approves it. Locked evidence gets a **fingerprint**: a manually, sequentially assigned mark, not a computed hash. Nothing here is cryptographic, and nothing of value can be purchased — it has to be demonstrated.

```
Blueprint → Intent → Evidence submitted → Human review → Locked (fingerprint issued)
```

---

## The frontends (16)

| # | File | What it's for |
|---|---|---|
| 1 | `2uit-app.jsx` | **Main app.** Feed, contracts, experiments, evidence, messaging, principles — the engineer's home base. |
| 2 | `2uit-onboarding.jsx` | Intro slides (Blueprint → Intent → Evidence) plus sign up / sign in. |
| 3 | `2uit-review-console.jsx` | Where a human approves or rejects pending evidence. Nothing locks without this. |
| 4 | `2uit-verify.jsx` | Public page — paste a fingerprint, confirm a locked record is genuine. No login needed. |
| 5 | `2uit-client-portal.jsx` | Clients browse engineers as "case files," see locked evidence as "exhibits," and request work. Includes an AI Background Lens. |
| 6 | `2uit-org-console.jsx` | Multi-org admin: members, seats, and full RBAC (5 resources × 4 permission levels, built-in + custom roles). |
| 7 | `2uit-network-providers.jsx` | Directory of infra/network vendors (cloud, CDN, DNS, observability, CI/CD, incident routing, security, local wireless), each with a proprietary 2NPI index code. |
| 8 | `2uit-code-ai-environment.jsx` | A staging workspace with a **real** Claude-powered pairing assistant reviewing your code. |
| 9 | `2uit-collectibles.jsx` | Trophy case of achievement badges — every one earned by a real milestone, never bought or randomized. |
| 10 | `2uit-builder.jsx` | Structured blueprint drafting tool (title, body, constraints, tags) with live preview, styled as literal blueprint paper. |
| 11 | `2uit-helper.jsx` | Help center — FAQ plus a **real** AI assistant grounded in how the platform actually works. |
| 12 | `2uit-mall.jsx` | Marketplace, deliberately split: **Shop** (ordinary paid add-ons — seats, storage) and **Redeem** (spend earned collectibles on perks — never buyable). |
| 13 | `2uit-bpo-protocol.jsx` | Coverage operations: escalation tiers, SLA tracking, shift schedule, shift handoff notes. |
| 14 | `2uit-mason.jsx` | Guild rank progression (Apprentice → Journeyman → Master), modeled on historical stonemason guilds — peer-endorsed advancement, not self-declared. |
| 15 | `2uit-pintor.jsx` | The Painter's Guild — a gallery of visual work (diagrams, mockups, sketches), "studied" rather than liked. |
| 16 | *(this file)* | `README.md` |

Supporting, non-app files:
- `generate_certificate.py` — produces a downloadable PDF certificate for a locked evidence record.
- `2uit-button-manual.md` — every clickable element in the main app and what it opens.
- `2uit-system-documentation.md` — architecture notes as of the interoperability build-out.
- `2uit-enterprise-readiness-assessment.md` — honest gap analysis against production/enterprise standards.

---

## Why so many separate files?

Each surface has a different audience and job, so each gets its own visual identity on purpose — an engineer's dashboard shouldn't look like a client's due-diligence tool, and a public verification page shouldn't look like an internal admin console. The full list, at a glance:

| Surface | Audience | Register |
|---|---|---|
| Main app | Engineers | Dark, engineering-focused |
| Onboarding | New users | Same family as main app |
| Review Console | Internal reviewers | Dark ops/terminal |
| Verify | The public | Light, notarized-document |
| Client Portal | Clients | Case-file / dossier |
| Org Console | Org admins | Clean, indigo, enterprise SaaS |
| Network Providers | Org admins | Dark, cyan, network-topology |
| Code AI Environment | Engineers, mid-work | Dark, violet, IDE-like |
| Collectibles | Engineers | Warm, brass, trophy-case |
| Builder | Engineers, drafting | Literal blueprint paper |
| Helper | Anyone stuck | Light, friendly, coral |
| Mall | Org admins / engineers | Light, magenta/gold, split shop |
| BPO Protocol | Ops/coverage staff | Dark, blue, NOC-style |
| Mason | Engineers | Stone/bronze, guild |
| Pintor | Engineers | Cream, gallery-wall |

---

## Interoperability

There's no real backend, so cross-surface sync uses the artifact platform's shared key-value storage, polled every 3.5–4 seconds:

| Key | Written by | Read by |
|---|---|---|
| `2uit:pending-evidence` | Main app (on submission) | Review Console |
| `2uit:evidence-ledger` | Review Console (on approval) | Main app, Verify, Client Portal |

This is honestly a prototype-grade mechanism — it demonstrates the right shape (submit → review → lock → verify) but isn't how a production system would actually sync data. See the [system documentation](./2uit-system-documentation.md) for the full picture, and the [enterprise readiness assessment](./2uit-enterprise-readiness-assessment.md) for what's still missing.

---

## Design rule that applies everywhere

**Fingerprints are earned, not computed.** Every place that issues one uses the same sequential, manually-assigned scheme (`2UIT-YYYYMMDD-####`) — never a hash, never anything derived from file contents. The point is that a fingerprint means something because of the obstacle behind it (staging, development, human review), not because a formula ran against some bytes. This rule extends everywhere the platform "earns" something: Collectibles can't be bought, Mall's Redeem tab only spends what you've already earned, and guild rank in Mason requires peer endorsement, not self-declaration.

---

## Enterprise Readiness

Short version: **these are UI prototypes**, roughly 15–20% of the way to production/enterprise-grade software. There's no real backend, database, authentication, encryption, or compliance work behind any of this — the shared storage used for "sync" is a demo-level mechanism, not production infrastructure. What's here is meant to serve as a specification for an actual engineering team to build from. Full breakdown in [`2uit-enterprise-readiness-assessment.md`](./2uit-enterprise-readiness-assessment.md).

---

## Running these

Each `.jsx` file is a standalone React component (default export) built for the Claude artifact environment — Tailwind utility classes, `lucide-react` icons, no external routing. To view one, open it as an artifact. To adapt it elsewhere, each file is self-contained aside from the shared design-token pattern (`c` / `ink` / `mono` etc. at the top of each file) and, where noted, calls to `window.storage` (artifact-only) or `fetch("https://api.anthropic.com/v1/messages")` (Code AI Environment, Helper, Client Portal's AI Background Lens).
