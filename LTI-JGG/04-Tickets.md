# 🎯 Historia: Crear y gestionar vacantes (H1)

**Contexto:**
Parte del **Epic 1 – Gestión de Vacantes y Pipeline**, pilar del sistema LTI.
Permite a un **reclutador autenticado** crear vacantes con su pipeline (etapas, estado, descripción, etc.), estableciendo la base para el resto del flujo de selección.

---

## 🎟️ 1. Feature – Implementar CRUD de Vacantes

**Propósito:**
Permitir crear, editar, publicar y cerrar vacantes en el sistema mediante API REST.

**Detalles Específicos:**

* Entidad: `JobPosting`
* Estados: `draft`, `open`, `on_hold`, `closed`
* Campos mínimos: `Title`, `Description`, `Seniority`, `Status`, `CreatedBy`, `CreatedAt`
* Endpoints:

  * `POST /api/vacancies`
  * `GET /api/vacancies/{id}`
  * `PUT /api/vacancies/{id}`
  * `DELETE /api/vacancies/{id}` (soft delete opcional)
* Integración con autenticación JWT.
* Validaciones de negocio: solo usuarios con rol `recruiter` o `admin` pueden crear o modificar.

**Criterios de Aceptación:**

* [ ] El usuario autenticado puede crear una vacante con todos los campos requeridos.
* [ ] Se valida el estado inicial (`draft` o `open`).
* [ ] Los cambios quedan registrados con `CreatedBy` y `OccurredAt`.
* [ ] Solo usuarios con permisos válidos pueden modificar o publicar.

**Pruebas de Validación:**

* Test funcional de creación y edición.
* Test de autorización con JWT inválido.
* Test de persistencia en BD (`JobPosting`).

**Prioridad:** Alta
**Estimación de Esfuerzo:** 5 puntos
**Responsable:** Equipo Backend
**Etiquetas:** `feature`, `backend`, `vacancies`, `MVP`
**Enlaces:** PRD §5, §9; UserStory H1; Backlog Epic 1
**Notas:** Base para todo el flujo de selección.

---

## 🎟️ 2. Feature – Configuración de Pipeline por Vacante

**Propósito:**
Permitir que el reclutador defina las etapas personalizadas del pipeline asociado a una vacante.

**Detalles Específicos:**

* Entidad: `PipelineStage`
* Atributos: `Name`, `Kind`, `OrderIndex`, `JobPostingId`
* Reglas:

  * Al menos una etapa obligatoria.
  * `OrderIndex` determina visualización y flujo.
  * Tipos predefinidos: `screen`, `phone`, `tech`, `onsite`, `offer`.

**Criterios de Aceptación:**

* [ ] Se pueden crear, reordenar y eliminar etapas del pipeline.
* [ ] Las etapas se guardan asociadas a la vacante.
* [ ] El orden se refleja en la API y UI.
* [ ] No se permite duplicar nombres de etapa en una misma vacante.

**Pruebas de Validación:**

* Test de persistencia de pipeline.
* Test de reordenamiento.
* Test de validación de duplicados.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 3 puntos
**Responsable:** Equipo Backend
**Etiquetas:** `feature`, `backend`, `pipeline`, `MVP`
**Notas:** Requisito previo para el movimiento de candidatos (H2).

---

## 🎟️ 3. Task – Implementar esquema relacional y migración inicial

**Propósito:**
Definir y crear las tablas necesarias en PostgreSQL.

**Detalles Específicos:**

* Tablas: `JobPosting`, `PipelineStage`
* Relaciones:
  * `PipelineStage.JobPostingId → JobPosting.JobPostingId`
* Uso de migraciones con **FluentMigrator**.
* Campos auditables: `CreatedAt`, `CreatedBy`.

**Criterios de Aceptación:**

* [ ] Las tablas y relaciones se crean correctamente.
* [ ] Índices sobre `OrganizationId`, `Status`, `JobPostingId`.
* [ ] Script de rollback funcional.

**Pruebas de Validación:**

