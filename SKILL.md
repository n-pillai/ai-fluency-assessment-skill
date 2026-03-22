---
name: ai-fluency-assessment
description: Run /assess-fluency to score AI fluency against Anthropic's 4D Framework (Delegation, Description, Discernment, Diligence) and Yegge's 8-Stage Developer-Agent Evolution Model. Generates dated reports, tracks progress across quarterly assessments, and produces .docx output. Use when asked to assess AI fluency, run a fluency check, compare to a past assessment, generate an AI skills report, or check Yegge stage.
---

# AI Fluency Assessment Skill

Quarterly assessment of AI fluency across two frameworks: Anthropic's 4D AI Fluency Framework and Yegge's 8-Stage Developer-Agent Evolution Model. Tracks progress over time with structured JSON history and generates dated .docx reports.

## When to Use

This skill triggers when the user:
- Runs `/assess-fluency`
- Asks to run, update, or compare an AI fluency assessment
- Wants to track AI skill progression over time
- Asks "where am I on the Yegge scale?" or "how has my AI fluency changed?"
- Says "run my quarterly assessment" or "time for a fluency check"

## Commands

| Command | What it does |
|---|---|
| `/assess-fluency` | Full assessment: gather data, score frameworks, diff against last, generate report |
| `/assess-fluency --quick` | Output the data-gathering prompts only, no report generation |
| `/assess-fluency --compare` | Load and diff the two most recent assessments without scoring new data |

---

## Full Assessment Workflow

### Step 1: Load Previous Assessment

Read the most recent file in `assessments/` (sorted by filename date, YYYY-MM-DD). Extract:
- Previous Yegge stage and label
- Previous 4D ratings per competency
- Previous project count and automation count
- Open development edges and their status
- Assessment date (to use as `[LAST_DATE]` in prompts)

If no previous assessment exists, note this is the baseline run.

---

### Step 2: Output Data-Gathering Prompts

Tell the user: "Run each of these in a separate session, then paste the outputs back here."

**Prompt A — Claude Code self-assessment:**

```
I'm running my periodic AI fluency assessment (last assessment: [LAST_DATE]).

Please generate a summary of my current Claude Code usage:

1. Projects: List each project/repo with one-line description, current status (active/maintained/dormant), and autonomy level (High = CC runs independently, I review / Medium = collaborative / Lower = I direct each step)
2. Technical stack: Languages, frameworks, APIs, infrastructure in active use
3. Automation: All cron jobs, scheduled tasks, dispatch workflows, headless scripts — with trigger and description
4. Multi-agent usage: Any parallel Claude Code instances? How are they coordinated?
5. New since [LAST_DATE]: New projects, skills, automation, workflow changes, or new domains explored
6. Yegge stage estimate: Based on the 8-stage model (Stage 1: zero AI → Stage 8: own orchestrator), where would you place my current usage?

Output as JSON with keys:
- projects: array of {name, description, status, autonomy}
- tech_stack: array of strings
- automation: array of {name, trigger, description}
- multi_agent: string description
- new_since_last: array of strings
- yegge_estimate: number (can be fractional, e.g. 5.5)
- yegge_rationale: string
```

**Prompt B — Other AI tools self-assessment:**

```
I'm running my periodic AI fluency assessment (last assessment: [LAST_DATE]).

Please summarize my usage of AI tools and automation beyond Claude Code. This includes tools like ChatGPT, Copilot, Gemini, custom GPTs, AI-powered automation platforms (Zapier, Make, n8n), scheduling agents, or any AI-integrated workflows you run.

1. Active automations or scheduled tasks: name, trigger, description, complexity
2. Integrations: which tools, services, or APIs are connected to your AI workflows
3. File or content artifacts: documents, outputs, or content these tools generate autonomously
4. Automation complexity: simple triggers vs. multi-step workflows
5. New since [LAST_DATE]: anything added or changed

If you don't use other AI tools beyond Claude Code, describe any standalone automations or scheduled scripts you run (cron jobs, task schedulers, headless scripts).

Output as JSON with keys:
- automations: array of {name, trigger, description, complexity}
- integrations: array of strings
- file_artifacts: array of strings
- automation_complexity: string
- new_since_last: array of strings
```

**Checklist C — Claude.ai manual notes (paste as plain text):**

```
Before the assessment, review your recent Claude.ai chat history and note:

□ Types of tasks delegated vs. retained (what did you keep for yourself?)
□ Instances where you caught an error, pushed back, or asked for a revision
□ Format, tone, or constraint specifications you gave
□ New domains or problem types you explored
□ Meta-conversations about how to use AI better
□ Patterns in how you started prompts (vague vs. specific, examples given vs. not)
□ Any tasks where you used the AI output directly vs. edited it heavily

Paste your notes as plain text — no structure needed.
```

Wait for the user to paste all three, then proceed to Step 3.

---

### Step 3: Parse and Normalize Inputs

