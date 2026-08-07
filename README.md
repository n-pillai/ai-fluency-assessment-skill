# AI Fluency Assessment Skill

A Claude Code skill for quarterly assessment of AI fluency across two frameworks:

- **Anthropic's 4D AI Fluency Framework** — Delegation, Description, Discernment, Diligence
- **Yegge's 8-Stage Developer-Agent Evolution Model** — Stage 1 (zero AI usage) through Stage 8 (own orchestrator)

Tracks progress over time with structured JSON history and generates dated `.docx` reports.

## Installation

Copy the full skill directory into your Claude Code skills folder — the skill
needs its companion files (`REFERENCE.md`, `templates/`, `assessments/`), so
copying `SKILL.md` alone will not work:

```bash
# macOS/Linux
cp -r . ~/.claude/skills/ai-fluency-assessment/

# Windows
xcopy /E /I . %USERPROFILE%\.claude\skills\ai-fluency-assessment\
```

Install the Python dependency for `.docx` report generation:

```bash
pip install python-docx
```

## Usage

Ask Claude Code to run your AI fluency assessment (e.g. "assess my AI fluency"
or "run a fluency check") — the skill is picked up by description match. The
flags below can be given in the same request (e.g. "run a quick fluency
assessment").

| Command | What it does |
|---|---|
| `/assess-fluency` | Full assessment: gather data, score frameworks, diff against last, generate report |
| `/assess-fluency --quick` | Output data-gathering prompts only — no scoring or report |
| `/assess-fluency --compare` | Diff the two most recent assessments without new data |

## What the Assessment Covers

The skill guides you through three data-gathering prompts (run in separate sessions):

- **Prompt A** — Claude Code self-assessment: projects, autonomy levels, automations, Yegge stage estimate
- **Prompt B** — Other AI tools: automations, integrations, and AI-generated artifacts (ChatGPT, Copilot, Gemini, n8n, Zapier, etc.)
- **Checklist C** — Claude.ai manual notes: delegation patterns, error-catching, format specifications

Then scores each 4D competency (Developing → Practitioner → Advanced → Expert → Exceptional), assigns a Yegge stage, reviews development edges from previous assessments, and generates a `.docx` report.

## File Structure

```
ai-fluency-assessment/
├── SKILL.md                        # Main skill definition (copy to ~/.claude/commands/)
├── REFERENCE.md                    # Framework definitions, rubrics, stage descriptions
├── README.md                       # This file
├── assessments/                    # Your assessment history (gitignored — stays local)
│   └── YYYY-MM-DD.json             # One file per assessment run
├── templates/
│   └── generate_report.py          # Generates .docx report from assessment JSON
└── example/
    └── example-assessment.json     # Anonymized example showing the JSON schema
```

## Assessment History

Each assessment is saved as `assessments/YYYY-MM-DD.json`. This folder is gitignored in the public repo — your assessment data stays local. The `--compare` mode reads the two most recent files in this folder to produce a progress diff.

See `example/example-assessment.json` for the full schema with sample data.

## Frameworks

### Anthropic 4D AI Fluency Framework

| Dimension | What it measures |
|---|---|
| **Delegation** | What you hand off to AI vs. retain; task decomposition and scoping |
| **Description** | Quality of prompts; specificity, constraints, examples, format direction |
| **Discernment** | Critical evaluation of outputs; error-catching, pushing back, not using blindly |
| **Diligence** | Verification habits; checking facts, testing outputs, reviewing for bias |

Full behavioral indicator rubric: see `REFERENCE.md`.

### Yegge's 8-Stage Developer-Agent Evolution Model

| Stage | Label |
|---|---|
| 1 | Zero AI usage |
| 2 | Occasional AI queries |
| 3 | Regular copilot usage |
| 4 | Context-aware prompting |
| 5 | Agentic workflows |
| 6 | Multi-agent coordination |
| 7 | AI-augmented systems design |
| 8 | Own orchestrator |

Full stage definitions with observable behaviors: see `REFERENCE.md`.

## License

MIT