* Migración ejecutada correctamente en entorno local y CI.
* Validación de constraints y foreign keys.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 2 puntos
**Responsable:** Equipo Backend / Infraestructura
**Etiquetas:** `task`, `database`, `infrastructure`

---

## 🎟️ 4. Task – Exponer endpoints RESTful para CRUD y pipeline

**Propósito:**
Crear endpoints de la API Minimal (en .NET 9) que integren validación, autenticación y versionado por header.

**Detalles Específicos:**

* Usar convenciones del proyecto (`x-api-version`, `ProblemDetails` para errores).
* Rutas:
  * `/api/vacancies`
  * `/api/vacancies/{id}/pipeline`
* Validación de modelo con `FluentValidation`.
* Logging y tracing con **OpenTelemetry** (`ActivitySource: LTI.Recruitment`).

**Criterios de Aceptación:**

* [ ] Todos los endpoints devuelven `ProblemDetails` en caso de error.
* [ ] Se registran trazas OTel con `Activity` de tipo `JobPosting.Create`.
* [ ] API versionada correctamente.

**Pruebas de Validación:**

* Tests de integración HTTP con status 200/400/401/403.
* Validación de headers y trazas.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 3 puntos
**Responsable:** Equipo Backend
**Etiquetas:** `api`, `backend`, `observability`

---

## 🎟️ 5. Task – Implementar pruebas unitarias e integración inicial

**Propósito:**
Garantizar que la creación y modificación de vacantes es consistente y auditable.

**Detalles Específicos:**

* Tests unitarios para `JobPostingService` y `PipelineService`.
* Tests de integración sobre endpoints HTTP.
* Cobertura mínima: 80%.

**Criterios de Aceptación:**

* [ ] Todas las operaciones CRUD se prueban con entradas válidas e inválidas.
* [ ] Validaciones de negocio cubiertas.
* [ ] Eventos de auditoría (`ApplicationEvent`) verificados.

**Prioridad:** Media
**Estimación de Esfuerzo:** 3 puntos
**Responsable:** Equipo QA / Backend
**Etiquetas:** `testing`, `backend`, `MVP`

---

## 🎟️ 6. Spike – Definir modelo de dominio y patrones de implementación (DDD-light)

**Propósito:**
Investigar la mejor estructura para el dominio `Recruitment` y su integración con Minimal API.

**Detalles Específicos:**

* Evaluar opciones: MediatR, comandos `CreateJobPosting`, `ConfigurePipeline`.
* Determinar agregados principales (`JobPosting`, `PipelineStage`).
* Analizar persistencia con NHibernate o EF Core (según stack final).

**Criterios de Aceptación:**

* [ ] Documento técnico con estructura de namespaces, comandos y repositorios.
* [ ] Validación de compatibilidad con OpenTelemetry y ProblemDetails.
* [ ] Prototipo funcional de creación de vacante.

**Pruebas de Validación:**

* Revisión técnica por arquitecto.
* Demostración funcional de flujo completo.

**Prioridad:** Alta
**Estimación de Esfuerzo:** 5 puntos
**Responsable:** Arquitecto / Equipo Backend
**Etiquetas:** `spike`, `architecture`, `backend`
**Notas:** Precede a la implementación de H1 y H2.

---

## 🎟️ 7. Improvement – Autoasignación de reclutador al crear vacante

**Propósito:**
Asignar automáticamente el `CreatedBy` como reclutador responsable de la vacante.

**Detalles Específicos:**

* Campo `OwnerId` = `UserId` del creador.
* Impacta métricas y filtros de visibilidad.

**Criterios de Aceptación:**

* [ ] La vacante creada queda asociada al usuario autenticado.
* [ ] Puede transferirse posteriormente (solo admin).

**Pruebas de Validación:**

* Test de creación con JWT → `OwnerId` correcto.

**Prioridad:** Media
**Estimación de Esfuerzo:** 2 puntos
**Responsable:** Backend
**Etiquetas:** `enhancement`, `backend`, `ownership`

---

## 🧾 **Resumen Global de Tickets (Historia H1)**

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
