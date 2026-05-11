# Harness Engineering: Guía de Implementación y Mejores Prácticas

## 1. Objetivo del documento

Este documento detalla la configuración de un **arnés de trabajo para agentes de Inteligencia Artificial** dentro de un proyecto de software.

El concepto de **Harness Engineering** se refiere a la práctica de construir un entorno controlado, auditable y repetible para dirigir el comportamiento de un modelo de IA. En lugar de tratar al agente como un simple chatbot, se lo integra como parte de un sistema de desarrollo robusto, con reglas, validaciones, memoria externa, protocolos de trabajo y mecanismos de verificación continua.

El objetivo principal es que los agentes puedan trabajar sobre un repositorio real sin depender únicamente del historial del chat, reduciendo errores, mejorando la trazabilidad y obligando a validar técnicamente cada avance.

---

## 2. Principios generales del Harness Engineering

Un harness para agentes debe cumplir con los siguientes principios:

1. **El repositorio es la fuente de verdad.**  
   Las reglas, protocolos, tareas, avances y validaciones deben vivir dentro del propio repositorio.

2. **El agente no debe trabajar sin protocolo.**  
   Antes de modificar código, debe leer las reglas del proyecto y validar el estado actual del entorno.

3. **Toda tarea debe tener criterios de aceptación.**  
   Ninguna funcionalidad debería implementarse si no está claro qué significa que esté terminada.

4. **La IA debe demostrar resultados, no solo declararlos.**  
   No alcanza con que el agente diga “terminado”. Debe ejecutar tests, validaciones o revisiones.

5. **El contexto debe ser administrado activamente.**  
   La memoria importante no debe quedar únicamente en la conversación. Debe externalizarse en archivos.

6. **El sistema debe ser simple de operar.**  
   Herramientas básicas, reglas claras y archivos fáciles de auditar suelen ser más efectivos que configuraciones complejas.

---

## 3. Los tres pilares de la arquitectura

Para que un sistema de agentes sea efectivo, debe sostenerse sobre tres pilares fundamentales.

### 3.1. El repositorio como sistema

El harness no debe funcionar como una herramienta externa aislada. Debe vivir dentro del propio repositorio del proyecto mediante archivos de configuración, reglas, protocolos, scripts y registros de progreso.

Esto permite que cualquier agente, desarrollador o revisor pueda entender el estado del proyecto leyendo los archivos del repositorio, sin depender de una conversación previa.

Ejemplos de archivos importantes:

```text
/agents.md
/init.sh
/features.json
/progress/
/tests/
/docs/
```

Beneficios:

- Mejora la trazabilidad.
- Reduce la dependencia del historial del chat.
- Permite que nuevos agentes retomen el trabajo con mayor precisión.
- Facilita la auditoría técnica del proceso.
- Permite versionar las reglas junto con el código.

---

### 3.2. Orquestación multi-agente

En lugar de utilizar un único agente para todo, se recomienda dividir responsabilidades entre distintos roles.

Un esquema recomendado sería:

| Rol | Responsabilidad principal |
|---|---|
| Agente orquestador | Lee el protocolo, selecciona tareas, coordina sub-agentes y valida el cierre. |
| Agente implementador | Desarrolla una funcionalidad específica siguiendo criterios de aceptación. |
| Agente revisor | Evalúa el trabajo del implementador, detecta errores y exige correcciones. |
| Agente explorador | Investiga el código existente, dependencias, patrones y posibles riesgos. |
| Agente de testing | Propone o ejecuta pruebas automatizadas para validar la implementación. |

La ventaja de este enfoque es que cada agente recibe solo el contexto mínimo necesario para su tarea. Esto evita que un único agente tenga demasiada información, pierda foco o arrastre supuestos incorrectos.

---

### 3.3. Verificación continua

El sistema debe incluir mecanismos obligatorios para validar que el trabajo realizado es correcto.

Esto implica:

- Ejecutar tests antes de comenzar.
- Ejecutar tests después de modificar código.
- Validar formato de archivos críticos.
- Revisar criterios de aceptación.
- Registrar resultados en archivos de progreso.
- Bloquear el cierre de tareas si las validaciones fallan.

Regla central:

> Una tarea no puede marcarse como terminada si no existe evidencia técnica de que funciona correctamente.

---

## 4. Puntos de entrada y archivos clave

### 4.1. `agents.md`: el cerebro del harness

El archivo `agents.md` es el punto de entrada principal para los agentes. Debe ser lo primero que el modelo lea al iniciar una sesión de trabajo.

Su función es definir el protocolo operativo del proyecto.

Debe incluir:

