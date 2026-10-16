# Fork Helialab de Superpowers (rama `helialab`)

`main` queda igual al upstream (obra/superpowers), así se puede sincronizar. Los cambios viven en la rama `helialab`.

**Base actual:** upstream v7.0.0 (`bb92a77`), sincronizada el 2026-10-16. Versión de ambos plugins: `7.0.0-helialab.1` (convención `<versión upstream>-helialab.<n>`; `n` sube con cada cambio propio sobre la misma base y vuelve a 1 al cambiar de base).

## Cambios respecto al upstream
- **Plugin renombrado** a `helialab-superpowers` (marketplace `helialab-superpowers`), para no chocar con el oficial.
- **Sin hook de SessionStart** (`hooks/hooks.json` vacío): no se inyecta `using-superpowers` ni se obliga a usar skills en cada sesión.
- **Skills conservadas:**
  - `writing-skills`: TDD para skills; el núcleo para mejorar las skills del framework.
  - `test-driven-development`: base de writing-skills; también se usa para el código de firmware.
  - `verification-before-completion`: coincide con la regla "estado solo con evidencia".
  - `systematic-debugging`: bring-up y depuración de firmware.
- **Skills de software, en un segundo plugin `helialab-superpowers-software`** (`plugins/helialab-superpowers-software/`, mismo marketplace): brainstorming, writing-plans, executing-plans, subagent-driven-development, using-git-worktrees, finishing-a-development-branch, requesting-code-review, receiving-code-review y dispatching-parallel-agents. Se sacaron del plugin principal porque compiten con las fases y gates de `helialab-framework`, pero sirven para proyectos de software sin ese framework (apps web, laboratorio, herramientas). Son copias de `skills/<skill>/` de `main` (resincronizadas con v7.0.0 el 2026-10-16) sin cambios salvo las referencias: `superpowers:<núcleo>` → `helialab-superpowers:<núcleo>` y `superpowers:<software>` → `helialab-superpowers-software:<software>`; la mención a `using-superpowers/references` en executing-plans apunta al upstream.
- **Siguen fuera:** using-superpowers (lo inyectaba el hook de SessionStart y obliga a usar skills en cada sesión) y diagnosing-superpowers (diagnóstico del propio plugin upstream).

## Qué plugin activar en cada proyecto
| Proyecto | Plugins |
|---|---|
| Hardware/firmware con `helialab-framework` (HEL01, PCB Design Firmware) | `helialab-superpowers` solamente |
| Software sin framework (apps web, laboratorio, herramientas) | `helialab-superpowers` + `helialab-superpowers-software` |

`helialab-superpowers-software` no funciona solo: sus skills llaman a TDD, debugging y verificación del núcleo. La skill global `seleccionar-skills` (Skills Vault, LAB-011) aplica estas reglas.
- **`writing-skills/helialab-adaptation.md`** (archivo propio, no existe en el upstream): dónde viven las skills Helialab (framework en `PCB Design Firmware/skills/`, globales en `~/.claude/skills/`, carpeta Helialab en `Helialab/.claude/skills/`), descripciones en español, ≥ 3 evals por skill con 1 negativo (`evals/evals.json` de skill-creator), disparo medido con `disparo.mjs` de LAB-011, política de modelos solo citada (fuente única: `PCB Design Firmware/framework/skill-tests/README.md`) y reglas duras del framework.

### Cambios al texto del upstream (revisar en cada sincronización)
Todos se marcan en el texto con "(Helialab)" o "Helialab:" cuando es posible.
- **Descripciones (13 skills, `description:` del frontmatter):** se mantiene el inglés original y se agrega al final una frase "En español: …" con los disparadores del equipo. Afecta a systematic-debugging, test-driven-development, verification-before-completion, writing-skills, brainstorming, dispatching-parallel-agents, executing-plans, finishing-a-development-branch, receiving-code-review, requesting-code-review, subagent-driven-development, using-git-worktrees y writing-plans. Al sincronizar, si el upstream cambia la descripción, tomar la nueva y volver a pegar la frase en español.
- **brainstorming, descripción acotada:** "before any creative work … in software or out of it (a talk, a business, a renovation)" (texto de v7) → "before any creative software work in a code repository … planning anything new in software", más "Not for Helialab Laboratorio experiments (experimento-nuevo), visual themes (tema-nuevo) or hardware projects (helialab-framework)."
- **Ruta de specs/planes:** una línea "(Helialab) Si el CLAUDE.md del proyecto define otra carpeta para specs/planes … usa esa; si no, la ruta por defecto" en brainstorming (The Written Design → Where it goes), writing-plans (Save plans to), executing-plans (Setup), subagent-driven-development (Setup) y requesting-code-review (después de los placeholders).
- **writing-skills/SKILL.md partido (681 → 463 líneas):** se movieron sin cambiar el texto a archivos nuevos del mismo directorio, dejando en SKILL.md un resumen de 2–3 líneas + enlace:
  - `authoring-conventions.md` ← Flowchart Usage, Code Examples, File Organization, Anti-Patterns.
  - `testing-skill-types.md` ← Testing All Skill Types, Micro-Test Wording Before Full Scenarios.
  - `bulletproofing.md` ← Bulletproofing Skills Against Rationalization (subtítulos subidos de `###` a `##`; las referencias "below" apuntan a SKILL.md).
  - En "Match the Form to the Failure", "(see Bulletproofing below)" → enlace a `bulletproofing.md`.
  - Al sincronizar: si el upstream edita alguna de esas secciones, aplicar el cambio en el archivo de referencia correspondiente.
