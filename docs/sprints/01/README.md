# Sprint 01 — Correcciones de contenido frente a modern-web-guidance

## Visión

La auditoría `docs/modern-web-guidance-gap.md` comparó los 12 skills DESY con modern-web-guidance (MWG). Después se verificó contra desy-html v17.0.2 (ver la sección «Correcciones» al final de la auditoría) y las decisiones se cerraron en un `/grill-with-docs`. Este sprint corrige los **huecos** de contenido: el marcado y las instrucciones que los skills añaden por su cuenta y que contradicen MWG. Los **choques** con macros oficiales se mantienen, pero se declaran.

Salió de `/create-feature` el 2026-11-19, a partir del grill de la misma sesión.

## Prerrequisito

- La rama de integración `docs/mwg-audit-sprint-01` (en `fork`), creada desde `main` `6908241`, con `GLOSSARY.md`, `docs/modern-web-guidance-gap.md`, `docs/propuestas-sda.md` y este sprint.
- Una copia local de desy-html v17.0.2: `~/desy-evals-work/r180/node_modules/desy-html`.

## Decisiones cerradas

| Tema | Decisión | Descartado y por qué |
|---|---|---|
| Modales | Se mantiene el checklist `role="dialog"`, `aria-modal`, Esc y clic fuera, y se declara con la **línea de choque de modales** (abajo). En confirmaciones destructivas, foco inicial en Cancelar con `openDialog(id, focusAfterClosed, focusFirst)` (`desy-html/src/js/aria/dialog.js:119-124`). | `<dialog>` nativo y `closedby`: ningún componente DESY usa `<dialog>` (hallazgos 1-2 descartados). La migración se propone a SDA. |
| FAQ | Macro `details` tal cual (emite `<span>` en `<summary>`), o `accordion` si cada pregunta necesita heading. Se quita «normalmente un solo abierto». | `<h3>` dentro de `<summary>`: lo inventó el skill, no hay patrón oficial. |
| Formularios | `type: "button"` explícito en los botones secundarios (la macro Nunjucks no pone `type`; en Angular el valor por defecto es `submit`). Sin submit deshabilitado. `method="post"` cuando hay datos personales. | — |
| Errores | Patrón de servidor: macro `errorMessage` + resumen de errores + foco al primer campo con error. La macro `input` ya emite `aria-invalid`/`aria-errormessage` solo con error (choque declarado). Para la validación en el cliente se remite a MWG `accessible-error-announcement` y `validate-input-after-interaction`. | Copiar el patrón `:user-invalid` al skill (D1: remitir, no duplicar). |
| Alcance | Además: hallazgos 13, 14, 15, 17, 18, 19, 28, 29 y 31, y greps nuevos en el checklist offline. | Forma de los skills (sprint 02), tablas (16), imágenes/CWV (21) y severidad baja. |

### Texto común (F1 y F3 lo copian literalmente)

**Línea de choque de modales:**

> Choque con MWG `dialog`: MWG recomienda `<dialog>` nativo sin `role`/`aria-modal`. Los componentes `modal` y `dialog` de DESY son `<div>` gestionados por `dialog.js`, que exige `role="dialog"`; por eso aquí sí se pide. Propuesta SDA: `docs/propuestas-sda.md` §1.

**Patrón de errores (resumen):**

> Con error de servidor: la macro emite `aria-invalid="true"` y `aria-errormessage`; el mensaje va con la macro `errorMessage` (prefijo `<span class="sr-only">Error:</span>`); arriba del formulario, un resumen de errores con enlaces a cada campo; al recargar, foco en el resumen o en el primer campo con error. Validación en el cliente: consulta MWG `accessible-error-announcement` y `validate-input-after-interaction` (`aria-invalid="false"` hasta `:user-invalid`). No se fija `aria-invalid="true"` a mano en el marcado.

## Glosario y ADRs

Términos ya presentes en `GLOSSARY.md`: **choque**, **hueco**, **macro oficial**, **remitir a MWG**, **propuesta SDA**. Este sprint no crea ADRs.

## Principios técnicos

- DESY manda en tokens, Tailwind y el marcado de las macros oficiales. Lo que el skill escribe a mano sigue a MWG.
- Cada choque que se mantenga se declara en el skill con la línea `Choque con MWG \`<id>\`: …`.
- Se cambia el ejemplo **y** la prosa que lo justifica. Un ejemplo corregido con una justificación antigua al lado vuelve a generar el error.
- No se reestructura ningún SKILL.md: partir ficheros, cambiar el frontmatter o añadir el paso disclose es trabajo del sprint 02.

