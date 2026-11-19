# Fase 3 — desy-validate-accessibility

## Objetivo

Que la validación de accesibilidad (con servidor o sin él) revise modales con el choque declarado, errores con el patrón de servidor y criterios WCAG 2.2 AA correctos, y que el checklist offline detecte con grep los nuevos fallos: botones sin `type` dentro de `<form>`, submit deshabilitado, `<html lang>`, `<title>` y `<meta viewport>`.

## Bloqueada por

Ninguna (puede empezar ya). Puede ir en paralelo con F1, F2 y F4.

## Lo que hay hoy

Foto del 2026-11-19 sobre `skills/desy-validate-accessibility/SKILL.md` (470 líneas). Si el código ha cambiado, manda el código.

- **56-72**: checklist sin servidor (greps sobre `dist/*.html`); no comprueba `lang`, `title` ni `viewport` (hallazgo 19).
- **313-319, 370**: checklist de modales sin choque declarado; «foco en título» en la confirmación.
- **297** y **436-447**: ejemplo de corrección de errores con `aria-invalid="true"` fijo (hallazgo 10).
- **150, 285**: `title` para «ventana nueva» (hallazgo 13).
- **284, 327**: botón solo-icono con `role=img` en el SVG (hallazgo 14).
- **175-176**: umbrales de contraste mezclados (hallazgo 17).
- **271**: cita 2.5.5 (AAA) y omite 2.5.8 Target Size Minimum (AA en WCAG 2.2) (hallazgo 18).

## Decisiones cerradas

| Tema | Decisión |
|---|---|
| Modales | Checklist intacto + línea de choque de modales del README; foco inicial en Cancelar. |
| Errores | Patrón de errores del README, literal. |
| Contraste | Texto normal 4.5:1. Texto grande (≥24 px, o ≥18.66 px en negrita) 3:1. Componentes de interfaz 3:1 (`color.md:17`). |
| Target size | 2.5.8 (AA): 24×24 px CSS como mínimo o espaciado equivalente. 2.5.5 (44×44 px) queda como recomendación AAA. |
| Greps nuevos | En el checklist sin servidor, con el mismo estilo que los existentes. |

## Costuras de test de esta fase

1. Greps (criterios de aceptación) sobre `skills/desy-validate-accessibility/SKILL.md`.
2. Los greps nuevos del checklist, ejecutados sobre `~/desy-evals-work/r180/dist/*.html`: deben correr sin error y devolver resultados razonables.
3. Prompts de §1 `desy-validate-accessibility`.

## Alcance

1. Línea de choque de modales y foco en Cancelar (313-319, 370).
2. Ejemplo de errores (436-447) y checklist (297) con el patrón de errores del README.
3. Ventana nueva sin `title` (150, 285).
4. Botón solo-icono (284, 327).
5. Tabla de contraste (175-176).
6. Sustituir 2.5.5 por 2.5.8, dejando 2.5.5 como AAA (271).
7. Greps nuevos en 56-72: `<button` sin `type` dentro de `<form>`, submit con `disabled`, `<html` sin `lang`, falta `<title>`, falta `<meta name="viewport"`.

## Fuera de alcance

Hallazgos 16 (tablas), 20 (reflow 320 px), 22 y 26-27 (severidad baja o fuera del sprint).

## Tareas

- [ ] Rama `fase-3-validate-accessibility` desde `main` (worktree propio).
- [ ] Greps en rojo.
- [ ] Alcance 1-7.
- [ ] Greps nuevos probados sobre `r180/dist`.
- [ ] `/code-review` atendido.
- [ ] PR.

## Criterios de aceptación

Con `F=skills/desy-validate-accessibility/SKILL.md`:

- `grep -c 'Choque con MWG .dialog.' $F` ≥ 1, con el texto literal del README.
- `grep -n 'foco en.*título' $F` = 0.
- `grep -n 'aria-invalid="true"' $F`: solo dentro del patrón de errores, como lo que emite la macro.
- `grep -n 'title=' $F | grep -i ventana` = 0.
- `grep -n '2.5.8' $F` ≥ 1, y 2.5.5 aparece solo marcado como AAA.
- `grep -nE '≥ ?18 ?px|18px' $F` no aparece asociado a «texto normal».
- El checklist sin servidor contiene 5 comandos nuevos que se ejecutan sin error sobre `~/desy-evals-work/r180/dist`.

## Skills recomendados

- `writing-for-agents`: al reescribir la prosa del SKILL.md.
- `modern-web-guidance`: para recuperar la guía citada y verificar la regla antes de reescribir.
- `desy-validate-accessibility`: su checklist, para revisar los ejemplos tocados.

Cierre: `/implement` para ejecutar, `/code-review` antes de la PR, `pr` para el cuerpo de la PR.

## PROMPT

```
Vamos a ejecutar la Fase 3 del Sprint 01 de "desy-skills".

Lee enteros docs/sprints/01/README.md y docs/sprints/01/fase-3-validate-accessibility.md, y además:
skills/desy-validate-accessibility/SKILL.md (entero) y la sección «Correcciones» de docs/modern-web-guidance-gap.md.

Las decisiones y las costuras de test están cerradas en esos documentos: no se re-preguntan.
Orden: greps nuevos → modales → errores → resto. Para la línea de choque y el patrón de errores, copia el texto literal del README.
Para los prompts de verificación, abre una sesión nueva e indica la ruta del SKILL.md del worktree (los enlaces de ~/.claude/skills apuntan al checkout principal).

Crea la rama fase-3-validate-accessibility desde main y lanza:
/implement docs/sprints/01/fase-3-validate-accessibility.md
Las costuras de la fase son los greps y los prompts: ejecútalos antes (rojo) y después (verde). Para /code-review, el punto fijo es main y la spec es
docs/sprints/01/fase-3-validate-accessibility.md. Atiende sus hallazgos antes de abrir la PR.
```
