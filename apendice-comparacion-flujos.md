# Apéndice: Comparación de flujos — antes y después del arnés

**Documento complementario a:** Guía de Instalación y Uso: Ingeniería de Arnés
**Versión:** 1.0
**Última actualización:** 11/05/2026

---

## Propósito de este apéndice

Este documento complementa la guía principal con una **visualización lado a lado** del flujo de trabajo previo a la instalación del arnés y el flujo posterior. La intención es que el equipo entienda de un vistazo qué cambia en la práctica cotidiana cuando se opera con el arnés instalado.

Se recomienda leer la guía principal antes que este apéndice. Los conceptos clave (leader, subagente, feature, criterios de aceptación, checkpoints) se asumen conocidos.

---

## Diagrama

<svg width="100%" viewBox="0 0 680 510" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Comparación de flujos: antes y después del arnés">
  <title>Comparación de flujos: antes (sin arnés) vs con arnés instalado</title>
  <desc>Diagrama lado a lado mostrando las seis etapas de trabajo con Claude: pedido, planning, materialización del plan, ejecución, validación y cierre, con los artefactos generados en cada paso.</desc>
  <style>
    .gray-fill { fill: #F1EFE8; stroke: #5F5E5A; }
    .teal-fill { fill: #E1F5EE; stroke: #0F6E56; }
    .amber-fill { fill: #FAEEDA; stroke: #BA7517; }
    .gray-text { fill: #444441; }
    .teal-text { fill: #085041; }
    .amber-text { fill: #854F0B; }
    .th { font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 14px; font-weight: 500; }
    .ts { font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 12px; font-weight: 400; }
    .arr { stroke: #888780; stroke-width: 1.5; fill: none; }
    .center-label { font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 12px; font-weight: 400; fill: #888780; }
    .note { font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 12px; font-weight: 400; fill: #5F5E5A; font-style: italic; }
  </style>
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#888780" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <text class="th gray-text" x="140" y="30" text-anchor="middle">Antes (sin arnés)</text>
  <text class="th teal-text" x="540" y="30" text-anchor="middle">Con arnés</text>

  <!-- Etapa 1: Inicio -->
  <rect class="gray-fill" x="40" y="55" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="78" text-anchor="middle">Pedido del dev</text>
  <text class="ts gray-text" x="140" y="96" text-anchor="middle">Le pide algo a Claude</text>

  <rect class="teal-fill" x="440" y="55" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th teal-text" x="540" y="78" text-anchor="middle">Pedido al leader</text>
  <text class="ts teal-text" x="540" y="96" text-anchor="middle">Claude en rol leader</text>

  <line x1="140" y1="105" x2="140" y2="125" class="arr" marker-end="url(#arrow)"/>
  <line x1="540" y1="105" x2="540" y2="125" class="arr" marker-end="url(#arrow)"/>

  <!-- Etapa 2: Planning -->
  <rect class="gray-fill" x="40" y="125" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="148" text-anchor="middle">Conversación libre</text>
  <text class="ts gray-text" x="140" y="166" text-anchor="middle">Plan en chat</text>

  <rect class="teal-fill" x="440" y="125" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th teal-text" x="540" y="148" text-anchor="middle">Modo planning</text>
  <text class="ts teal-text" x="540" y="166" text-anchor="middle">Explorer si hace falta</text>

  <line x1="140" y1="175" x2="140" y2="195" class="arr" marker-end="url(#arrow)"/>
  <line x1="540" y1="175" x2="540" y2="195" class="arr" marker-end="url(#arrow)"/>

  <!-- Etapa 3: Materializar (destacada con ámbar en la derecha) -->
  <rect class="gray-fill" x="40" y="195" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="218" text-anchor="middle">Plan en archivo</text>
  <text class="ts gray-text" x="140" y="236" text-anchor="middle">plan_feature.md (libre)</text>

  <rect class="amber-fill" x="440" y="195" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th amber-text" x="540" y="218" text-anchor="middle">Crea feature</text>
  <text class="ts amber-text" x="540" y="236" text-anchor="middle">JSON + Markdown</text>

  <line x1="140" y1="245" x2="140" y2="265" class="arr" marker-end="url(#arrow)"/>
  <line x1="540" y1="245" x2="540" y2="265" class="arr" marker-end="url(#arrow)"/>

  <!-- Etapa 4: Ejecutar -->
  <rect class="gray-fill" x="40" y="265" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="288" text-anchor="middle">"Ejecutalo"</text>
  <text class="ts gray-text" x="140" y="306" text-anchor="middle">Implementa todo de corrido</text>

  <rect class="teal-fill" x="440" y="265" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th teal-text" x="540" y="288" text-anchor="middle">Delega a implementer</text>
  <text class="ts teal-text" x="540" y="306" text-anchor="middle">+ tests + progress/</text>

  <line x1="140" y1="315" x2="140" y2="335" class="arr" marker-end="url(#arrow)"/>
  <line x1="540" y1="315" x2="540" y2="335" class="arr" marker-end="url(#arrow)"/>

  <!-- Etapa 5: Validar (destacada con ámbar en la derecha) -->
  <rect class="gray-fill" x="40" y="335" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="358" text-anchor="middle">Revisión mental</text>
  <text class="ts gray-text" x="140" y="376" text-anchor="middle">Dev mira el output</text>

  <rect class="amber-fill" x="440" y="335" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th amber-text" x="540" y="358" text-anchor="middle">Reviewer agente</text>
  <text class="ts amber-text" x="540" y="376" text-anchor="middle">Valida criterios + C1-C9</text>

  <line x1="140" y1="385" x2="140" y2="405" class="arr" marker-end="url(#arrow)"/>
  <line x1="540" y1="385" x2="540" y2="405" class="arr" marker-end="url(#arrow)"/>

  <!-- Etapa 6: Cerrar -->
  <rect class="gray-fill" x="40" y="405" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th gray-text" x="140" y="428" text-anchor="middle">Commit</text>
  <text class="ts gray-text" x="140" y="446" text-anchor="middle">Si pareció bien</text>

  <rect class="teal-fill" x="440" y="405" width="200" height="50" rx="8" stroke-width="0.5"/>
  <text class="th teal-text" x="540" y="428" text-anchor="middle">Archivado</text>
  <text class="ts teal-text" x="540" y="446" text-anchor="middle">progress/archive/FEAT-XXX/</text>

  <!-- Etiquetas de etapa en el centro -->
  <text class="center-label" x="340" y="83" text-anchor="middle">Inicio</text>
  <text class="center-label" x="340" y="153" text-anchor="middle">Planning</text>
  <text class="center-label" x="340" y="223" text-anchor="middle">Materializar</text>
  <text class="center-label" x="340" y="293" text-anchor="middle">Ejecutar</text>
  <text class="center-label" x="340" y="363" text-anchor="middle">Validar</text>
  <text class="center-label" x="340" y="433" text-anchor="middle">Cerrar</text>

  <text class="note" x="340" y="490" text-anchor="middle">Las cajas en ámbar marcan los dos cambios estructurales importantes del arnés.</text>
</svg>

---

## Cómo leer el diagrama

Las **seis filas** representan **etapas equivalentes** entre los dos flujos. Lo importante no es la cantidad de pasos, sino lo que cambia en cada etapa.

| Etapa | Antes (sin arnés) | Con arnés |
|---|---|---|
| **1. Inicio** | El dev le pide algo a Claude conversacionalmente | El dev le pide algo a Claude, que ya está en rol de leader (cargado automáticamente desde `CLAUDE.md`) |
| **2. Planning** | Conversación libre, el plan se construye en el chat | Modo planning conversacional, con `explorer` lanzado si hace falta mapear contexto técnico |
| **3. Materializar** | El plan se escribe en un archivo libre (`plan_feature.md`) que mezcla criterios, razonamiento y bitácora | El plan se divide en dos lugares con propósitos distintos: `feature_list.json` (estructurado, verificable) + `progress/current.md` (razonamiento libre) |
| **4. Ejecutar** | Claude implementa todo de corrido en su contexto principal | El leader delega a un `implementer` o specialist que escribe código + tests + archivo de progreso en el mismo turno |
| **5. Validar** | Revisión mental: el dev mira el output y decide si está bien | Reviewer agente que valida cada criterio de aceptación uno por uno + los criterios globales C1-C9 antes de aprobar |
| **6. Cerrar** | Commit si pareció estar bien; el plan queda en el repo como archivo suelto | Archivado automático en `progress/archive/FEAT-XXX/` con todo el historial preservado (explore, implement, review) |

---

## Los dos cambios estructurales importantes (cajas en ámbar)

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
