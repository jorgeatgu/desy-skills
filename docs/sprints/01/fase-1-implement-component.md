# Fase 1 — desy-implement-component

## Objetivo

Que un agente que implementa un componente con este skill genere modales con el choque declarado y el foco inicial en Cancelar, formularios sin submit deshabilitado, errores con el patrón de servidor, avisos de «ventana nueva» sin `title` y botones solo-icono con el nombre en el botón.

## Bloqueada por

Ninguna (puede empezar ya). Puede ir en paralelo con F2, F3 y F4.

## Lo que hay hoy

Foto del 2026-11-19 sobre `skills/desy-implement-component/SKILL.md` (684 líneas). Si el código ha cambiado, manda el código.

- **483-488**: checklist de modales (`role=dialog`, `aria-modal`, foco atrapado, Esc, clic fuera) sin declarar el choque.
- **488** y **659-660**: confirmación destructiva con «foco en el título».
- **521**: `[disabled]="formulario.invalid"` en el submit (hallazgo 8).
- **470**: checklist de errores con `aria-invalid` fijo (hallazgo 10).
- **461**: `title` como aviso de «abre en ventana nueva» (hallazgo 13).
- **460, 492, 496**: botón solo-icono con `role=img` + `aria-label` en el SVG (hallazgo 14).
- **636-651**: ejemplo Ionic que marca «✅ Esc cierra» sin demostrarlo (hallazgo 4, baja, fuera de alcance).

## Decisiones cerradas

| Tema | Decisión |
|---|---|
| Modales | Checklist intacto + línea de choque de modales del README. Foco inicial en Cancelar: `openDialog(id, focusAfterClosed, 'id-boton-cancelar')`. |
| Submit | Nunca deshabilitado; se deja enviar y se muestran los errores (MWG `forms`, `forms.md:137`). |
| Errores | Patrón de errores del README, literal. |
| Ventana nueva | Texto visible o `<span class="sr-only">(se abre en ventana nueva)</span>` dentro del enlace; sin `title` (`accessibility.md:181`). |
| Botón solo-icono | `aria-label` en el `<button>`, SVG con `aria-hidden="true"`. `role="img"` solo para SVG informativo suelto (`icons.md:171,210`). |

## Costuras de test de esta fase

1. Greps (criterios de aceptación) sobre `skills/desy-implement-component/SKILL.md`.
2. Prompts de `docs/skill-prompts-collection.md` §1 `desy-implement-component`. Además, un prompt propio: «modal de confirmación para borrar un expediente, en desy-html».

## Alcance

1. Añadir la línea de choque de modales junto al checklist (483-488).
2. Cambiar «foco en el título» por foco inicial en Cancelar con `focusFirst`, en el checklist y en el ejemplo (488, 659-660).
3. Quitar `[disabled]="formulario.invalid"` (521) y añadir la regla «no deshabilitar el submit».
4. Sustituir la línea de errores (470) por el patrón de errores del README.
5. Reescribir la regla de ventana nueva (461).
6. Reescribir la regla de botón solo-icono (460, 492, 496).

## Fuera de alcance

Hallazgos 4, 12 y 27 (Ionic, inputmode y `aria-label` del form); reestructurar el SKILL.md; el frontmatter.

## Tareas

- [ ] Rama `fase-1-implement-component` desde `docs/mwg-audit-sprint-01` (worktree propio).
- [ ] Greps y prompts en rojo, guardando la salida de antes.
- [ ] Alcance 1-6.
- [ ] Greps en verde y prompts repetidos.
- [ ] `/code-review` atendido.
- [ ] Merge `--no-ff` en `docs/mwg-audit-sprint-01`, push a `fork` y borrar el worktree (sin PR).

## Criterios de aceptación

Con `F=skills/desy-implement-component/SKILL.md`:

- `grep -c 'Choque con MWG .dialog.' $F` ≥ 1, con el texto literal del README.
- `grep -n 'focusFirst\|openDialog(' $F` aparece en la confirmación destructiva; `grep -n 'foco en.*título' $F` = 0.
- `grep -nE '\[disabled\]=".*invalid' $F` = 0.
- `grep -n 'aria-invalid="true"' $F`: solo dentro del patrón de errores, explicado como lo que emite la macro.
- `grep -n 'title=' $F | grep -i ventana` = 0.
- `grep -n 'role="img"\|role=img' $F`: solo para SVG informativo suelto, nunca dentro de un `<button>`.
- El prompt del modal genera `openDialog` con foco en Cancelar y ningún `disabled` en el submit.

## Skills recomendados

- `writing-for-agents`: al reescribir la prosa del SKILL.md.
- `modern-web-guidance`: para recuperar la guía citada y verificar la regla antes de reescribir.
- `desy-validate-accessibility`: su checklist, para revisar los ejemplos tocados.

Cierre: `/implement` para ejecutar, `/code-review` antes de la PR, merge en `docs/mwg-audit-sprint-01` (sin PR por fase; la PR única la abre la fase 5 con `pr`).

## PROMPT

```
Vamos a ejecutar la Fase 1 del Sprint 01 de "desy-skills".

Lee enteros docs/sprints/01/README.md y docs/sprints/01/fase-1-implement-component.md, y además:
skills/desy-implement-component/SKILL.md (entero) y la sección «Correcciones» de docs/modern-web-guidance-gap.md.

Las decisiones y las costuras de test están cerradas en esos documentos: no se re-preguntan.
Orden: modales → submit → errores → ventana nueva → botón solo-icono. Para la línea de choque y el patrón de errores, copia el texto literal del README.
Para los prompts de verificación, abre una sesión nueva e indica la ruta del SKILL.md del worktree (los enlaces de ~/.claude/skills apuntan al checkout principal).

Crea el worktree y la rama fase-1-implement-component desde docs/mwg-audit-sprint-01 y lanza:
/implement docs/sprints/01/fase-1-implement-component.md
Las costuras de la fase son los greps y los prompts: ejecútalos antes (rojo) y después (verde). Para /code-review, el punto fijo es docs/mwg-audit-sprint-01 y la spec es
docs/sprints/01/fase-1-implement-component.md. Atiende sus hallazgos y después mergea con --no-ff en docs/mwg-audit-sprint-01 y haz push a fork. No abras PR: la única PR la abre la fase 5.
```