- **Índice "## Contents"** (insertado tras el título) en las referencias de > 100 líneas: writing-skills (`anthropic-best-practices.md`, `testing-skills-with-subagents.md`, `persuasion-principles.md`, `examples/CLAUDE_MD_TESTING.md`, `authoring-conventions.md`), systematic-debugging (`condition-based-waiting.md`, `defense-in-depth.md`, `root-cause-tracing.md`, `CREATION-LOG.md`) y test-driven-development (`writing-good-tests.md`). Se regenera desde los `##` si el upstream cambia los títulos.

## Instalación (a nivel proyecto, en el repo del framework)
```
/plugin marketplace add C:\Users\ffeli\Desktop\Empresa\Helialab\forks\superpowers
/plugin install helialab-superpowers@helialab-superpowers
/plugin install helialab-superpowers-software@helialab-superpowers   # solo proyectos de software
```
El marketplace local lee la rama que tenga checkout el clon, que debe ser `helialab`.

## Sincronización 2026-10-16: v6.4.2 → v7.0.0
- **Merge** de `main` (v7.0.0) en `helialab`, sin rebase. Conflictos: versiones/nombres en `.claude-plugin/plugin.json` y `marketplace.json` (se mantienen los nombres Helialab, versión a `7.0.0-helialab.1`) y `brainstorming/SKILL.md` (se tomó el texto nuevo y se reaplicaron los cambios Helialab).
- **brainstorming reconstruido upstream:** SKILL.md nuevo (conversación de una pregunta por mensaje, "Size the Work", "Builder check"). Se agrega `builder-check-prompt.md` y se elimina `spec-document-reviewer-prompt.md`, igual que upstream. `visual-companion.md` y `scripts/` sin cambios propios (`server.cjs` trae el arreglo de v7).
- **Otros cambios upstream que entraron:** `systematic-debugging/SKILL.md` (comandos de ejemplo portables), `requesting-code-review/SKILL.md` (`cut` en vez de `awk`), `executing-plans/scripts/task-done` y `subagent-driven-development/scripts/sdd-workspace`. Las copias del plugin de software quedan idénticas al upstream salvo descripciones, rutas de specs y referencias.
- **Skills nuevas en v7:** ninguna. Siguen fuera using-superpowers y diagnosing-superpowers.
- **Arreglos de hooks para Windows (`hooks/run-hook.cmd`, `hooks/session-start`):** entran tal cual, pero no aplican: `hooks/hooks.json` sigue vacío y el plugin no registra hooks.
- **Manifiesto nuevo de Antigravity (`.antigravity-plugin/`):** queda como upstream (nombre `superpowers`); no lo usa Claude Code y no choca con `helialab-superpowers`. Los manifiestos de otros harness (Codex, Cursor, Gemini, etc.) tampoco se tocan.
- **Permisos:** los scripts del plugin de software recuperan el bit ejecutable (100755) que tienen en el upstream.

## Sincronizar con el upstream
```
git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream && git checkout main && git merge upstream/main
git push origin main
git checkout helialab && git merge main   # merge, no rebase: la rama ya está publicada
```
Después del merge: copiar a `plugins/helialab-superpowers-software/skills/` los cambios del upstream en esas 9 skills (incluidos archivos nuevos o borrados), reaplicar los renombres de referencias y los cambios de la lista de arriba, revisar si hay skills nuevas, subir la versión en `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` y `plugins/helialab-superpowers-software/.claude-plugin/plugin.json`, y verificar que cada `superpowers:` apunte a una skill existente (`grep -rn "superpowers:" skills plugins`).
