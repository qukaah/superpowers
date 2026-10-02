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
- **Skills eliminadas:** brainstorming, writing-plans, executing-plans, subagent-driven-development, using-git-worktrees, finishing-a-development-branch, requesting/receiving-code-review, dispatching-parallel-agents, diagnosing-superpowers y using-superpowers. Asumen un flujo de software que compite con las fases del framework.
- **`writing-skills/helialab-adaptation.md`:** rutas, descripciones en español, política de modelos para los subagentes de prueba y reglas duras del framework.

## Instalación (a nivel proyecto, en el repo del framework)
```
/plugin marketplace add C:\Users\ffeli\Desktop\Empresa\Helialab\forks\superpowers
/plugin install helialab-superpowers@helialab-superpowers
```
El marketplace local lee la rama que tenga checkout el clon, que debe ser `helialab`.

## Sincronizar con el upstream
```
git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream && git checkout main && git merge upstream/main
git checkout helialab && git rebase main
```
