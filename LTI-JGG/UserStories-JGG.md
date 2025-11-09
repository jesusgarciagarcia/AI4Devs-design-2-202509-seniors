# 📘 User Stories, Backlog y Tickets de Trabajo – LTI (Lean Talent Intelligence)

**Autor:** Jesús García García
**Fecha:** Noviembre 2025
**Herramienta utilizada:** ChatGPT 5
**Contexto:** Ejercicio del módulo AI4Devs - Design 2 - Product Management con IA

---

## 📋 Índice

1. [Introducción](#-introducción)
2. [Historias de Usuario](#-historias-de-usuario)
3. [Backlog Priorizado](#-backlog-priorizado)
4. [Tickets de Trabajo - Historia H1](#️-tickets-de-trabajo---historia-h1)

---

## 🎯 Introducción

Este documento consolida el trabajo realizado para transformar el **Product Requirements Document (PRD)** del sistema LTI en un conjunto de **historias de usuario**, un **backlog priorizado** y los **tickets técnicos** necesarios para iniciar el desarrollo.

El proceso siguió una metodología estructurada utilizando técnicas avanzadas de prompting:

- **Meta-prompting** para definir el contexto y rol del asistente IA
- **Prompts especializados** por cada entregable (User Stories, Backlog, Tickets)
- **Iteración y refinamiento** basado en los principios ágiles y mejores prácticas

### 📚 Documentos Base Utilizados

- `00-LTI-JGG.md` - Documento original de especificaciones
- `01-PRD.md` - Product Requirements Document generado
- Contexto adicional de arquitectura y modelo de datos

---

## 🧠 Historias de Usuario

A continuación se presentan las **7 historias de usuario** identificadas y desarrolladas a partir del análisis del PRD.

### 🔍 1. Identificación de Necesidades

El análisis reveló **7 necesidades principales** de los usuarios:

| Tipo de usuario      | Necesidad identificada                        | Fuente         |
| -------------------- | --------------------------------------------- | -------------- |
| Reclutador (HR)      | Crear y gestionar vacantes y pipelines        | PRD §3, §4, §5 |
| Reclutador (HR)      | Mover candidatos y mantener trazabilidad      | PRD §5, §9     |
| Manager técnico      | Consultar un timeline de aplicación           | PRD §3, §5     |
| Reclutador / Manager | Colaborar mediante notas y valoraciones       | PRD §3, §5     |
| Reclutador           | Generar textos (JD, emails, resúmenes) con IA | PRD §5, §9     |
| HR / Manager         | Buscar y filtrar rápidamente candidatos       | PRD §5         |
| Organización         | Medir métricas de conversión y tiempo         | PRD §5, §8     |

---

### ✍️ 2. Historias de Usuario Detalladas

#### **Historia 1: Crear y gestionar vacantes**

**Como** reclutador,
**quiero** crear y configurar vacantes con etapas personalizadas,
**para que** pueda organizar el proceso de selección de forma clara y repetible.

**Descripción:**
Permite definir vacantes, su descripción, seniority y etapas del pipeline. Facilita una estructura estándar de reclutamiento con control de estados (`draft`, `open`, `on_hold`, `closed`).

**Criterios de aceptación:**

- Dado que soy un reclutador autenticado, cuando creo una vacante, entonces puedo definir título, descripción y estado inicial.
- Dado que configuro etapas, cuando las guardo, entonces se ordenan y vinculan al pipeline.
- Dado que publico una vacante, cuando su estado es "open", entonces puedo asociar candidatos.

**Notas adicionales:**
Funcionalidad base del sistema. Relacionada con gestión de pipeline (H2).

**Historias relacionadas:** H2, H6

---

#### **Historia 2: Mover candidatos entre etapas del pipeline**

**Como** reclutador,
**quiero** mover candidatos entre etapas del pipeline,
**para que** el flujo de selección sea rápido y trazable.

**Descripción:**
Cada movimiento genera un `ApplicationEvent` con `fromStage` y `toStage`, reflejado automáticamente en el timeline.

**Criterios de aceptación:**

- Dado que tengo una aplicación activa, cuando cambio su etapa, entonces se crea un evento `moved_stage`.
- Dado que visualizo la aplicación, cuando abro el timeline, entonces veo la transición registrada.
- Dado que hay reglas de validación, cuando intento una transición no válida, entonces recibo un error descriptivo.

**Notas adicionales:**
Núcleo funcional para métricas y auditoría.

**Historias relacionadas:** H1, H3, H7

---

#### **Historia 3: Visualizar el timeline de una aplicación**

**Como** manager,
**quiero** ver un timeline con todos los eventos y notas,
**para que** pueda comprender el historial del candidato antes de decidir.

**Descripción:**
Integra eventos (`ApplicationEvent`) y notas (`Note`) en una vista unificada ordenada cronológicamente.

**Criterios de aceptación:**

- Dado que accedo a una aplicación, cuando la abro, entonces visualizo el historial completo (eventos + notas).
- Dado que un evento proviene de IA, cuando aparece en el timeline, entonces se identifica claramente como "IA".
- Dado que varios usuarios participan, cuando añaden notas, entonces todas se muestran cronológicamente.

**Notas adicionales:**
Elemento clave de colaboración y auditoría.

**Historias relacionadas:** H2, H4

---

#### **Historia 4: Añadir notas y valoraciones colaborativas**

**Como** usuario (reclutador o manager),
**quiero** añadir notas o valoraciones a las aplicaciones,
**para que** las decisiones y comentarios queden centralizados.

**Descripción:**
Las notas pueden tener fuente `user` o `ai`. Permiten documentar evaluaciones y comentarios visibles en el timeline.

**Criterios de aceptación:**

- Dado que soy usuario autenticado, cuando agrego una nota, entonces se registra con mi identidad y fecha.
- Dado que la nota proviene de IA, cuando se muestra, entonces se etiqueta como tal.
- Dado que varios usuarios añaden notas, cuando reviso la aplicación, entonces veo todas ordenadas.

**Notas adicionales:**
Fomenta colaboración asíncrona. Base de decisiones en H3 y H5.

**Historias relacionadas:** H3, H5

---

#### **Historia 5: Generar textos con IA (JD, correos, resúmenes)**

**Como** reclutador,
**quiero** generar descripciones o correos mediante IA,
**para que** reduzca tiempo operativo y mantenga coherencia de comunicación.

**Descripción:**
Invoca un servicio externo de IA para proponer textos que luego se revisan y guardan como notas con `source='ai'`.

**Criterios de aceptación:**

- Dado que solicito generar un texto, cuando se envía la petición, entonces recibo una propuesta.
- Dado que apruebo el texto, cuando lo guardo, entonces se almacena con trazabilidad.
- Dado que consulto el timeline, cuando veo la nota, entonces se identifica como generada por IA.

**Notas adicionales:**
Requiere integración con servicio IA externo. Depende de H4 (notas).

**Historias relacionadas:** H4

---

#### **Historia 6: Buscar y filtrar vacantes o candidatos**

**Como** reclutador,
**quiero** buscar y filtrar por vacante, etapa o estado,
**para que** pueda priorizar acciones diarias sin perder tiempo.

**Descripción:**
Permite localizar aplicaciones y vacantes de forma rápida mediante filtros combinados y búsqueda textual.

**Criterios de aceptación:**

- Dado que hay varias vacantes, cuando filtro por estado "open", entonces solo aparecen las abiertas.
- Dado que busco por nombre, cuando escribo en el buscador, entonces obtengo coincidencias relevantes.
- Dado que selecciono una etapa, cuando aplico el filtro, entonces veo solo las aplicaciones en esa fase.

**Notas adicionales:**
Mejora eficiencia operativa.

**Historias relacionadas:** H1, H2

---

#### **Historia 7: Medir métricas de conversión y tiempos**

**Como** organización,
**quiero** obtener métricas de conversión y tiempos por etapa,
**para que** pueda mejorar la eficiencia del proceso de contratación.

**Descripción:**
Calcula métricas de "time-to-hire" y "conversion rate" a partir de los eventos registrados en `ApplicationEvent`.

**Criterios de aceptación:**

- Dado que existen eventos, cuando consulto métricas, entonces obtengo promedios y conversiones.
- Dado que hay múltiples aplicaciones, cuando genero un reporte, entonces se exporta en CSV.
- Dado que reviso los datos, cuando comparo etapas, entonces puedo detectar cuellos de botella.

**Notas adicionales:**
Permite decisiones basadas en datos. Depende de H2 (pipeline).

**Historias relacionadas:** H2

---

### 🤖 3. Evaluación de Historias

| Historia | Claridad | Coherencia | Consistencia | Lagunas detectadas                           |
| -------- | -------- | ----------- | ------------ | -------------------------------------------- |
| H1       | ✅ Alta  | ✅ Alta     | ✅           | Ninguna                                      |
| H2       | ✅ Alta  | ✅ Alta     | ✅           | Ninguna                                      |
| H3       | ✅ Alta  | ✅ Alta     | ✅           | Ninguna                                      |
| H4       | ✅ Alta  | ✅ Media    | ⚠️           | Posible redundancia con H3 (timeline)        |
| H5       | ✅ Alta  | ✅ Alta     | ✅           | Requiere integración IA                      |
| H6       | ✅ Alta  | ✅ Alta     | ✅           | Ninguna                                      |
| H7       | ✅ Alta  | ✅ Alta     | ✅           | Ninguna                                      |

---

### 🧾 4. Recomendaciones

**Recomendaciones globales:**

1. Evitar solapamiento entre **H3 (timeline)** y **H4 (notas)** — se sugiere unificar en epic "Colaboración y Timeline".
2. Incluir una historia futura sobre **gestión de usuarios y roles** (detectada en PRD §2, faltante en MVP).
3. Agregar validación de **seguridad y permisos** en criterios de aceptación (autenticación JWT mencionada en PRD §7).

---

## 📋 Backlog Priorizado

### ⚖️ Priorización (Modelo MoSCoW + Análisis de Dependencias)

| Historia       | Valor | Esfuerzo | Dependencias     | Prioridad       | Justificación                               |
| -------------- | ----- | -------- | ---------------- | --------------- | ------------------------------------------- |
| H1 – Vacantes  | Alto  | Medio    | Base del sistema | **Must Have**   | Punto de partida del flujo de reclutamiento |
| H2 – Pipeline  | Alto  | Medio    | H1               | **Must Have**   | Núcleo del proceso de selección             |
| H3 – Timeline  | Alto  | Medio    | H2, H4           | **Must Have**   | Transparencia y colaboración                |
| H4 – Notas     | Medio | Bajo     | H3               | **Should Have** | Complementa timeline y decisiones           |
| H5 – IA textos | Medio | Alto     | H4               | **Should Have** | Valor añadido, dependiente de IA externa    |
| H6 – Búsqueda  | Medio | Bajo     | H1, H2           | **Should Have** | Mejora eficiencia pero no crítica           |
| H7 – Métricas  | Alto  | Bajo     | H2               | **Must Have**   | Impacto directo en decisiones y ROI         |

---

### 🧩 **Epic 1 – Gestión de Vacantes y Pipeline**

| ID     | Historia                                   | Tipo        | Prioridad       | Dependencias | Estimación | Estado    | Responsable     |
| ------ | ------------------------------------------ | ----------- | --------------- | ------------ | ---------- | --------- | --------------- |
| **H1** | Crear y gestionar vacantes                 | Feature     | **Must Have**   | —            | 8          | Pendiente | Equipo Backend  |
| **H2** | Mover candidatos entre etapas del pipeline | Feature     | **Must Have**   | H1           | 8          | Pendiente | Equipo Backend  |
| **H6** | Buscar y filtrar vacantes o candidatos     | Enhancement | **Should Have** | H1, H2       | 5          | Pendiente | Equipo Frontend |

#### 🔍 Detalles de H1 – Crear y gestionar vacantes

- **Rol:** Reclutador
- **Descripción:** Crear vacantes con título, descripción, seniority y estados (`draft`, `open`, `on_hold`, `closed`), incluyendo definición de etapas (`PipelineStage`).
- **Criterios de aceptación:**
  - Puede crear, editar y publicar vacantes.
  - Puede definir etapas personalizadas.
  - Estado `open` permite asociar candidatos.
- **Valor:** Alto
- **Notas:** Base del sistema y fuente de métricas.

---

### 🧩 **Epic 2 – Colaboración y Timeline**

| ID     | Historia                                  | Tipo    | Prioridad       | Dependencias | Estimación | Estado    | Responsable     |
| ------ | ----------------------------------------- | ------- | --------------- | ------------ | ---------- | --------- | --------------- |
| **H3** | Visualizar el timeline de una aplicación  | Feature | **Must Have**   | H2           | 8          | Pendiente | Equipo Frontend |
| **H4** | Añadir notas y valoraciones colaborativas | Feature | **Should Have** | H3           | 5          | Pendiente | Equipo Frontend |

---

### 🧩 **Epic 3 – Inteligencia Asistida (IA Generativa)**

| ID     | Historia                                       | Tipo    | Prioridad       | Dependencias | Estimación | Estado    | Responsable         |
| ------ | ---------------------------------------------- | ------- | --------------- | ------------ | ---------- | --------- | ------------------- |
| **H5** | Generar textos con IA (JD, correos, resúmenes) | Feature | **Should Have** | H4           | 13         | Pendiente | Equipo IA / Backend |

---

### 🧩 **Epic 4 – Analítica y Métricas**

| ID     | Historia                         | Tipo    | Prioridad     | Dependencias | Estimación | Estado    | Responsable           |
| ------ | -------------------------------- | ------- | ------------- | ------------ | ---------- | --------- | --------------------- |
| **H7** | Métricas de conversión y tiempos | Feature | **Must Have** | H2           | 5          | Pendiente | Equipo Backend / Data |

---

### ⚙️ **Resumen General del Backlog**

| ID | Epic                | Prioridad   | Estimación | Dependencias | Valor |
| -- | ------------------- | ----------- | ---------- | ------------ | ----- |
| H1 | Gestión de Vacantes | Must Have   | 8          | —            | Alto  |
| H2 | Pipeline            | Must Have   | 8          | H1           | Alto  |
| H3 | Timeline            | Must Have   | 8          | H2           | Alto  |
| H4 | Colaboración        | Should Have | 5          | H3           | Medio |
| H5 | IA Generativa       | Should Have | 13         | H4           | Medio |
| H6 | Búsqueda / Filtrado | Should Have | 5          | H1, H2       | Medio |
| H7 | Métricas            | Must Have   | 5          | H2           | Alto  |

**📦 Total de Historias:** 7
**Total estimado:** 52 puntos

---

### 🧩 **Backlog Visual (ordenado por prioridad y dependencia)**

```text
1️⃣ H1 – Crear y gestionar vacantes
2️⃣ H2 – Mover candidatos entre etapas
3️⃣ H3 – Visualizar timeline
4️⃣ H7 – Métricas del proceso
5️⃣ H4 – Notas colaborativas
6️⃣ H5 – IA generativa
7️⃣ H6 – Búsqueda y filtrado
```

---

## 🎟️ Tickets de Trabajo - Historia H1

### 🎯 Historia: Crear y gestionar vacantes (H1)

**Contexto:**
Parte del **Epic 1 – Gestión de Vacantes y Pipeline**, pilar del sistema LTI.
Permite a un **reclutador autenticado** crear vacantes con su pipeline (etapas, estado, descripción, etc.), estableciendo la base para el resto del flujo de selección.

---

### 🎟️ 1. Feature – Implementar CRUD de Vacantes

**Propósito:**
Permitir crear, editar, publicar y cerrar vacantes en el sistema mediante API REST.

**Detalles Específicos:**

- Entidad: `JobPosting`
- Estados: `draft`, `open`, `on_hold`, `closed`
- Campos mínimos: `Title`, `Description`, `Seniority`, `Status`, `CreatedBy`, `CreatedAt`
- Endpoints:
  - `POST /api/vacancies`
  - `GET /api/vacancies/{id}`
  - `PUT /api/vacancies/{id}`
  - `DELETE /api/vacancies/{id}` (soft delete opcional)
- Integración con autenticación JWT.
- Validaciones de negocio: solo usuarios con rol `recruiter` o `admin` pueden crear o modificar.

**Criterios de Aceptación:**

- [ ] El usuario autenticado puede crear una vacante con todos los campos requeridos.
- [ ] Se valida el estado inicial (`draft` o `open`).
- [ ] Los cambios quedan registrados con `CreatedBy` y `OccurredAt`.
- [ ] Solo usuarios con permisos válidos pueden modificar o publicar.

**Pruebas de Validación:**

- Test funcional de creación y edición.
- Test de autorización con JWT inválido.
- Test de persistencia en BD (`JobPosting`).

**Prioridad:** Alta
**Estimación de Esfuerzo:** 5 puntos (Fibonacci)
**Responsable:** Equipo Backend
**Etiquetas:** `feature`, `backend`, `vacancies`, `MVP`
**Enlaces:** PRD §5, §9; UserStory H1; Backlog Epic 1
**Notas:** Base para todo el flujo de selección.

---

### 🎟️ 2. Feature – Configuración de Pipeline por Vacante

**Propósito:**
Permitir que el reclutador defina las etapas personalizadas del pipeline asociado a una vacante.

**Detalles Específicos:**

- Entidad: `PipelineStage`
- Atributos: `Name`, `Kind`, `OrderIndex`, `JobPostingId`
- Reglas:
  - Al menos una etapa obligatoria.
  - `OrderIndex` determina visualización y flujo.
  - Tipos predefinidos: `screen`, `phone`, `tech`, `onsite`, `offer`.

**Criterios de Aceptación:**

- [ ] Se pueden crear, reordenar y eliminar etapas del pipeline.
- [ ] Las etapas se guardan asociadas a la vacante.
- [ ] El orden se refleja en la API y UI.
- [ ] No se permite duplicar nombres de etapa en una misma vacante.

**Pruebas de Validación:**

- Test de persistencia de pipeline.
- Test de reordenamiento.
- Test de validación de duplicados.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 3 puntos (Fibonacci)
**Responsable:** Equipo Backend
**Etiquetas:** `feature`, `backend`, `pipeline`, `MVP`
**Notas:** Requisito previo para el movimiento de candidatos (H2).

---

### 🎟️ 3. Task – Implementar esquema relacional y migración inicial

**Propósito:**
Definir y crear las tablas necesarias en PostgreSQL.

**Detalles Específicos:**

- Tablas: `JobPosting`, `PipelineStage`
- Relaciones:
  - `PipelineStage.JobPostingId → JobPosting.JobPostingId`
- Uso de migraciones con **FluentMigrator**.
- Campos auditables: `CreatedAt`, `CreatedBy`.

**Criterios de Aceptación:**

- [ ] Las tablas y relaciones se crean correctamente.
- [ ] Índices sobre `OrganizationId`, `Status`, `JobPostingId`.
- [ ] Script de rollback funcional.

**Pruebas de Validación:**

- Migración ejecutada correctamente en entorno local y CI.
- Validación de constraints y foreign keys.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 2 puntos (Fibonacci)
**Responsable:** Equipo Backend / Infraestructura
**Etiquetas:** `task`, `database`, `infrastructure`

---

### 🎟️ 4. Task – Exponer endpoints RESTful para CRUD y pipeline

**Propósito:**
Crear endpoints de la API Minimal (en .NET 9) que integren validación, autenticación y versionado por header.

**Detalles Específicos:**

- Usar convenciones del proyecto (`x-api-version`, `ProblemDetails` para errores).
- Rutas:
  - `/api/vacancies`
  - `/api/vacancies/{id}/pipeline`
- Validación de modelo con `FluentValidation`.
- Logging y tracing con **OpenTelemetry** (`ActivitySource: LTI.Recruitment`).

**Criterios de Aceptación:**

- [ ] Todos los endpoints devuelven `ProblemDetails` en caso de error.
- [ ] Se registran trazas OTel con `Activity` de tipo `JobPosting.Create`.
- [ ] API versionada correctamente.

**Pruebas de Validación:**

- Tests de integración HTTP con status 200/400/401/403.
- Validación de headers y trazas.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 3 puntos (Fibonacci)
**Responsable:** Equipo Backend
**Etiquetas:** `api`, `backend`, `observability`

---

### 🎟️ 5. Task – Implementar pruebas unitarias e integración inicial

**Propósito:**
Garantizar que la creación y modificación de vacantes es consistente y auditable.

**Detalles Específicos:**

- Tests unitarios para `JobPostingService` y `PipelineService`.
- Tests de integración sobre endpoints HTTP.
- Cobertura mínima: 80%.

**Criterios de Aceptación:**

- [ ] Todas las operaciones CRUD se prueban con entradas válidas e inválidas.
- [ ] Validaciones de negocio cubiertas.
- [ ] Eventos de auditoría (`ApplicationEvent`) verificados.

**Prioridad:** Media
**Estimación de Esfuerzo:** 3 puntos (Fibonacci)
**Responsable:** Equipo QA / Backend
**Etiquetas:** `testing`, `backend`, `MVP`

---

### 🎟️ 6. Spike – Definir modelo de dominio y patrones de implementación (DDD-light)

**Propósito:**
Investigar la mejor estructura para el dominio `Recruitment` y su integración con Minimal API.

**Detalles Específicos:**

- Evaluar opciones: MediatR, comandos `CreateJobPosting`, `ConfigurePipeline`.
- Determinar agregados principales (`JobPosting`, `PipelineStage`).
- Analizar persistencia con NHibernate o EF Core (según stack final).

**Criterios de Aceptación:**

- [ ] Documento técnico con estructura de namespaces, comandos y repositorios.
- [ ] Validación de compatibilidad con OpenTelemetry y ProblemDetails.
- [ ] Prototipo funcional de creación de vacante.

**Pruebas de Validación:**

- Revisión técnica por arquitecto.
- Demostración funcional de flujo completo.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 5 puntos (Fibonacci)
**Responsable:** Arquitecto / Equipo Backend
**Etiquetas:** `spike`, `architecture`, `backend`
**Notas:** Precede a la implementación de H1 y H2.

---

### 🎟️ 7. Improvement – Autoasignación de reclutador al crear vacante

**Propósito:**
Asignar automáticamente el `CreatedBy` como reclutador responsable de la vacante.

**Detalles Específicos:**

- Campo `OwnerId` = `UserId` del creador.
- Impacta métricas y filtros de visibilidad.

**Criterios de Aceptación:**

- [ ] La vacante creada queda asociada al usuario autenticado.
- [ ] Puede transferirse posteriormente (solo admin).

**Pruebas de Validación:**

- Test de creación con JWT → `OwnerId` correcto.

**Prioridad:** Media
**Estimación de Esfuerzo:** 2 puntos (Fibonacci)
**Responsable:** Backend
**Etiquetas:** `enhancement`, `backend`, `ownership`

---

### 🧾 **Resumen Global de Tickets (Historia H1)**

| ID | Tipo        | Título                                | Puntos | Prioridad | Responsable   |
| -- | ----------- | ------------------------------------- | ------ | --------- | ------------- |
| 1  | Feature     | CRUD de Vacantes                      | 5      | Alta      | Backend       |
| 2  | Feature     | Configuración de Pipeline por Vacante | 3      | Alta      | Backend       |
| 3  | Task        | Migraciones DB                        | 2      | Alta      | Backend/Infra |
| 4  | Task        | Endpoints REST + OTel + Auth          | 3      | Alta      | Backend       |
| 5  | Task        | Tests Unitarios/Integración           | 3      | Media     | QA/Backend    |
| 6  | Spike       | Definición de modelo DDD-light        | 5      | Alta      | Arquitecto    |
| 7  | Improvement | Autoasignación de reclutador          | 2      | Media     | Backend       |

**Total estimado:** 23 puntos
**Epic asociado:** Epic 1 – Gestión de Vacantes y Pipeline
**Dependencias:** Ninguna previa
**Siguientes:** H2 (Pipeline de candidatos)
