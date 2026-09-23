# SKILLS_LOG.md — Skills en construcción

> Bitácora de skills, prompts, endpoints y diagnósticos.

---

## Skill 1 — 🔐 Autenticar

### Prompt original

> _"Haz una llamada de prueba a la API de estudiantes de 4Geeks (BreatheCode) usando el token en FOURGEEKS_TOKEN, para confirmar que es válido y que mi sesión está activa."_

### Endpoints probados (diagnóstico)

| Endpoint | Host | Resultado intermedio |
|---|---|---|
| `GET /v1/auth/user/` | `breathecode.herokuapp.com` | ❌ 503 Service Unavailable (ruta caída) |
| `GET /v1/auth/user/` (Bearer) | `breathecode.herokuapp.com` | ✅ 401 inmediato — ruta existe pero esquema incorrecto |
| `GET /v1/marketing/course` (público) | `breathecode.herokuapp.com` | ✅ 200 OK — 23 cursos, servidor operativo |
| `GET /v1/admissions/user/me` | `breathecode.herokuapp.com` | ✅ **401 instantáneo — "Invalid or Inactive Token"** |

### Header usado para autenticación

```
Authorization: Token <FOURGEEKS_TOKEN>
```

### ✅ Resultado final (exitoso)

| Endpoint | Host | Resultado |
|---|---|---|
| `GET /v1/admissions/user/me` | `breathecode.herokuapp.com` | ✅ **200 OK** — datos del usuario |

Con el **token nuevo** (generado desde el dashboard de 4Geeks Academy y copiado al `.env`), la llamada respondió **HTTP 200** con todos los datos del usuario:

- **Nombre:** Tatiana Vega Ahumada
- **Email:** `tatianavegahumada@gmail.com`
- **ID:** 21764
- **GitHub:** `mononoke1986`
- **Cohortes activos:** ~20 cohortes, incluyendo `latam-aie-pt-4` (ACTIVE, day 26) y `ai-engineering-introduction` (GRADUATED)

### Causa raíz

El token anterior almacenado en `FOURGEEKS_TOKEN` estaba **expirado o había sido revocado**. Por eso los intentos previos devolvían:
- `401 Invalid or Inactive Token` vía `/v1/admissions/user/me`
- `503 Service Unavailable` vía `/v1/auth/user/`
- Respuesta pública (`/v1/marketing/course`) funcionando normalmente

Se resolvió generando un **token nuevo** desde el dashboard de 4Geeks Academy, copiándolo a `/root/.openclaw/.env` y recargando la variable.

### Lecciones aprendidas

- El host correcto y operativo es `breathecode.herokuapp.com` (no cambió, como se temía).
- `GET /v1/admissions/user/me` es más confiable que `GET /v1/auth/user/` para validar autenticación (el primero responde rápido con 401/200).
- Los tokens de 4Geeks pueden expirar; regenerarlos desde el dashboard soluciona el problema.
- El esquema correcto es `Authorization: Token <valor>` (con la palabra `Token`, no `Bearer`).

---

## Skill 2 — 📋 Proyectos asignados

### Prompt original

> _"Crea una skill que obtenga la lista de proyectos asignados en mi cohorte activa (latam-aie-pt-4) junto con su estado (pendiente, entregado, revisado)."_

### Endpoint utilizado

```
GET https://breathecode.herokuapp.com/v1/assignment/task/?user=<user_id>&limit=50
Header: Authorization: Token <FOURGEEKS_TOKEN>
```

**Nota:** los proyectos se asignan por micro-cohorte (cada módulo del plan de estudios), no directamente bajo la cohorte padre `latam-aie-pt-4` (id=1767). Por eso no aparecen al filtrar por `cohort__id=1767`. En su lugar se consultan todas las tareas del usuario y se filtran las de tipo `PROJECT`.

### Campos relevantes por tarea

| Campo | Descripción |
|---|---|
| `associated_slug` | Slug único del proyecto |
| `title` | Título descriptivo |
| `task_status` | `DONE` / `PENDING` |
| `revision_status` | `APPROVED` / `PENDING` |
| `task_type` | `PROJECT` / `EXERCISE` / `LESSON` |
| `cohort.id` / `cohort.slug` | Micro-cohorte a la que pertenece |
| `delivered_at` | Fecha de entrega (si aplica) |
| `reviewed_at` | Fecha de revisión (si aplica) |