## Costuras de test

No hay suite de tests. Cada fase usa dos costuras, antes y después del cambio:

1. **Greps sobre el SKILL.md** (los comandos están en cada fase). Detectan el marcado prohibido en los ejemplos y la prosa. No detectan si el agente reconstruye el error a partir de otro texto.
2. **Prompts del skill** en `docs/skill-prompts-collection.md` (§1, dos por skill), lanzados en una **sesión nueva** con la instrucción «lee el skill desde `<worktree>/skills/<skill>/SKILL.md`». Ojo: `~/.claude/skills/desy-*` son enlaces simbólicos al checkout principal, no al worktree. Detectan lo que el agente genera de verdad. No son deterministas: se comparan las salidas con los mismos greps.

## Fases

| # | Fase | Documento | Bloqueada por |
|---|---|---|---|
| 1 | desy-implement-component | [fase-1-implement-component.md](fase-1-implement-component.md) | — |
| 2 | desy-implement-layout-patterns | [fase-2-layout-patterns.md](fase-2-layout-patterns.md) | — |
| 3 | desy-validate-accessibility | [fase-3-validate-accessibility.md](fase-3-validate-accessibility.md) | — |
| 4 | angular-translator + component-recognizer + scaffold-project | [fase-4-translator-recognizer-scaffold.md](fase-4-translator-recognizer-scaffold.md) | — |
| 5 | Cierre | [fase-5-cierre.md](fase-5-cierre.md) | 1, 2, 3, 4 |

**Frontera inicial:** F1, F2, F3 y F4 no comparten ficheros y pueden ir en paralelo, cada una en su worktree (`git worktree add -b fase-N-<slug> ../desy-skills-fase-N docs/mwg-audit-sprint-01`). F5 va al final.

## Ramas

Todo el sprint va en **una única PR contra `main`**, desde la rama de integración `docs/mwg-audit-sprint-01` (fork `jorgeatgu/desy-skills`), que ya contiene la auditoría y este sprint.

- **F1-F4:** cada una en su worktree y rama, creada desde la rama de integración: `git worktree add -b fase-N-<slug> ../desy-skills-fase-N docs/mwg-audit-sprint-01`. Al terminar, `/code-review` con la rama de integración como punto fijo. Después, en el checkout principal: `git switch docs/mwg-audit-sprint-01 && git merge --no-ff fase-N-<slug> && git push fork`, y `git worktree remove ../desy-skills-fase-N`. No hay PR por fase.
- **Conflictos:** F1-F4 no comparten ficheros, así que el orden de merge da igual.
- **F5:** sin worktree, en el checkout principal sobre la rama de integración (los enlaces de `~/.claude/skills` apuntan a él). Al terminar, abre la PR única `jorgeatgu:docs/mwg-audit-sprint-01` → `minilama2222/desy-skills:main`.

## Riesgos

| Riesgo | Qué se hace |
|---|---|
| F1 y F3 redactan modales o errores de forma distinta | Copiar literalmente el «Texto común» de este README. F5 lo comprueba con grep. |
| Las líneas citadas se han movido | Las fases son una foto del 2026-11-19: manda el código. Buscar por el texto citado, no por el número de línea. |
| Los prompts cargan el SKILL.md del checkout principal y no el del worktree | Indicar la ruta del worktree en el prompt. Si el agente carga el skill por nombre, repetir el prompt. |
| Un hallazgo no se sostiene al releerlo | Se descarta y se apunta en el commit de merge y en la retro. No se fuerza el cambio. |

## Definición de hecho del sprint

- Las 5 fases mergeadas en `docs/mwg-audit-sprint-01`, cada una con `/code-review` atendido, y una única PR de `docs/mwg-audit-sprint-01` contra `main`.
- Los greps de todas las fases dan 0 coincidencias prohibidas sobre `skills/`.
- La línea de choque de modales aparece igual en implement-component y validate-accessibility.
- Prompts combinados 2.1 y 2.2 lanzados tras el sprint sin regresiones.
- La tabla de `docs/modern-web-guidance-gap.md` marca como resueltas las filas corregidas.
- `/retro` lanzado y `retro.md` escrita.

## Después de este sprint

Sprint 02, «Forma de los skills y relación con MWG»: anti-triggers y `version` en el frontmatter, política Baseline en `skills/SKILL.md`, remitir a MWG, partir los SKILL.md de más de 500 líneas, paso disclose y carga parcial de `catalog.json`. El prompt ya está preparado en la conversación del grill.
