# Apéndice: Comparación de flujos — antes y después del arnés

**Documento complementario a:** Guía de Instalación y Uso: Ingeniería de Arnés
**Versión:** 1.0
**Última actualización:** 11/05/2026

---

## Propósito de este apéndice

Este documento complementa la guía principal con una **comparación lado a lado** del flujo de trabajo previo a la instalación del arnés y el flujo posterior. La intención es que el equipo entienda de un vistazo qué cambia en la práctica cotidiana cuando se opera con el arnés instalado.

Se recomienda leer la guía principal antes que este apéndice. Los conceptos clave (leader, subagente, feature, criterios de aceptación, checkpoints) se asumen conocidos.

---

## Cuadro comparativo

Las seis filas son **etapas equivalentes** entre los dos flujos. Lo importante no es la cantidad de pasos, sino lo que cambia en cada etapa.

| # | Etapa | Antes (sin arnés) | Con arnés |
|---|---|---|---|
| 1 | Inicio | Pedido del dev a Claude | Pedido al leader (rol auto-cargado vía `CLAUDE.md`) |
| 2 | Planning | Conversación libre, plan en chat | Modo planning conversacional, con `explorer` si hace falta mapear contexto |
| 3 | **Materializar** ★ | Plan en archivo libre `plan_feature.md` (mezcla criterios, razonamiento, bitácora) | `feature_list.json` (estructurado) + `progress/current.md` (razonamiento libre) |
| 4 | Ejecutar | Claude implementa todo de corrido en su contexto principal | Leader delega a `implementer` o specialist, que escribe código + tests + `progress/implement_*.md` |
| 5 | **Validar** ★ | Revisión mental: el dev mira el output y decide si está bien | `reviewer` agente valida criterios de aceptación + criterios globales C1-C9 |
| 6 | Cerrar | Commit si pareció bien; el plan queda como archivo suelto | Archivado automático en `progress/archive/FEAT-XXX/` con todo el historial preservado |

**★ Las etapas 3 y 5 son los dos cambios estructurales más importantes del arnés.** Se explican en detalle en la sección siguiente.

---

## Los dos cambios estructurales importantes (etapas 3 y 5)

### Cambio 1: la materialización del plan (etapa 3)

**Antes:** el plan vivía en un archivo libre. Criterios, razonamiento, alternativas consideradas, decisiones, bitácora, todo mezclado. Útil para escribir, pero difícil de auditar después: cuando alguien quería saber **qué tenía que cumplir** la feature, tenía que leer todo el documento para encontrar los criterios entre el razonamiento.

**Ahora:** el plan se divide en dos archivos según naturaleza:

- **`feature_list.json`** guarda solo lo **estructurado y verificable**:
  - ID de la feature.
  - Título corto.
  - Status (`pending`, `in_progress`, `paused`, `blocked`, `done`, `rejected`).
  - Criterios de aceptación (lista verificable).
  - Archivos relacionados.
  - Riesgos detectados.
  - Flags de validación.

- **`progress/current.md`** guarda lo **libre y narrativo**:
  - Razonamiento del modo planning.
  - Alternativas consideradas y por qué se descartaron.
  - Plan de descomposición (qué subagente para cada parte).
  - Bitácora de la sesión (timestamps, decisiones intermedias).
  - Bloqueos detectados.

**Beneficio operativo:** si el contexto se corta y otro dev (o el mismo, mañana) abre el proyecto, en 30 segundos sabe dónde está el trabajo. Mira `feature_list.json` para el estado, `progress/current.md` para el detalle. No tiene que reconstruir nada del chat.

### Cambio 2: la validación (etapa 5)

**Antes:** la validación era **mental**. El dev miraba el output del trabajo de Claude y decidía si estaba bien. Subjetiva, dependiente del estado de atención del dev, sin criterios explícitos.

**Ahora:** hay un **reviewer agente** que valida contra criterios objetivos antes de cerrar la feature:

- **Criterios específicos de la feature** (los `acceptanceCriteria` definidos en el modo planning, uno por uno).
- **Criterios globales C1 a C9** definidos en `CHECKPOINTS.md`:
  - C1: tests verdes obligatorios.
  - C2: linter limpio (solo warnings de la lista tolerable).
  - C3: cobertura del diff (funciones modificadas tienen tests).
  - C4: sin deuda técnica visible (sin TODOs huérfanos, sin código comentado).
  - C5: builds compilan.
  - C6: migraciones aplicables (si la feature toca DB).
  - C7: contratos públicos documentados.
  - C8: documentación de módulos nuevos.
  - C9: sin nuevos tests marcados como `@flaky` en la sesión.

El reviewer **vuelve a ejecutar** los comandos de validación (no confía en los outputs pegados por el implementer) y emite un veredicto binario: `approved` o `rejected_implementation` (con feedback específico).

**Beneficio operativo:** se atrapan problemas que la revisión mental dejaba pasar (tests que faltan, criterios mal cumplidos, deuda técnica acumulada). El feedback es accionable y queda registrado en `progress/review_FEAT-XXX.md`.

---

## Las otras etapas (cambios menores pero relevantes)

**Etapa 1 — Inicio:**
Claude entra automáticamente en rol leader gracias al `CLAUDE.md` que se carga al abrir el proyecto. El dev no tiene que aclararle "actuá como X". El cambio es invisible pero garantiza consistencia entre sesiones y entre miembros del equipo.