Extract structured data from the three pastes. Build the `inventory` section of the assessment JSON:
- `claude_code_projects`: from Prompt A
- `automated_workflows`: from Prompt A automation + Prompt B automations
- `other_ai_tasks`: from Prompt B
- `new_since_last`: merge both new_since_last arrays, deduplicated

---

### Step 4: Score the Anthropic 4D Framework

For each competency, review all evidence (CC data, other AI tools data, Claude.ai notes, CLAUDE.md governance patterns) and assign a rating. Document specific behavioral evidence — not vague characterizations.

**Rating scale:** Developing → Practitioner → Advanced → Expert → Exceptional

Note the delta from the previous assessment rating: "No change", "Improved: [from] → [to]", or "Declined: [from] → [to]".

See REFERENCE.md for the full behavioral indicator rubric per competency.

**Key pattern to watch — artifact-passivity:**
If the user describes using AI outputs directly without revision or questioning, note this explicitly under Discernment delta. The AI Fluency Index found: polished outputs reduce questioning by ~3-5pp. Counter-evidence (editing, pushing back, asking for reasoning) should be highlighted.

---

### Step 5: Score the Yegge Stage

Review CC and other AI tools data. Assign a stage (can be fractional, e.g., 5.5) and document:
- Evidence supporting the current stage assignment
- Evidence of behaviors pointing toward the next stage
- Delta from last assessment: "No change", "Advanced: Stage X → Stage Y"

See REFERENCE.md for full stage definitions and observable behaviors.

---

### Step 6: Review Development Edges

Pull open development edges from the previous assessment. For each:
- `resolved`: behavioral change is evident in this assessment's data
- `in-progress`: some movement but not consistent yet
- `open`: no change observed

Add 1-2 new development edges identified from patterns in this assessment. Each edge must include:
- A specific target behavior (measurable, not vague)
- Why it would improve fluency

---

### Step 7: Save Assessment JSON

Write the complete assessment to:
`assessments/YYYY-MM-DD.json`

Use the schema defined below.

---

### Step 8: Generate .docx Report

Run:
```bash
python templates/generate_report.py --date YYYY-MM-DD
```

This reads the JSON and produces `AI_Fluency_Assessment_YYYY-MM-DD.docx` in the assessments folder.

If python-docx is not installed: `pip install python-docx`

---

### Step 9: Summary

After the report is generated, output a plain-text summary:
- Yegge stage: current vs. previous, and what specific behavior would push to the next stage
- 4D: any ratings that changed, and why
- New projects or automation since last assessment
- Development edges: which resolved, which are in-progress, new additions
- Report location

---

## --quick Mode

Skip Steps 3–9. Only output the three data-gathering prompts (Step 2), formatted for direct copy-paste into each tool.

---

## --compare Mode

Skip data-gathering (Steps 2–3). Load the two most recent JSON files from the assessments folder and output a diff table:

| Dimension | Previous ([DATE]) | Current ([DATE]) | Change |
|---|---|---|---|
| Yegge Stage | X | Y | +Z |
| Delegation | rating | rating | — |
| Description | rating | rating | — |
| Discernment | rating | rating | — |
| Diligence | rating | rating | — |
| Projects | N | N | +N |
| Automation | N | N | +N |

Then list: new_since_last items, edge status changes, and days between assessments.

---

## Assessment JSON Schema

```json
{
  "date": "YYYY-MM-DD",
  "frameworks": {
    "anthropic_4d": {
      "delegation": { "rating": "string", "evidence": ["string"], "delta": "string" },
      "description": { "rating": "string", "evidence": ["string"], "delta": "string" },
      "discernment": { "rating": "string", "evidence": ["string"], "delta": "string" },
      "diligence": { "rating": "string", "evidence": ["string"], "delta": "string" }
    },
    "yegge": {
      "stage": 5.5,
      "stage_label": "string",
      "evidence_for_current": ["string"],
      "evidence_for_next": ["string"],
      "delta": "string"
    }
  },
  "inventory": {
    "claude_code_projects": [{"name": "string", "status": "string", "autonomy": "string"}],
    "automated_workflows": [{"name": "string", "trigger": "string", "description": "string"}],
    "other_ai_tasks": [{"name": "string", "trigger": "string", "description": "string"}],
    "new_since_last": ["string"]
  },
  "development_edges": [
    {
      "id": 1,
      "title": "string",
      "description": "string",
      "target_behavior": "string",
      "status": "open | in-progress | resolved"
    }
  ],
  "raw_summaries": {
    "claude_code": "string",
    "other_ai_tools": "string",
    "claude_ai_notes": "string"
  }
}
```

## Supporting Files

- **REFERENCE.md**: Full framework definitions — 4D behavioral indicators, Yegge stage descriptions, scoring rubric, artifact-passivity tracking
- **assessments/**: Dated JSON files, one per assessment run (e.g., `2026-03-19.json`)
- **templates/generate_report.py**: Generates .docx report from assessment JSON (navy/gold, Arial)