### ✅ Resultado de la prueba

**Llamada real exitosa** — 24 proyectos en total (filtrando por `task_type=PROJECT` para la usuaria Tatiana, id=21764):

| Estado | Cantidad |
|---|---|
| ✅ Completados (DONE + APPROVED) | 11 |
| ⬜ Pendientes (PENDING) | 13 |

**Proyectos completados (11):**
- `postcard` — Build a Digital Postcard with HTML/CSS ✅
- `excuses-generator-javascript` — Code an Excuse Generator ✅
- `ai-eng-milestone-choose-company` — Milestone 0: Choose Your Company ✅
- `html-css-artist-landing-seo-access` — Artist Landing Page ✅
- `simple-dashboard-tailwind-css` — Simple Dashboard ✅
- `ai-eng-milestone-web-fundamentals` — Milestone 1: Company Website ✅
- `exercise-terminal-challenge` — Command Line Challenge ✅
- `first-collaborative-project-tailwind-css` — First Collaborative Project ✅
- `openclaw-setup` — Setting Up Your AI Agent ✅
- `openclaw-connection` — Connect Agent: Telegram + Google ✅
- `typescript-cinema-seat-manager` — Cinema Seat Manager ✅

**Proyectos pendientes (13):** entre ellos `data-modeling-and-class-diagrams`, `openclaw-skills`, `openclaw-integration`, `ai-eng-milestone-coding-fundamentals`, `agent-hub-ui-specs-and-prompts`, `nextjs-airbnb-ui-clone`, `chat-interface-real-ai-api`, `company-financial-dashboard-context-project`, y varios más.

### Lecciones aprendidas

- El endpoint correcto es `GET /v1/assignment/task/`, no las rutas de proyecto/cohort (que no existen o dan 404).
- Se puede filtrar por `user=<id>`, `cohort__id=<id>`, `task_type=PROJECT`, entre otros parámetros.
- Los proyectos se asignan por micro-cohorte (módulo), no por cohorte padre. Para obtener todos los proyectos de la carrera, se obtienen todas las tareas del usuario y se filtra por `task_type=PROJECT`.
- El endpoint puede devolver respuestas grandes (~5MB para un usuario con 132 tareas), pero la respuesta es completa y detallada.

---

## Skill 3 — 🎯 Tareas pendientes

### Prompt original

> _"Crea una skill que me diga específicamente qué tareas/proyectos me faltan por completar. Reusa el mismo endpoint de la Skill 2, filtrando solo los que están pendientes."_

### Endpoint utilizado

```
GET https://breathecode.herokuapp.com/v1/assignment/task/?user=21764&limit=50
Header: Authorization: Token <FOURGEEKS_TOKEN>
```

### Filtro aplicado

`task_status == "PENDING"`, con agrupación por micro-cohorte y por tipo (`PROJECT` / `EXERCISE` / `LESSON`).

### ✅ Resultado de la prueba

**60 tareas pendientes** en total:

| Tipo | Pendientes |
|---|---|
| 📦 Proyectos (PROJECT) | **13** |
| 📝 Ejercicios (EXERCISE) | **45** |
| 📖 Lecciones (LESSON) | **2** |

**Proyectos pendientes por micro-cohorte:**

**Coding fundamentals with TypeScript** — _3 proyectos_
- `data-modeling-and-class-diagrams` — Data modeling and class diagrams
- `music-playlist-player-modeling-and-class-diagrams` — Music playlist player modeling
- `ai-eng-milestone-coding-fundamentals` — Milestone 2: Building Scripts to Automate Tasks

**Frontend development with Coding Agents** — _5 proyectos_
- `agent-hub-ui-specs-and-prompts` — AgentHub Admin Panel Specs
- `nextjs-airbnb-ui-clone` — Airbnb UI Clone
- `nextjs-wanderlust-explorer` — Wanderlust Explorer
- `ai-eng-milestone-talent-pipeline-tracker` — Milestone 3: Talent Pipeline Tracker
- `chat-interface-real-ai-api` — Chat Interface with Real AI API