- Objetivo general del proyecto.
- Reglas obligatorias para los agentes.
- Mapa del repositorio.
- Convenciones de código.
- Flujo de trabajo esperado.
- Criterios mínimos de calidad.
- Comandos permitidos o recomendados.
- Condiciones para marcar una tarea como finalizada.
- Instrucciones para registrar avances.
- Protocolo de revisión.

Ejemplo de contenido mínimo:

```md
# agents.md

## Protocolo obligatorio

1. Leer este archivo antes de comenzar cualquier tarea.
2. Ejecutar `./init.sh` antes de modificar código.
3. Revisar `features.json` para seleccionar una tarea pendiente.
4. Registrar avances en `/progress`.
5. Ejecutar tests antes de marcar una tarea como hecha.
6. No declarar una tarea como finalizada si existen tests fallando.

## Mapa del repositorio

- `/src`: código fuente principal.
- `/tests`: pruebas automatizadas.
- `/docs`: documentación técnica.
- `/progress`: registro histórico de avances.
- `/features.json`: backlog estructurado de funcionalidades.

## Reglas de calidad

- No modificar código sin entender el contexto mínimo.
- No eliminar tests existentes sin justificación.
- No cambiar contratos públicos sin documentarlo.
- No marcar una tarea como hecha sin pruebas exitosas.
```

---

### 4.2. `init.sh`: script de inicialización y validación

El archivo `init.sh` debe validar que el entorno esté en condiciones correctas para que el agente trabaje.

Sus responsabilidades principales son:

1. Validar dependencias necesarias.
2. Verificar existencia de archivos críticos.
3. Validar formato de archivos de configuración.
4. Ejecutar la suite de tests actual.
5. Detener el trabajo si la base está inestable.

Ejemplo conceptual:

```bash
#!/usr/bin/env bash
set -e

echo "Validando entorno del harness..."

# Validar archivos críticos
[ -f "agents.md" ] || { echo "Falta agents.md"; exit 1; }
[ -f "features.json" ] || { echo "Falta features.json"; exit 1; }

# Validar herramientas básicas
command -v python3 >/dev/null 2>&1 || { echo "Python3 no está instalado"; exit 1; }
command -v grep >/dev/null 2>&1 || { echo "grep no está disponible"; exit 1; }

# Validar JSON
python3 -m json.tool features.json >/dev/null

# Ejecutar tests
npm test

echo "Entorno validado correctamente."
```

Regla importante:

> Si los tests actuales fallan antes de comenzar, el agente debe detenerse y reportar el problema. No debe construir sobre una base rota.

---

### 4.3. `features.json`: gestión estructurada de tareas

El archivo `features.json` funciona como una fuente de verdad para las tareas del agente.

Permite evitar que los objetivos dependan únicamente del historial del chat.

Debe contener:

- Identificador de la tarea.
- Nombre de la funcionalidad.
- Descripción.
- Estado.
- Prioridad.
- Criterios de aceptación.
- Archivos relacionados.
- Riesgos conocidos.
- Resultado de validación.

Estados sugeridos:

```text
pending
in_progress
blocked
done
rejected
```

Ejemplo:

```json
{
  "features": [
    {
      "id": "FEAT-001",
      "title": "Agregar validación de schema para endpoints custom",
      "status": "pending",
      "priority": "high",
      "description": "Implementar validación estricta para la colección endpointsCustom.",
      "acceptanceCriteria": [
        "La colección debe rechazar documentos inválidos.",
        "Los campos obligatorios deben estar definidos.",
        "Los enums deben estar limitados.",
        "Debe existir test automatizado de validación."
      ],
      "relatedFiles": [
        "src/modules/endpointsCustom",
        "tests/endpointsCustom"
      ],
      "validation": {
        "testsPassed": false,
        "reviewedByAgent": false,
        "lastRun": null
      }
    }
  ]
}
```

Regla recomendada:

> El agente debe cambiar una tarea a `in_progress` antes de trabajarla y solo puede pasarla a `done` si todos los criterios de aceptación fueron validados.

---

## 5. Gestión del contexto y memoria externa

La eficiencia de un agente disminuye cuando su ventana de contexto se llena. Por eso, el harness debe evitar que toda la memoria del proyecto viva dentro del chat.

### 5.1. Externalización de la memoria

La información relevante debe guardarse en archivos dentro del repositorio.

Se recomienda usar una carpeta:

```text
/progress
```

Dentro de esa carpeta pueden existir archivos como:

```text
/progress/2026-05-11-FEAT-001-analysis.md
/progress/2026-05-11-FEAT-001-implementation.md
/progress/2026-05-11-FEAT-001-review.md
/progress/2026-05-11-FEAT-001-tests.md
```

Cada archivo debería registrar:

- Qué se hizo.
- Qué archivos se tocaron.
- Qué decisiones se tomaron.
- Qué problemas aparecieron.
- Qué pruebas se ejecutaron.
- Qué quedó pendiente.

