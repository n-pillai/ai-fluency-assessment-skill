# AI Fluency Assessment — Reference

Framework definitions, scoring rubrics, and behavioral indicators for both assessment frameworks.

---

## Framework 1: Anthropic 4D AI Fluency Framework

Source: AI Fluency Index (Anthropic, February 2026)

### The Four Competencies

**Delegation** — Deciding what to give AI vs. do yourself
The ability to identify which tasks benefit from AI assistance and calibrate the scope of that assistance appropriately. High-fluency delegators give AI enough context and latitude to be genuinely useful, while retaining decisions that require human judgment or accountability.

**Description** — Communicating effectively with AI
The ability to specify goals, constraints, format, tone, and examples clearly. High-fluency describers don't rely on the AI to guess intent; they invest in precise setup and iterate on prompts deliberately.

**Discernment** — Evaluating AI outputs critically
The ability to assess AI outputs for accuracy, completeness, and reasoning quality — and to push back when warranted. High-fluency discerners question outputs proactively, especially polished or confident-sounding ones.

**Diligence** — Responsible and ethical AI use
The ability to use AI in ways that are safe, accurate, and appropriate. High-fluency diligent users verify facts, protect sensitive data, and govern AI behavior systematically rather than ad hoc.

---

### 11 Observable Behavioral Indicators (AI Fluency Index, Feb 2026)

Prevalence rates from the Index study (n=~2,000 users):

| Indicator | Competency | Prevalence | Notes |
|---|---|---|---|
| Iteration and refinement | Description | 85.7% | Most common behavior |
| Clarifying goals before prompting | Description | ~70% | |
| Specifying output format | Description | ~65% | |
| Providing examples (few-shot) | Description | ~55% | |
| Identifying missing context | Discernment | ~60% | |
| Checking facts in AI outputs | Discernment | ~55% | |
| Questioning AI reasoning | Discernment | ~50% | Drops significantly post-polished output |
| Setting interaction terms / constraints | Description | 30% | Only 30% of users do this |
| Appropriate task delegation | Delegation | — | |
| Scope calibration (not over-delegating) | Delegation | — | |
| Responsible data handling | Diligence | — | |

**Artifact-Passivity Pattern (critical to track):**
When AI produces polished, confident-looking outputs, users become measurably less critical:
- Questioning reasoning: -3.1 percentage points
- Checking facts: -3.7 percentage points
- Identifying missing context: -5.2 percentage points

A strong Discernment score requires evidence of counter-passivity behaviors: editing AI outputs, requesting reasoning explanations, catching errors, or deliberately stress-testing outputs.

---

### Scoring Rubric — 4D Framework

#### Delegation

| Rating | Behavioral Evidence |
|---|---|
| Developing | Uses AI reactively; no consistent framework for what to delegate |
| Practitioner | Delegates routine tasks; retains judgment calls; some miscalibration |
| Advanced | Clear mental model of AI vs. human tasks; adjusts autonomy per project |
| Expert | Structured delegation: governance docs, autonomy tiers, deliberate scope-setting |
| Exceptional | Delegation is architecturally embedded — agent infrastructure, skill layers, workflow governance that persists across sessions and projects |

#### Description

| Rating | Behavioral Evidence |
|---|---|
| Developing | Vague prompts; relies on AI to interpret intent; minimal iteration |
| Practitioner | States goals clearly; specifies format sometimes; iterates when output misses |
| Advanced | Consistently specifies format, tone, constraints; gives examples; anticipates ambiguity |
| Expert | Prompt design as deliberate craft; interaction terms set upfront; context-rich setup |
| Exceptional | Description is systemic — persistent instruction layers (CLAUDE.md, SKILL.md files), conventional prompt formats, cross-session context preservation |

#### Discernment

| Rating | Behavioral Evidence |
|---|---|
| Developing | Accepts AI outputs at face value; rarely revises or pushes back |
| Practitioner | Notices obvious errors; revises when output is clearly wrong |
| Advanced | Proactively checks facts; asks for clarification; catches reasoning gaps |
| Expert | Questions AI logic even on polished outputs; establishes explicit review protocols |
| Exceptional | Discernment is institutionalized — documented anti-patterns, review checklists, structured QA steps, governance rules that prevent over-trust |

#### Diligence

| Rating | Behavioral Evidence |
|---|---|
| Developing | Uses AI without regard for data sensitivity or accuracy verification |
| Practitioner | Avoids obviously sensitive data; verifies critical facts |
| Advanced | Consistent privacy hygiene; validates AI outputs before acting |
| Expert | Systematic governance: secrets protection, documentation rules, privacy classifications |
| Exceptional | Diligence is infrastructure-level — pre-commit hooks, repo visibility controls, documented privacy rules, automated compliance enforcement |

---

## Framework 2: Yegge's 8-Stage Developer-Agent Evolution Model

Source: Steve Yegge's developer-agent evolution framework

### Stage Definitions

| Stage | Label | Observable Behaviors |
|---|---|---|
| 1 | Zero or near-zero AI | Occasional code completions, asking chat questions. AI is a search engine replacement. |
| 2 | Coding agent in IDE, permissions on | Using GitHub Copilot, Cursor, or CC IDE plugin with file access enabled. Agents can read/write files. |
| 3 | Agent in IDE, YOLO mode | Auto-accept enabled; agent runs without confirmation on each step. Starting to trust the agent. |
| 4 | In IDE, wide agent — code is just diffs | Agent handles multi-file changes; developer reviews diffs rather than writing code. |
| 5 | CLI, single agent, YOLO | Primary interface is CLI (not IDE plugin). Headless execution. Diffs scroll by. Agent runs scripts, pipelines, and maintenance tasks without supervision. |
| 6 | CLI, multi-agent, YOLO — 3-5 parallel instances | Multiple CC instances running simultaneously on independent tasks. Developer coordinates agents rather than doing the work. |
| 7 | 10+ agents, hand-managed | Significant agent coordination overhead. Monitoring, debugging, and steering multiple simultaneous workstreams. |
| 8 | Building your own orchestrator | Custom orchestration layer spawning and coordinating agents programmatically. The developer's primary artifact is agent infrastructure, not application code. |

### Stage Transition Indicators

**Stage 4 → 5:** Moved from IDE plugin to CLI as primary interface. Comfortable with headless execution and scrolling diffs.

**Stage 5 → 6:** Running 3+ CC instances simultaneously on parallel tasks. Distinct tasks that can be independently executed. Developer's job shifts from "do the work" to "assign and review."

**Stage 6 → 7:** Scale and coordination complexity increases. Needs tooling to track which agents are doing what.

**Stage 7 → 8:** Builds custom infrastructure to spawn, coordinate, and monitor agents without hand-management. Agents spawn agents.

### Fractional Stages

Fractional stages (e.g., 5.5) indicate:
- Clearly past the lower stage
- Exhibiting some behaviors of the higher stage
- Missing the defining characteristic of the higher stage

Example: Stage 5.5 = CLI-primary with headless execution AND occasional parallel usage, but not yet systematic or routine multi-agent coordination.

---

## Delta Tracking Guidelines

When comparing assessments:
- Note specific new behaviors, not just rating changes
- For Yegge, identify the specific gap to next stage
- For 4D, identify which behavioral indicators are newly present or absent
- Development edges should be specific enough to be falsifiable next quarter
