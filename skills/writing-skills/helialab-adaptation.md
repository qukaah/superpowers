# Helialab adaptation of writing-skills

How this method applies to Helialab skills (Spanish, document-driven; the hardware/firmware framework is gate-based).

## Where Helialab skills live

| Skills | Path | Notes |
|---|---|---|
| Framework (hardware/firmware): proyecto-nuevo, fase-siguiente, fw-fase, gate-check, trazabilidad, auditoria, seguimiento, cambio, entregables, mejorar-skill | `Helialab/PCB Design Firmware/skills/<skill>/` | Distributed by the `helialab-framework` plugin. Scenarios and results in `PCB Design Firmware/framework/skill-tests/` |
| Global (all sessions): experimento-nuevo, reutilizar-experimento, seleccionar-skills, helialab-marca, firmware-review | `~/.claude/skills/<skill>/` | Personal skills; load in every project, so their descriptions compete with everything else |
| Laboratorio/company folder: tema-nuevo, publicar-laboratorio, publicar-landing | `Helialab/.claude/skills/<skill>/` | Project skills of the Helialab folder |
| Forks (helialab-superpowers, helialab-superpowers-software, helialab-autoharness) | `Helialab/forks/<fork>/` | Edit only on branch `helialab`; list every change to upstream text in the fork's `HELIALAB.md` |

## What differs from upstream

| Upstream assumption | Helialab |
|---|---|
| Skills in `~/.claude/skills/` | See the table above: framework, global and Helialab-folder skills live in different places |
| Descriptions start with "Use when..." | Spanish: "Úsala cuando..." plus Spanish trigger phrases the team actually says. Third person, triggers only, no workflow summary. Upstream (English) skills keep their text and add one Spanish sentence of triggers at the end |
| Deploy = commit and push | Diff and test results are shown to a human; commit only after approval |
| Tests run with the main model | Test subagents follow the framework's model policy (see below) |
| Scenarios written ad hoc | Scenario library at `PCB Design Firmware/framework/skill-tests/escenarios/`, one file per hard rule |
| Evidence of real failures: your memory | Proposals from autoharness (`.claude/autoharness/proposals/`) with session evidence |

## Evals required for every skill

- **≥ 3 evals per skill, at least 1 negative** (a prompt that sounds close but must NOT load the skill, e.g. "nuevo experimento" must not load brainstorming).
- Format: `evals/evals.json` inside the skill folder, as defined by skill-creator (`skill_name`, `evals[]` with `id`, `prompt`, `expected_output`, optional `files`, `expectations`).
- Write the evals **before** rewriting a description, and run them before and after.
- **Triggering is measured with the full environment**, not the skill alone: the test in `Laboratorio/experimentos/LAB-011-skills-vault-helialab/auditoria/disparo/` (`node disparo.mjs preparar <indice.json> <salida.md> [semilla]`, then `node disparo.mjs puntuar <respuestas.json>…`). It lists every installed skill with its description, so it catches collisions between skills (e.g. brainstorming vs experimento-nuevo vs tema-nuevo).

## Model policy for test subagents

Single source of truth: `PCB Design Firmware/framework/skill-tests/README.md`, section "Política de modelos". Do not copy the table here; follow that file.

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