---

### 5.2. Evitar el “teléfono descompuesto”

Los sub-agentes no deben heredar todo el contexto del agente padre. Deben recibir solo la información necesaria para cumplir una tarea específica.

Ejemplo incorrecto:

> Pasar al sub-agente toda la conversación, todas las reglas y todo el historial del proyecto.

Ejemplo correcto:

> Pasar al sub-agente el ID de la tarea, los criterios de aceptación, los archivos relevantes y el resultado esperado.

Esto mejora:

- Precisión.
- Velocidad.
- Uso de tokens.
- Capacidad de revisión.
- Separación de responsabilidades.

---

### 5.3. Registro de progreso

Todo avance relevante debe quedar registrado.

Ejemplo de archivo de progreso:

```md
# FEAT-001 - Validación de schema para endpointsCustom

## Estado
En progreso

## Objetivo
Agregar validación estricta para documentos de endpoints custom.

## Archivos revisados
- src/modules/endpointsCustom/model.ts
- src/modules/endpointsCustom/repository.ts
- tests/endpointsCustom/schema.test.ts

## Decisiones tomadas
- Se validarán campos obligatorios en MongoDB.
- Se mantendrán validaciones de negocio en Node.js.
- Los enums se restringirán en ambos niveles.

## Tests ejecutados
- npm test

## Resultado
Pendiente de revisión final.
```

---

## 6. Herramientas minimalistas y validación

Un error común es dar a los agentes herramientas demasiado complejas. En la práctica, las herramientas simples suelen producir mejores resultados.

### 6.1. Herramientas recomendadas

Se recomienda habilitar herramientas básicas como:

| Herramienta | Uso |
|---|---|
| `ls` | Listar archivos y carpetas. |
| `cat` | Leer archivos completos pequeños. |
| `grep` | Buscar texto o patrones. |
| `find` | Localizar archivos. |
| `jq` | Validar y consultar JSON. |
| `npm test` | Ejecutar tests en proyectos Node.js. |
| `git diff` | Ver cambios realizados. |
| `git status` | Revisar estado del repositorio. |

La ventaja de estas herramientas es que son predecibles, auditables y fáciles de usar por cualquier agente o desarrollador.

---

### 6.2. Demostración de resultados

No se debe confiar únicamente en la declaración del agente.

El harness debe exigir evidencia, por ejemplo:

- Salida de tests.
- Resultado de linters.
- Validación de JSON.
- Revisión de diferencias con `git diff`.
- Informe del agente revisor.
- Cumplimiento explícito de criterios de aceptación.

Regla recomendada:

> Toda tarea cerrada debe incluir una sección de evidencia técnica.

Ejemplo:

```md
## Evidencia técnica

- `npm test`: exitoso.
- `npm run lint`: exitoso.
- `python3 -m json.tool features.json`: exitoso.
- Revisión de criterios de aceptación: completa.
```

---

### 6.3. Capacidad de automejora

El harness puede mejorar con el tiempo.

Si el agente revisor detecta fallos recurrentes, se deben actualizar las reglas del proyecto en `agents.md`.

Ejemplo:

```md
## Nueva regla agregada

Cuando se modifique una colección MongoDB, el agente debe revisar:

1. Schema validator.
2. Índices.
3. Campos obligatorios.
4. Enums.
5. Tests de inserción válida e inválida.
```

Esto permite que el sistema aprenda de errores previos y reduzca la repetición de fallos.

---

## 7. Flujo de trabajo recomendado del agente

El flujo operativo recomendado es el siguiente:

### Paso 1: carga de protocolo

El agente debe leer:

```text
agents.md
```

Objetivo:

- Entender las reglas del proyecto.
- Conocer el mapa del repositorio.
- Identificar restricciones y criterios mínimos de calidad.

---

### Paso 2: validación inicial

El agente ejecuta:

```bash
./init.sh
```

Si hay errores:

- Debe detenerse.
- Debe reportar el problema.
- No debe comenzar a modificar código.

---

### Paso 3: selección de tarea

El agente consulta:

```text
features.json
```

Luego selecciona una tarea `pending` y la marca como:

```text
in_progress
```

---

### Paso 4: exploración controlada

Antes de modificar código, el agente debe revisar solo los archivos relevantes.

Herramientas recomendadas:

```bash
ls
find
grep
cat
```

Objetivo:

- Entender patrones existentes.
- Evitar cambios innecesarios.
- Detectar dependencias.
- Identificar tests relacionados.

---

### Paso 5: implementación

El agente implementa la funcionalidad respetando:

- Convenciones del proyecto.
- Criterios de aceptación.
- Reglas de `agents.md`.
- Patrones existentes en el código.

---

### Paso 6: registro de avance

El agente escribe un archivo en:

```text
/progress
```