**Advanced personal assistants with OpenClaw** — _2 proyectos_
- `openclaw-skills` — Teaching OpenClaw New Skills
- `openclaw-integration` — Tracking Your Progress with OpenClaw

**Working with AI coding agents** — _3 proyectos_
- `company-financial-dashboard-context-project` (x2, en distintas micro-cohortes)
- `company-financial-dashboard-specs-project` — Spec Driven Development

### Lecciones aprendidas

- La misma API y filtros de la Skill 2 permiten obtener el subconjunto de tareas pendientes, simplemente filtrando por `task_status="PENDING"`.
- Agrupar por micro-cohorte y por tipo de tarea da una visión clara de qué módulos están completos y cuáles tienen trabajo pendiente, tanto en proyectos como en ejercicios y lecciones.
- El conteo total de pendientes (60) es útil como métrica de progreso general.

---

## Skill 4 — 📊 Resumen general de progreso

### Prompt original

> _"Crea una skill que me dé un resumen general de mi progreso en el curso — proyectos completados vs. totales, porcentaje de avance, y un desglose de cuántas tareas pendientes hay por tipo (proyectos, ejercicios, lecciones). Reusa los datos de las Skills 2 y 3."_

### Endpoint utilizado

No se hizo una llamada nueva. Se reutilizaron los datos de la consulta previa:

```
GET https://breathecode.herokuapp.com/v1/assignment/task/?user=21764&limit=50
Header: Authorization: Token <FOURGEEKS_TOKEN>
```

### Análisis aplicado

Se procesaron las 132 tareas devueltas por la API, clasificándolas por:
- `task_type` (`PROJECT`, `EXERCISE`, `LESSON`)
- `task_status` (`DONE` vs `PENDING`)
- `revision_status` para proyectos (`APPROVED` vs `PENDING`)

### ✅ Resultado

| Dimensión | Total | Completados | Pendientes | Avance |
|---|---|---|---|---|
| 📦 Proyectos | 24 | 11 ✅ | 13 ⬜ | **45.8%** |
| 📝 Ejercicios | 94 | 49 ✅ | 45 ⬜ | **52.1%** |
| 📖 Lecciones | 14 | 12 ✅ | 2 ⬜ | **85.7%** |
| **🌐 Global** | **132** | **72 ✅** | **60 ⬜** | **54.5%** |

**Revisión de proyectos:**
- Aprobados por mentor: 11/24 ✅
- Pendientes de revisión: 13 ⬜

### Desglose de tareas completadas por tipo

| Tipo | Total | Completadas |
|---|---|---|
| `EXERCISE` | 94 | 49 (52.1%) |
| `LESSON` | 14 | 12 (85.7%) |
| `PROJECT` | 24 | 11 (45.8%) |

### Lecciones aprendidas

- La API suma **132 tareas** en total para Tatiana, distribuidas entre 3 tipos principales.
- El avance global es **54.5%**, con un avance de proyectos del **45.8%**.
- Las lecciones tienen el mayor avance (85.7%), seguidas por ejercicios (52.1%) y proyectos (45.8%).
- Todos los proyectos completados están aprobados (11/11). Ningún proyecto completado está pendiente de revisión.
---

## Skill 5 — 🔥 Prioridad de proyectos pendientes

### Prompt original

> _"Crea una skill que identifique cuáles de mis proyectos pendientes son los más urgentes — según fecha límite o según el orden en el que aparecen en mi cohorte activa."_

### Endpoint utilizado

No se hizo una llamada nueva. Se reutilizaron los datos de la consulta previa:

```
GET https://breathecode.herokuapp.com/v1/assignment/task/?user=21764&limit=50
```

### Criterio de urgencia

No existe un campo "fecha límite" (due_date) en la API. Se usaron dos criterios en orden:

1. **Posición en el plan de estudios:** la cohorte `latam-aie-pt-4` define un orden de micro-cohortes: `1607, 1608, 1692, 1609, 1626, 1693, 1610, 1666, ...`. Los proyectos de micro-cohortes que aparecen **primero en el orden** son más urgentes (porque están diseñados para hacerse antes en la carrera).
2. **Antigüedad:** entre proyectos de la misma micro-cohorte, los que llevan más días abiertos (`opened_at`) tienen mayor urgencia.

