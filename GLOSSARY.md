# desy-skills

Skills para agentes que implementan, revisan y afinan páginas con el Design System DESY (Gobierno de Aragón), contrastados con modern-web-guidance (MWG).

## Fuentes de verdad

**Macro oficial**:
Componente o patrón publicado por DESY (desy-html, desy-angular, desy-ionic) cuyo marcado emite la propia librería.
_Avoid_: componente DESY (cuando se habla de lo que escribe el skill a mano), plantilla

**MWG**:
modern-web-guidance, el corpus de guías de buenas prácticas web de Google Chrome instalado como skill.
_Avoid_: guías de Google, best practices

## Relación entre DESY y MWG

**Choque**:
Regla de MWG que contradice el marcado de una macro oficial; manda DESY y el skill lo declara.
_Avoid_: conflicto, excepción, error de DESY

**Hueco**:
Instrucción o código que el skill añade por su cuenta y que contradice u omite una regla de MWG; se corrige en el skill.
_Avoid_: bug, choque (cuando no hay macro oficial detrás)

**Remitir a MWG**:
Que un skill DESY indique consultar una guía de MWG para el HTML genérico en vez de copiar sus reglas.
_Avoid_: delegar, duplicar

**Propuesta SDA**:
Choque con riesgo real que se anota para proponer el cambio al equipo que mantiene DESY.
_Avoid_: issue upstream, petición