Debe registrar:

- Tarea trabajada.
- Archivos modificados.
- Decisiones tomadas.
- Riesgos encontrados.
- Validaciones ejecutadas.

---

### Paso 7: revisión

Un agente revisor o un desarrollador debe validar:

- Si la solución cumple el objetivo.
- Si respeta arquitectura.
- Si no introduce deuda técnica innecesaria.
- Si los tests son suficientes.
- Si hay cambios que deban documentarse.

---

### Paso 8: cierre y verificación

Antes de marcar la tarea como `done`, el agente debe ejecutar:

```bash
npm test
npm run lint
```

O los comandos equivalentes del proyecto.

Solo si todo está correcto puede actualizar `features.json`:

```json
{
  "status": "done"
}
```

---

## 8. Estructura sugerida del repositorio

Una estructura mínima recomendada sería:

```text
/project-root
│
├── agents.md
├── init.sh
├── features.json
│
├── progress/
│   ├── README.md
│   ├── 2026-05-11-FEAT-001-analysis.md
│   ├── 2026-05-11-FEAT-001-implementation.md
│   └── 2026-05-11-FEAT-001-review.md
│
├── docs/
│   └── architecture.md
│
├── src/
│   └── ...
│
├── tests/
│   └── ...
│
└── package.json
```

---

## 9. Checklist operativo para el equipo

Antes de permitir que un agente trabaje sobre el repositorio, validar lo siguiente:

### Configuración base

- [ ] Existe `agents.md`.
- [ ] Existe `init.sh`.
- [ ] Existe `features.json`.
- [ ] Existe carpeta `/progress`.
- [ ] El script `init.sh` tiene permisos de ejecución.
- [ ] Los comandos de test están documentados.
- [ ] Los criterios de aceptación están definidos por tarea.

### Validación técnica

- [ ] `features.json` es JSON válido.
- [ ] Los tests actuales pasan antes de comenzar.
- [ ] El agente no puede cerrar tareas sin evidencia técnica.
- [ ] Existe un mecanismo de revisión.
- [ ] Los cambios importantes quedan registrados en `/progress`.

### Control de calidad

- [ ] El agente usa herramientas simples y auditables.
- [ ] No se modifica código sin análisis previo.
- [ ] No se eliminan tests sin justificación.
- [ ] No se marcan tareas como terminadas sin tests exitosos.
- [ ] Las reglas recurrentes se incorporan a `agents.md`.

---

## 10. Reglas mínimas recomendadas para `agents.md`

Estas reglas pueden copiarse como base para cualquier proyecto:

```md
# Reglas obligatorias para agentes

1. Antes de trabajar, leer este archivo completo.
2. Ejecutar `./init.sh` antes de modificar código.
3. Si los tests iniciales fallan, detenerse y reportar el problema.
4. Consultar `features.json` antes de seleccionar una tarea.
5. Marcar la tarea como `in_progress` antes de comenzar.
6. Revisar únicamente el contexto necesario para la tarea.
7. Registrar avances en `/progress`.
8. Ejecutar tests y validaciones antes de cerrar.
9. No marcar una tarea como `done` si falla algún test.
10. Si se detecta un error recurrente, proponer una actualización de estas reglas.
```

---

## 11. Recomendación de implementación gradual

Para incorporar Harness Engineering en un equipo de desarrollo, se recomienda avanzar por etapas.

### Etapa 1: base mínima

Crear:

- `agents.md`
- `init.sh`
- `features.json`
- `/progress`

Objetivo:

> Que el agente tenga reglas, tareas y validación inicial.

---

### Etapa 2: validación automática

Agregar:

- Tests obligatorios.
- Validación de JSON.
- Validación de Markdown.
- Reglas de bloqueo si el entorno está roto.

Objetivo:

> Evitar que el agente trabaje sobre una base inestable.

---

### Etapa 3: revisión multi-agente

Agregar roles:

- Implementador.
- Revisor.
- Explorador.
- Tester.

Objetivo:

> Separar responsabilidades y mejorar la calidad del resultado.

---

### Etapa 4: automejora

Agregar reglas para que los errores recurrentes alimenten el propio harness.

Objetivo:

> Convertir aprendizajes técnicos en reglas permanentes del repositorio.

---

## 12. Conclusión

Harness Engineering permite transformar el uso de agentes de IA en un proceso controlado, verificable y alineado con buenas prácticas de ingeniería de software.

La clave no está en pedirle al agente que “haga algo”, sino en construir un sistema que lo obligue a trabajar con reglas, contexto mínimo, memoria externa, validaciones y evidencia técnica.

Un buen harness reduce errores, mejora la trazabilidad, facilita el trabajo multi-agente y permite incorporar IA al ciclo de desarrollo sin perder control sobre la calidad del software.

