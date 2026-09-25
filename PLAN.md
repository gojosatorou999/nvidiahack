# Greenlight — Project Plan

**Nebius x NVIDIA Global AI Hackathon**
Track: **Coding and Agentic Engineering** · Bonus target: **Best Use of Tavily**

> Dependency upgrades that break your build: fixed, proven, and opened as a pull request.

---

## Contents

1. [The hackathon at a glance](#1-the-hackathon-at-a-glance)
2. [Choosing a track: pros and cons](#2-choosing-a-track-pros-and-cons)
3. [Decision](#3-decision)
4. [The product: Greenlight](#4-the-product-greenlight)
5. [How it works](#5-how-it-works)
6. [Model strategy: Nemotron tiering](#6-model-strategy-nemotron-tiering)
7. [How we beat the other entries](#7-how-we-beat-the-other-entries)
8. [Infrastructure: what runs where](#8-infrastructure-what-runs-where)
9. [Tech stack](#9-tech-stack)
10. [Repository layout](#10-repository-layout)
11. [Mapping to the judging criteria](#11-mapping-to-the-judging-criteria)
12. [Risks and mitigations](#12-risks-and-mitigations)
13. [Submission checklist](#13-submission-checklist)

---

## 1. The hackathon at a glance

| | |
|---|---|
| **Hard requirement** | Must make a runtime call to **Nebius Token Factory**, or run on **Nebius AI Cloud** compute (Serverless Jobs, Serverless Endpoints, DevPods), **and** use at least one **NVIDIA open-source model** |
| **Competition** | ~11,900 registrants worldwide |
| **Judges** | ~20 engineers, PMs and developer advocates from NVIDIA, Nebius and Tavily |
| **Judging** | Stage 1 is pass/fail: a genuine attempt at the track, reasonable use of the required APIs. Stage 2 has four **equally weighted** criteria: Technological Implementation, Design, Potential Impact, Quality of the Idea |
| **Tie-break order** | Technological Implementation first, then Design, then Impact, then Idea |

### Prizes

| Prize | Amount | Count |
|---|---|---|
| Grand Prize | $20,000 | 1 |
| 2nd Place | $10,000 | 1 |
| 3rd Place | $6,000 | 1 |
| Track Winner (per track) | NVIDIA Jetson Orin Nano | 4 |
| Best Use of Tavily | $3,000 | 1 |
| City Winner (Builders & Brews attendees) | $500 | 20 |
| Most Valuable Feedback | $100 + NVIDIA swag | 10 |

**Stacking rule:** a project can win **one Overall award, or one Track award plus one Bonus award**. We aim for the Grand Prize. The fallback is Track Winner plus Best Use of Tavily.

---

## 2. Choosing a track: pros and cons

### A. Coding and Agentic Engineering
*Build coding agents and developer tools: agents that write, run and test code in Token Factory Sandboxes.*

**Pros**
- **It showcases the sponsor's newest product.** Sandboxes are in beta. The standout feature is **checkpoint and branch**: save a sandbox state, fork it, try several things in parallel, roll back instantly. Nebius wants to see this used well, and most entrants will use the sandbox only as "a place to run code".
- **The judges are the users.** NVIDIA and Nebius engineers feel dev-tool pain themselves, so a sharp developer problem makes the Impact case easy.
- **Results can be measured.** Tests pass or fail, so we can report hard numbers (fix rate, cost per fix). That kind of evidence is rare in hackathons and judges trust it.
- **Tavily fits naturally.** Code agents need current docs and changelogs, which keeps the $3k bonus in play.
- **It demos well.** Parallel branches going red or green make a clear, visual 3-minute video.

**Cons**
- **Generic coding agents are the default hackathon idea.** The track will have many "AI code reviewer" and "write my tests" entries, so we must be narrow and differentiated.
- **The Sandboxes API is beta.** Limits apply (50 simultaneous operations), docs are thin, and behaviour may change.
- **Nebius already has its own demos**, including an agent that repairs a failing repo by searching a tree of sandbox states. We must not look like a copy of their demo.

### B. Best Apps and Agents
*Build any app or agent someone would actually use, powered by Nemotron via Token Factory (Ultra for reasoning, Nano/Super for fast calls).*

**Pros**
- **Maximum freedom.** Any domain, any audience.
- **Low technical barrier.** It only needs Token Factory API calls, so there's no beta tooling to fight.
- **It rewards product polish,** and the Design criterion favours a complete, well-designed app.
- **Tavily is easy to integrate** for research or search-style apps.

**Cons**
- **The most crowded track.** With the loosest requirements, it attracts the most generic entries: chat-with-your-PDF, travel planners, study buddies.
- **It's hard to be "non-obvious".** The Quality of the Idea bar is highest where everyone is allowed to build anything.
- **It's a weak showcase of Nebius tech.** Calling an LLM API is table stakes, so Technological Implementation is harder to max out.
- **Impact claims are usually soft,** because consumer apps rarely have measurable proof they work.

### C. Personal AI
*An always-on, private assistant with persistent memory, reusable skills and tool access, using NVIDIA NemoClaw, OpenShell, Hermes Agent and Nebius Serverless.*

**Pros**
- **Probably less crowded.** The required tooling is newer and harder, which filters out casual entrants.
- **A strong narrative.** "Your data, your control" and open models are exactly what NVIDIA and Nebius market.
- **Several sponsor tools at once** (NemoClaw, OpenShell, Hermes Agent, Serverless) makes a big Technological Implementation story.

**Cons**
- **Heavy setup, little documentation.** NemoClaw, OpenShell and Hermes Agent are new, and the integration risk is high.
- **"Always-on" is hard to demonstrate.** Persistent memory and long-running autonomy don't show well in 3 minutes, and a judge can't feel it in a quick test.
- **It's hard to host for judges.** A private, personal system conflicts with giving judges a public working demo URL.
- **Personal assistants all look alike.** "It reads my email and manages my calendar" is a familiar pitch, and Impact is hard to prove.

### D. Physical AI (considered, rejected)
Robotics, IoT and edge work with GR00T, Cosmos and similar models. It's probably the least crowded, but it needs hardware or a serious simulation pipeline, and at least a minute of footage of the physical system operating. The build cost is too high for the payoff.

### Scorecard

| Criterion | Coding & Agentic | Best Apps & Agents | Personal AI |
|---|---|---|---|
| Crowding (lower is better) | Medium-high | **Very high** | Medium |
| Room for a non-obvious idea | **High** (via branching) | Low | Medium |
| Showcases Nebius tech | **High** (Sandboxes + Token Factory + Serverless) | Low-medium | High |
| Measurable impact | **High** (tests pass or fail) | Low | Low |
| Demo clarity in 3 min | **High** | Medium | Low |
| Judge-testable hosted demo | **High** | High | Low |
| Tavily bonus fit | **High** | High | Medium |
| Build risk | Medium (beta API) | **Low** | High |

---

## 3. Decision

**Track: Coding and Agentic Engineering.**

It is the only track where a single idea can max out all four judging criteria together: a sponsor-feature-driven implementation, a measurable real-world impact, a clearly non-obvious use of branching, and a demo judges can test themselves. To avoid the "generic coding agent" trap, we go **narrow and deep** on one painful, specific problem, and we make **sandbox branching the core of the product**, not an accessory.

---

## 4. The product: Greenlight

### The problem
Dependabot and Renovate open dependency-upgrade PRs constantly. The ones that matter most are **security upgrades**. When an upgrade crosses a breaking change (Pydantic v1→v2, SQLAlchemy 1.4→2.0, Next.js 14→15, major Jest/ESLint/React releases), CI goes red and the PR **sits untouched for weeks or months**. Nobody has time for the migration, so teams knowingly run code with published CVEs.

**Audience:** engineering teams and open-source maintainers who get more upgrade PRs than they can fix by hand.

### Why an LLM alone can't solve this (and why Greenlight can)

| Failure mode of a plain LLM | What Greenlight does | Sponsor tech |
|---|---|---|
| The model **doesn't know the new version**, because the breaking changes shipped after its training cutoff | Pulls the real changelog, migration guide and related GitHub issues at runtime | **Tavily** |
| The **first fix is usually wrong**, and there are several plausible migration strategies | Installs once, **checkpoints**, then **forks N branches** that each try a different strategy in parallel. Dead ends roll back to their last good checkpoint, not to the start | **Token Factory Sandboxes** |
| The model **claims** it fixed things | Every branch runs the repo's **real test suite** in an isolated microVM. Only green branches qualify | **Token Factory Sandboxes** |
| Big repos don't fit a context window, and planning needs deep reasoning | The planner reads the whole repo plus the breaking-change list in one pass (1M-token context) | **Nemotron 3 Ultra** |

### What the user gets
A pull request that contains:
- the minimal diff that makes tests pass on the new version
- **test evidence**: before (red) and after (green) logs from the sandbox
- **sources**: links to the changelog and migration guide sections it relied on
- a short **branch report**: which strategies were tried, why the winner was chosen, and what each model tier cost

---

## 5. How it works

```
Input: GitHub PR URL (failing Dependabot bump) or repo URL + "upgrade X to vY"
        │
        ▼
┌─ RESEARCH ─────────────────────────────────────────────────────────┐
│ [Nano]   parse failing CI log → classify errors                    │
│ [Tavily] fetch changelog, migration guide, related issues          │
│ [Nano]   condense → "breaking changes that affect THIS repo"       │
└────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─ BASELINE ─────────────────────────────────────────────────────────┐
│ [Sandbox] clone → install new version → run tests                  │
│           → CHECKPOINT "baseline-red"                              │
└────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─ PLAN ─────────────────────────────────────────────────────────────┐
│ [Ultra] read repo + breaking changes + failures                    │
│         → propose 3–6 DISTINCT fix strategies                      │
│         (e.g. rewrite call sites · compatibility shim ·            │
│          config change · pin transitive dep · adapter layer)       │
└────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─ EXPLORE (parallel, forked from "baseline-red") ───────────────────┐
│  branch A [Super] edit → test → checkpoint → iterate (≤ k steps)   │
│  branch B [Super] edit → test → checkpoint → iterate               │
│  branch C [Super] ...                                              │
│  dead end → roll back to that branch's last good checkpoint        │
│  promising sub-path → fork again (tree, not list)                  │
└────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─ JUDGE ────────────────────────────────────────────────────────────┐
│ [Ultra] compare green branches: diff size, touched surface,        │
│         test coverage, risk → pick winner + explain tradeoffs      │
└────────────────────────────────────────────────────────────────────┘
        │
        ▼
Output: PR with diff + test evidence + sources + branch report
```

Every step emits events that stream live to the web UI, which draws the branch tree as it grows.

---

## 6. Model strategy: Nemotron tiering

The track brief explicitly encourages using Ultra for serious reasoning and Nano/Super for fast, everyday calls. Greenlight does exactly that, and shows the cost split in the UI.

| Model | Role | Why this tier |
|---|---|---|
| **Nemotron 3 Nano** (30B, MoE) | Log triage, error classification, summarising Tavily pages and test output | High volume, low stakes: needs to be fast and cheap |
| **Nemotron 3 Super** (120B, MoE) | Patch-writing worker inside each branch | Strong at code, affordable enough to run N times in parallel |
| **Nemotron 3 Ultra** (550B, MoE, 1M context) | Planner (strategy generation) and judge (winner selection) | Few calls, heaviest reasoning, whole-repo context |

- All calls go through Token Factory's **OpenAI-compatible API**.
- Exact model IDs are confirmed at build time via `GET /v1/models`.
- A single **model router** module owns the tier choice, so it can be changed in one place (for example, Super standing in for Ultra during development to save credits).
- If Token Factory offers NVIDIA embedding and reranker models, use them for repo search. This is optional.

---

## 7. How we beat the other entries

1. **A real eval, not a cherry-picked demo.** Build a dataset of **20–30 real failing dependency-upgrade PRs** from public GitHub repos (Python first, then JS). Report **fix rate, cost per fix, and time per fix**, compared against a **single-shot baseline** (same models, no branching, no Tavily). This shows that branching and Tavily each measurably help. Almost no hackathon entry ships an ablation table.
2. **Real merged PRs.** Open Greenlight fixes on real open-source repos where upgrade PRs are stuck, clearly disclosed as AI-assisted and only where the fix is genuinely correct. "N PRs merged by maintainers" is the strongest possible Impact evidence.
3. **A live branch tree.** A React Flow graph where branches fork, turn red or green, and roll back in real time. It's the hero shot of the demo video and makes the Sandboxes feature instantly understandable.
4. **Transparent cost.** Per-run token and dollar breakdown by model tier, which proves the Nano/Super/Ultra split pays off.
5. **A judge-proof hosted demo.** Judges can pick a preloaded example or paste a public repo. Recorded **replay mode** keeps the demo working even if credits or the beta sandbox misbehave during the judging period.
6. **Narrow scope, done completely.** One problem, solved end-to-end with evidence, beats a broad agent that half-works.

---

## 8. Infrastructure: what runs where

**Yes, this runs on the cloud, but we never need our own GPU.** All models are served by Nebius. We develop locally on a normal laptop.

| Component | Runs on | Notes |
|---|---|---|
| LLM inference | **Nebius Token Factory** | Required runtime call; OpenAI-compatible API |
| Code execution | **Token Factory Sandboxes** (ConTree) | Required for this track; checkpoint, branch, rollback |
| Web research | **Tavily API** | Changelogs, migration guides, issues |
| Backend API | **Nebius Serverless Endpoint** (Docker) | Encouraged by the brief; counts toward Nebius usage |
| Agent runs (long, async) | **Nebius Serverless Jobs** | One job per fix run. Fallback: background worker in the API container |
| Frontend | Vercel or Cloudflare Pages | Static Next.js; hosting location doesn't affect judging |
| Database | Postgres (Neon/Supabase free tier) or SQLite | Runs, branch trees, event logs, eval results |
| GitHub integration | GitHub App | Read PRs and CI logs, open fix PRs |

### Credits
- $25 Token Factory credits via the hackathon promo code
- $25 more via the Nebius Builders Program (also includes Tavily credits)
- Extra credits at Builders & Brews city events

**Credit discipline:** use Super as a stand-in for Ultra during development, cache LLM responses keyed by prompt hash, cap branches per run, and rate-limit the public demo.

---

## 9. Tech stack

| Layer | Choice | Why |
|---|---|---|
| Agent and backend language | **Python 3.12** | ConTree SDK and Tavily SDK are Python-first |
| LLM client | **`openai` SDK** with `base_url` set to Token Factory | Token Factory is OpenAI-compatible; standard tool calling and structured output |
| Orchestration | **Hand-written asyncio orchestrator** + **Pydantic** schemas (plan, strategy, patch, verdict) | Full control and easy debugging; judges can read the code. No LangChain |
| Sandboxes | **`contree_client`** (ConTree SDK, async) | Checkpoint, branch and run commands in microVMs |
| Research | **`tavily-python`** | Runtime web research; Tavily bonus eligibility |
| API | **FastAPI** + **Server-Sent Events** | Streams branch events live to the UI |
| GitHub | **GitHub App** via `githubkit` | Webhooks on failing bump PRs; opens fix PRs |
| Frontend | **Next.js** + **Tailwind** + **shadcn/ui** | Fast to build, polished result for the Design criterion |
| Visualisation | **React Flow** (branch tree) + a diff viewer | The core visual of the product |
| Packaging and deploy | **Docker**, Nebius Serverless Endpoints and Jobs | Nebius-native deployment |
| CI | **GitHub Actions** | Lint, tests, image builds |
| License | **Apache-2.0** | Required: an open-source license visible in the repo's About section |

---

## 10. Repository layout

```
greenlight/
├── agent/          # orchestrator, strategy planner, workers, judge, model router, prompts
├── sandbox/        # ConTree wrapper: create, checkpoint, fork, rollback, run_tests
├── research/       # Tavily client + Nano summariser → breaking-change list
├── api/            # FastAPI app, SSE stream, GitHub App webhooks
├── web/            # Next.js UI: new run, live branch tree, diff, evidence, cost
├── eval/           # dataset of failing bump PRs, runner, baseline, results table
├── docs/           # architecture diagram, screenshots
├── FEEDBACK.md     # running notes on Nebius/NVIDIA tooling (for the feedback prize)
├── Dockerfile
├── LICENSE         # Apache-2.0
└── README.md
```

---

## 11. Mapping to the judging criteria

| Criterion (equal weight) | How Greenlight scores |
|---|---|
| **Technological Implementation** | Three Nemotron tiers with clear roles; Sandboxes checkpoint and branch as the core mechanism; Token Factory for all inference; Serverless Endpoints and Jobs for deployment; Tavily for fresh knowledge; an eval harness with an ablation |
| **Design** | A complete product loop: submit a PR, watch the live branch tree, review diff and evidence, open the fix PR. Polished UI, cost transparency, replay mode |
| **Potential Impact** | A specific audience (teams with stuck security upgrades), a specific pain (red Dependabot PRs and unpatched CVEs), and proof: fix rate on real PRs plus maintainer-merged PRs |
| **Quality of the Idea** | Non-obvious: uses branching as *parallel hypothesis testing* over migration strategies, and Tavily to cover the model's knowledge-cutoff gap. Shows a real understanding of why upgrades are hard |

---

## 12. Risks and mitigations

| Risk | Mitigation |
|---|---|
| The Sandboxes beta doesn't support branching the way we need | Validate checkpoint and fork **first**, before building anything else. If it falls short, adapt the design around what the SDK does support |
| Beta limits (50 simultaneous operations) | Cap branches per run (~6); queue runs |
| Credits run out | Super stands in for Ultra during development; response caching; branch caps; replay mode for the demo |
| Other entrants build "a coding agent" | Stay narrow (dependency upgrades only), measured (eval + ablation), and proven (merged PRs) |
| The demo breaks during judging | Recorded replays; clear README with local-run instructions; health checks on the hosted demo |
| Repos with slow or flaky test suites | Prefer repos with fast suites for the eval; per-branch time limits; flag flakiness by re-running the baseline |
| Scope creep | Python repos only until the Python eval is solid; JS is a stretch goal |
| An open-source PR is wrong or unwanted | Human review before opening any PR; clear AI-assisted disclosure; only target PRs where the fix is verified |

---

## 13. Submission checklist

- [ ] Working project on Nebius Token Factory using NVIDIA Nemotron models
- [ ] Track selected: **Coding and Agentic Engineering**
- [ ] Project description: what it is, why we built it, how it works
- [ ] **Hosted demo URL** that judges can use for free, with no restrictions, through the judging period
- [ ] **Demo video** under 3 minutes, public on YouTube, with narration covering how Token Factory and Nemotron are used, and no copyrighted music
- [ ] **Public GitHub repo** with an **Apache-2.0 LICENSE** visible in the About section
- [ ] **README** with setup instructions, how to run, architecture, eval results, and a clear section on NVIDIA model usage, where Token Factory helped, and other Nebius services used
- [ ] **Feedback** on Token Factory, AI Cloud and NVIDIA tools (drawn from `FEEDBACK.md`)
- [ ] Runtime **Tavily** call in the solution (Best Use of Tavily eligibility)
- [ ] City selected, if we attended a Builders & Brews event
