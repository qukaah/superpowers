# Helialab adaptation of writing-skills

How this method applies to the Helialab hardware/firmware framework skills (Spanish, document-driven, gate-based).

## What differs from upstream

| Upstream assumption | Helialab |
|---|---|
| Skills in `~/.claude/skills/` | Project skills in `<framework>/.claude/skills/` |
| Descriptions start with "Use when..." | Spanish: "Úsala cuando..." plus Spanish trigger phrases the team actually says. Third person, triggers only, no workflow summary |
| Deploy = commit and push | Diff and test results are shown to a human; commit only after approval |
| Tests run with the main model | Test subagents use the cheapest model that can reveal the failure (see below) |
| Scenarios written ad hoc | Scenario library at `<framework>/framework/skill-tests/escenarios/`, one file per hard rule |
| Evidence of real failures: your memory | Proposals from autoharness (`.claude/autoharness/proposals/`) with session evidence |

## Model policy for test subagents

| Task | Model | Why |
|---|---|---|
| Triggering test (does this prompt load the right skill?) | haiku | Classification; cheap, many samples |
| Pressure scenario, RED and GREEN runs (≥ 5 per variant) | haiku **and** sonnet | Framework skills also run inside cheaper subagents; a rule must hold for the weakest model that executes it |
| Grading a transcript against a rubric (A/B/C chosen, rule cited, rationalization quoted) | haiku | Mechanical, rubric-driven |
| Meta-test ("how should the skill be written so that A is the only choice?") | sonnet | Needs reasoning about the text |
| Writing the skill change (GREEN/REFACTOR) | main session model | Authoring quality matters; one call |
| Final confirmation of ⛔ rules after REFACTOR | main session model, 2 runs | Confirms the production model still complies |

A failure that appears only on haiku is still a failure if the skill can be executed by a haiku subagent; otherwise record it as "haiku-only" and decide explicitly.

## Hard rules of the framework (discipline skills)

These are the rules that get pressure-tested. Each one has a scenario file in the framework repo.

1. Never approve a gate or emit a deliverable without explicit human approval.
2. ⛔ items cannot be accepted as risk.
3. LCSC C-numbers only from direct lookup, never from memory or free-text search.
4. "Tested/validated/closed" only with evidence (TST result, log, approved gate, emitted document).
5. Unknown data becomes a TBD with owner; never invented.
6. Agents never read layer 3 (`entregables/out/`).
7. Scope changes go through a CR; approved REQs are not edited directly.
8. KiCad writes: lock check, `_PRE_` backup, authoritative verification.
