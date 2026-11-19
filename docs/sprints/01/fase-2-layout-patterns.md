# Fase 2 — desy-implement-layout-patterns

## Objetivo

Que los patrones estructurales que enseña este skill (FAQ, acciones de formulario, formulario de dirección, avisos) generen un acordeón con la macro oficial, botones secundarios que no envían el formulario, formularios con datos personales por POST y SVG de estado con nombre accesible coherente.

## Bloqueada por

Ninguna (puede empezar ya). Puede ir en paralelo con F1, F3 y F4.

## Lo que hay hoy

Foto del 2026-11-19 sobre `skills/desy-implement-layout-patterns/SKILL.md` (566 líneas). Si el código ha cambiado, manda el código.

- **478-510**: patrón FAQ con `<h3>` dentro de `<summary>`, justificado con «para que screen readers anuncien correctamente» (507), y «normalmente un solo acordeón abierto a la vez» (509). Cita `_pattern.faqs-acordeon.njk` (478), que no existe en desy-html v17.0.2.
- **193, 319**: `<button class="c-button c-button--transparent">Saltar</button>` dentro de `<form>`, sin `type`. La prosa de **207** sí lo pide.
- **263**: `<form action="proximo-paso.html">` sin `method`, con datos de dirección postal.
- **421, 447**: `<svg aria-label="Error">` / `aria-label="Éxito"` sin `role="img"` (hallazgo 15).

Marcado oficial (desy-html v17.0.2): `components/details/_template.details.njk:2-5` emite `<summary>` con `<span>`; `components/accordion/_template.accordion.njk` usa `<button type="button" aria-expanded>` fuera del heading.

## Decisiones cerradas

| Tema | Decisión |
|---|---|
| FAQ | Macro `details` tal cual. Si cada pregunta debe ser heading navegable, `accordion`. Sin heading dentro de `<summary>` (MWG `accordion.md:21`). Varios abiertos a la vez por defecto (`accordion.md:30-53`). |
| Botones secundarios | `type: "button"` explícito en la macro (`type="button"` en HTML). |
| Formulario de dirección | `method="post"` (`forms.md:10,19`). |
| SVG de estado | Si es informativo: `role="img"` + `aria-label`. Si el texto ya lo dice: `aria-hidden="true"`. |

## Costuras de test de esta fase

1. Greps (criterios de aceptación) sobre `skills/desy-implement-layout-patterns/SKILL.md`.
2. Prompts de §1 `desy-implement-layout-patterns`. Además, un prompt propio: «sección de preguntas frecuentes con 3 preguntas, en desy-html».

## Alcance

1. Reescribir el patrón FAQ (ejemplo + reglas) con la macro `details`, y añadir la alternativa `accordion`. Quitar la referencia a `_pattern.faqs-acordeon.njk`.
2. Quitar «un solo abierto a la vez».
3. Añadir `type="button"` a «Saltar» (193, 319).
4. Añadir `method="post"` al formulario de dirección (263).
5. Corregir los SVG de estado (421, 447).

## Fuera de alcance

Hallazgos 23-26 (redundancias y skip-link, severidad baja) y choques C6, C8 y C9 (se mantienen tal cual).

## Tareas

- [ ] Rama `fase-2-layout-patterns` desde `docs/mwg-audit-sprint-01` (worktree propio).
- [ ] Greps y prompts en rojo.
- [ ] Alcance 1-5.
- [ ] Greps en verde y prompts repetidos.
- [ ] `/code-review` atendido.
- [ ] Merge `--no-ff` en `docs/mwg-audit-sprint-01`, push a `fork` y borrar el worktree (sin PR).

## Criterios de aceptación

Con `F=skills/desy-implement-layout-patterns/SKILL.md`:

- `grep -n -A3 '<summary' $F | grep -E '<h[1-6]'` = 0.
- `grep -n 'anuncien correctamente\|un solo acordeón abierto\|faqs-acordeon' $F` = 0.
- `grep -nE '<button( [^>]*)?>' $F | grep -v 'type='` = 0.
- `grep -nE '<form[^>]*>' $F | grep -v 'method='` = 0, o solo formularios sin datos personales (justificado en la PR).
- `grep -nE '<svg[^>]*aria-label' $F | grep -v 'role="img"'` = 0.
- El prompt de FAQ genera `<summary>` sin heading.

## Skills recomendados

- `writing-for-agents`: al reescribir la prosa del SKILL.md.
- `modern-web-guidance`: para recuperar la guía citada y verificar la regla antes de reescribir.
- `desy-validate-accessibility`: su checklist, para revisar los ejemplos tocados.

Cierre: `/implement` para ejecutar, `/code-review` antes de la PR, merge en `docs/mwg-audit-sprint-01` (sin PR por fase; la PR única la abre la fase 5 con `pr`).

## PROMPT

```
Vamos a ejecutar la Fase 2 del Sprint 01 de "desy-skills".

Lee enteros docs/sprints/01/README.md y docs/sprints/01/fase-2-layout-patterns.md, y además:
skills/desy-implement-layout-patterns/SKILL.md (entero), ~/desy-evals-work/r180/node_modules/desy-html/src/templates/components/details/_template.details.njk y .../accordion/_template.accordion.njk.

Las decisiones y las costuras de test están cerradas en esos documentos: no se re-preguntan.
Orden: FAQ → botones → form → SVG.
Para los prompts de verificación, abre una sesión nueva e indica la ruta del SKILL.md del worktree (los enlaces de ~/.claude/skills apuntan al checkout principal).

Crea el worktree y la rama fase-2-layout-patterns desde docs/mwg-audit-sprint-01 y lanza:
/implement docs/sprints/01/fase-2-layout-patterns.md
Las costuras de la fase son los greps y los prompts: ejecútalos antes (rojo) y después (verde). Para /code-review, el punto fijo es docs/mwg-audit-sprint-01 y la spec es
docs/sprints/01/fase-2-layout-patterns.md. Atiende sus hallazgos y después mergea con --no-ff en docs/mwg-audit-sprint-01 y haz push a fork. No abras PR: la única PR la abre la fase 5.
```