### ✅ Resultado

**Avance por módulo (orden curricular):**

| Módulo | Proyectos | Avance |
|---|---|---|
| ✅ Web UI Fundamentals | 3/3 | 100% |
| ✅ Command Line & Git | 2/2 | 100% |
| ✅ Personal assistants (OpenClaw básico) | 2/2 | 100% |
| 🟡 Coding fundamentals with TypeScript | 1/4 | **25%** |
| 🔴 Frontend development | 0/5 | **0%** |
| 🔴 Advanced OpenClaw | 0/2 | **0%** |
| 🔴 Working with AI coding agents | 0/2 | **0%** |
| 🔴 Backend, Auth, Docker, DB, Data Pipelines... | — | No iniciados |

**Avance total en la carrera:** 8/20 proyectos completados (**40%**)

**🔝 Top 3 más urgentes:**

| # | Proyecto | Módulo | Abierto | Días |
|---|---|---|---|---|
| 🔥 1 | **Milestone 2 — Building Scripts to Automate Tasks** | Coding Typescript | 2026-08-22 | **32 días** |
| 🔥 2 | **Data modeling and class diagrams** | Coding Typescript | 2026-09-07 | 15 días |
| 🔥 3 | **Music playlist player modeling** | Coding Typescript | 2026-09-07 | 15 días |

Los 3 proyectos más urgentes están todos en el módulo **Coding fundamentals with TypeScript**, que es el siguiente módulo incompleto en la secuencia curricular (tiene 1/4 proyectos, 25%). Completar este módulo destraba el avance hacia Frontend, Advanced OpenClaw, y el resto de la carrera.

---

## Skill 6 — ⏳ Tiempo restante en la cohorte

### Prompt original

> _"Crea una skill que calcule cuánto tiempo me queda antes de que termine oficialmente mi cohorte activa — busca la fecha de finalización en los datos de la API y compárala con hoy."_

### Endpoint utilizado

```
GET https://breathecode.herokuapp.com/v1/admissions/user/me
Header: Authorization: Token <FOURGEEKS_TOKEN>
```

### Campo usado

De la respuesta, dentro del objeto `cohorts[]`, para la cohorte `latam-aie-pt-4` (id=1767):

| Campo | Valor | Descripción |
|---|---|---|
| `cohort.kickoff_date` | `2026-07-20T00:00:00Z` | Fecha de inicio |
| `cohort.ending_date` | `2027-01-20T00:00:00Z` | Fecha de finalización |
| `never_ends` | `false` | Indica que sí tiene fecha de término |

No existe un campo `due_date` o fecha límite por proyecto individual en la API de tareas `/v1/assignment/task/`.

### ✅ Resultado

| Medición | Valor |
|---|---|
| Inicio | 20 jul 2026 |
| Fin | **20 ene 2027** |
| Hoy | 23 sep 2026 |
| **Duración total** | 184 días (26 semanas) |
| **Días transcurridos** | 65 días (35.3%) |
| **Días restantes** | **119 días (17 semanas)** |
| **Tiempo restante** | **64.7%** |

### Ritmo sugerido

| Métrica | Valor |
|---|---|
| Proyectos pendientes | 13 |
| Días disponibles | 119 |
| Proyectos por semana recomendado | **~0.8/semana** |

### Lecciones aprendidas

- La fecha de finalización (`ending_date`) está disponible en el endpoint `/v1/admissions/user/me`, dentro del array `cohorts[].cohort.ending_date`.
- No es necesario hacer una llamada adicional a la API de tareas para obtener esta información.
- La cohorte tiene una duración de 6 meses (julio 2026 → enero 2027). A 23 de septiembre, Tatiana ha consumido ~35% del tiempo pero tiene ~46% de los proyectos completados, lo que sugiere que va ligeramente adelantada en ritmo de proyectos vs. tiempo transcurrido.
- El ritmo de 1 proyecto cada ~9 días es suficiente para terminar los 13 pendientes antes de enero.

---

_Última actualización: 2026-09-23 02:33 UTC_

Actualización: el bloqueo de litellm fue resuelto por el instructor el 16-09-2026. OpenClaw está activo nuevamente. Modelo activo después de la resolución: litellm/online/openai/gpt-5.6-lunaopenclaw