**Etapa 2 — Planning:**
La conversación previa a la ejecución se preserva (no se pierde ese hábito útil), pero ahora termina con una **propuesta concreta** del leader (ID, criterios, archivos, riesgos) que el dev confirma o ajusta antes de avanzar. Si la solicitud es ambigua, el leader puede lanzar primero un `explorer` para mapear el contexto técnico necesario.

**Etapa 4 — Ejecutar:**
La ejecución deja de ser monolítica. En vez de un Claude que hace todo de corrido, el leader delega a un subagente especializado (un `implementer` genérico o un specialist del proyecto) que entra con contexto limpio y se enfoca solo en su trabajo. El subagente escribe código + tests en el mismo turno y devuelve al leader un resumen de ≤ 10 líneas + la ruta del archivo de progreso (no el contenido completo). Esto preserva el contexto del leader para coordinar varias features sin perder hilo.

**Etapa 6 — Cerrar:**
Cuando el reviewer aprueba, el leader ejecuta el archivado: mueve todos los archivos de la feature de `progress/` a `progress/archive/FEAT-XXX/`. La carpeta `progress/` solo muestra features activas. El historial completo (explore, implement, review) queda preservado y se puede buscar después con `grep`.

---

## Implicaciones para el equipo

Algunos cambios prácticos en la cotidianidad del desarrollo que conviene anticipar:

### El plan ya no es "un archivo"

Antes los devs tenían el hábito de buscar `plan_*.md` para entender qué se había planteado en una sesión. Ahora deben mirar dos lugares:

- `feature_list.json` para saber **qué** hay que hacer (lo estructurado).
- `progress/current.md` o `progress/archive/FEAT-XXX/` para saber **por qué** se decidió eso (lo narrativo).

Inicialmente puede sentirse como un paso extra. Después de un par de semanas se vuelve natural y la ganancia en trazabilidad compensa el costo.

### Confirmar antes de ejecutar

En el modo planning, el leader **propone** criterios y archivos, y **espera confirmación** antes de avanzar. Esto no es opcional: el leader no debería crear una feature sin OK explícito del dev. Si el dev se acostumbra a decir "sí" sin leer, el sistema pierde su valor de control.

**Buena práctica:** leer la propuesta del leader, ajustar criterios si hace falta, **recién entonces** confirmar.

### El rechazo del reviewer es esperado

A diferencia de la revisión mental (donde el dev solía aprobar mentalmente y avanzar), el reviewer agente **rechaza con frecuencia** en el ciclo de adopción inicial:

- Si el implementer no escribió tests del cambio → rechazo.
- Si tests pasan pero un criterio de aceptación no se cumple → rechazo.
- Si quedó un `console.log` olvidado → rechazo (C4).
- Si se marcó algún test como `@flaky` durante la sesión → rechazo (C9).

Cada rechazo es información útil: indica qué falta para considerar la feature terminada. El leader maneja hasta **2 ciclos** de rechazo, después escala al dev.

**Buena práctica:** tratar el rechazo del reviewer como una segunda pasada del implementer, no como una falla del sistema.

### Una feature activa a la vez

El arnés impone que solo puede haber **una feature con status `in_progress`** en `feature_list.json` en cualquier momento. Si una nueva tarea interrumpe a la activa:

1. Registrar el estado actual en `progress/current.md`.
2. Cambiar la activa a `paused` en `feature_list.json`.
3. Recién entonces marcar la nueva como `in_progress`.

Esto evita el patrón típico de "empecé tres features y no terminé ninguna". Para hotfixes urgentes hay un mecanismo de bypass específico (`[HOTFIX-BYPASS]`).

### Auditabilidad post-cierre

Después de cerrar una feature, todo su historial queda en `progress/archive/FEAT-XXX/`. Para entender por qué se hizo algo de una manera específica meses después, el equipo puede:

```bash
# Ver todo lo relacionado con una feature
ls progress/archive/FEAT-027/

# Buscar un término en todas las features archivadas
grep -r "termino" progress/archive/

# Ver el veredicto del reviewer
cat progress/archive/FEAT-027/review_FEAT-027.md
```

Esto reemplaza el patrón anterior de "preguntá a quien lo hizo" o "buscá en el chat de hace 3 meses".

---

## Resumen: la ganancia neta del arnés

| Aspecto | Antes | Con arnés |
|---|---|---|
| **Trazabilidad** | Plan en chat, se pierde | Todo en archivos del repo |
| **Reanudabilidad** | Reconstruir contexto de cero | `feature_list.json` + `current.md` |
| **Validación** | Mental, subjetiva | Mecánica, criterios objetivos |
| **Disciplina cross-stack** | Implícita | Checkpoint obligatorio entre etapas |
| **Audit post-cierre** | Releer plan si existe | `progress/archive/FEAT-XXX/` completo |
| **Onboarding de nuevos devs** | "Acordate de hacer X, Y, Z" | El protocolo está en archivos del repo |

---

## Referencias

- **Documento principal:** Guía de Instalación y Uso: Ingeniería de Arnés.
- **Archivos del arnés relevantes para este apéndice:**
  - `AGENTS.md` — mapa de navegación del arnés.
  - `CLAUDE.md` — binding del rol leader.
  - `.claude/agents/leader.md` — tabla de escalado y reglas operativas.
  - `.claude/agents/reviewer.md` — protocolo de validación.
  - `CHECKPOINTS.md` — criterios globales C1-C9.
  - `feature_list.json` — backlog estructurado.
  - `progress/` — memoria externa del arnés.
