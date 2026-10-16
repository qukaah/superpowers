# Fork Helialab de Superpowers (rama `helialab`)

`main` queda igual al upstream (obra/superpowers), así se puede sincronizar. Los cambios viven en la rama `helialab`.

## Cambios respecto al upstream
- **Plugin renombrado** a `helialab-superpowers` (marketplace `helialab-superpowers`), para no chocar con el oficial.
- **Sin hook de SessionStart** (`hooks/hooks.json` vacío): no se inyecta `using-superpowers` ni se obliga a usar skills en cada sesión.
- **Skills conservadas:**
  - `writing-skills`: TDD para skills; el núcleo para mejorar las skills del framework.
  - `test-driven-development`: base de writing-skills; también se usa para el código de firmware.
  - `verification-before-completion`: coincide con la regla "estado solo con evidencia".
  - `systematic-debugging`: bring-up y depuración de firmware.
- **Skills de software, en un segundo plugin `helialab-superpowers-software`** (`plugins/helialab-superpowers-software/`, mismo marketplace): brainstorming, writing-plans, executing-plans, subagent-driven-development, using-git-worktrees, finishing-a-development-branch, requesting-code-review, receiving-code-review y dispatching-parallel-agents. Se sacaron del plugin principal porque compiten con las fases y gates de `helialab-framework`, pero sirven para proyectos de software sin ese framework (apps web, laboratorio, herramientas). Se restauraron desde `main` sin cambios salvo las referencias: `superpowers:<núcleo>` → `helialab-superpowers:<núcleo>` y `superpowers:<software>` → `helialab-superpowers-software:<software>`; la mención a `using-superpowers/references` en executing-plans apunta al upstream.
- **Siguen fuera:** using-superpowers (lo inyectaba el hook de SessionStart y obliga a usar skills en cada sesión) y diagnosing-superpowers (diagnóstico del propio plugin upstream).

## Qué plugin activar en cada proyecto
| Proyecto | Plugins |
|---|---|
| Hardware/firmware con `helialab-framework` (HEL01, PCB Design Firmware) | `helialab-superpowers` solamente |
| Software sin framework (apps web, laboratorio, herramientas) | `helialab-superpowers` + `helialab-superpowers-software` |

`helialab-superpowers-software` no funciona solo: sus skills llaman a TDD, debugging y verificación del núcleo. La skill global `seleccionar-skills` (Skills Vault, LAB-011) aplica estas reglas.
- **`writing-skills/helialab-adaptation.md`** (archivo propio, no existe en el upstream): dónde viven las skills Helialab (framework en `PCB Design Firmware/skills/`, globales en `~/.claude/skills/`, carpeta Helialab en `Helialab/.claude/skills/`), descripciones en español, ≥ 3 evals por skill con 1 negativo (`evals/evals.json` de skill-creator), disparo medido con `disparo.mjs` de LAB-011, política de modelos solo citada (fuente única: `PCB Design Firmware/framework/skill-tests/README.md`) y reglas duras del framework.

### Cambios al texto del upstream (revisar en cada rebase)
Todos se marcan en el texto con "(Helialab)" o "Helialab:" cuando es posible.
- **Descripciones (13 skills, `description:` del frontmatter):** se mantiene el inglés original y se agrega al final una frase "En español: …" con los disparadores del equipo. Afecta a systematic-debugging, test-driven-development, verification-before-completion, writing-skills, brainstorming, dispatching-parallel-agents, executing-plans, finishing-a-development-branch, receiving-code-review, requesting-code-review, subagent-driven-development, using-git-worktrees y writing-plans. En el rebase, si el upstream cambia la descripción, tomar la nueva y volver a pegar la frase en español.
- **brainstorming, descripción acotada:** "before any creative work" → "before any creative software work in a code repository", más "Not for Helialab Laboratorio experiments (experimento-nuevo), visual themes (tema-nuevo) or hardware projects (helialab-framework)."
- **Ruta de specs/planes:** una línea "(Helialab) Si el CLAUDE.md del proyecto define otra carpeta para specs/planes … usa esa; si no, la ruta por defecto" en brainstorming (After the Design → Documentation), writing-plans (Save plans to), executing-plans (Setup), subagent-driven-development (Setup) y requesting-code-review (después de los placeholders).
- **writing-skills/SKILL.md partido (681 → 463 líneas):** se movieron sin cambiar el texto a archivos nuevos del mismo directorio, dejando en SKILL.md un resumen de 2–3 líneas + enlace:
  - `authoring-conventions.md` ← Flowchart Usage, Code Examples, File Organization, Anti-Patterns.
  - `testing-skill-types.md` ← Testing All Skill Types, Micro-Test Wording Before Full Scenarios.
  - `bulletproofing.md` ← Bulletproofing Skills Against Rationalization (subtítulos subidos de `###` a `##`; las referencias "below" apuntan a SKILL.md).
  - En "Match the Form to the Failure", "(see Bulletproofing below)" → enlace a `bulletproofing.md`.
  - En el rebase: si el upstream edita alguna de esas secciones, aplicar el cambio en el archivo de referencia correspondiente.
- **Índice "## Contents"** (insertado tras el título) en las referencias de > 100 líneas: writing-skills (`anthropic-best-practices.md`, `testing-skills-with-subagents.md`, `persuasion-principles.md`, `examples/CLAUDE_MD_TESTING.md`, `authoring-conventions.md`), systematic-debugging (`condition-based-waiting.md`, `defense-in-depth.md`, `root-cause-tracing.md`, `CREATION-LOG.md`) y test-driven-development (`writing-good-tests.md`). Se regenera desde los `##` si el upstream cambia los títulos.

## Instalación (a nivel proyecto, en el repo del framework)
```
/plugin marketplace add C:\Users\ffeli\Desktop\Empresa\Helialab\forks\superpowers
/plugin install helialab-superpowers@helialab-superpowers
/plugin install helialab-superpowers-software@helialab-superpowers   # solo proyectos de software
```
El marketplace local lee la rama que tenga checkout el clon, que debe ser `helialab`.

## Sincronizar con el upstream
```
git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream && git checkout main && git merge upstream/main
git checkout helialab && git rebase main
```
