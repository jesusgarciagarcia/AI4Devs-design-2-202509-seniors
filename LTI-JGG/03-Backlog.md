# 📋 Backlog

El backlog resultante:

* Representa un **MVP funcional completo** con dependencias explícitas.
* Mantiene **alineación total con la visión del PRD** (rapidez, colaboración, IA trazable).
* Es **accionable para planificación ágil (Scrum/Kanban)** y estimación iterativa.
* Prioriza **valor de negocio** sobre complejidad técnica, siguiendo **criterios MoSCoW**.

## 🧱 Estructura del Backlog

| ID | Título | Tipo | Prioridad | Dependencias | Estimación (pts) | Estado | Responsable |
| -- | ------ | ---- | --------- | ------------ | ---------------- | ------ | ----------- |

---

## 🧩 **Epic 1 – Gestión de Vacantes y Pipeline**

| ID     | Historia                                   | Tipo        | Prioridad       | Dependencias | Estimación | Estado    | Responsable     |
| ------ | ------------------------------------------ | ----------- | --------------- | ------------ | ---------- | --------- | --------------- |
| **H1** | Crear y gestionar vacantes                 | Feature     | **Must Have**   | —            | 8          | Pendiente | Equipo Backend  |
| **H2** | Mover candidatos entre etapas del pipeline | Feature     | **Must Have**   | H1           | 8          | Pendiente | Equipo Backend  |
| **H6** | Buscar y filtrar vacantes o candidatos     | Enhancement | **Should Have** | H1, H2       | 5          | Pendiente | Equipo Frontend |

### 🔍 Detalles de H1 – Crear y gestionar vacantes

* **Rol:** Reclutador
* **Descripción:** Crear vacantes con título, descripción, seniority y estados (`draft`, `open`, `on_hold`, `closed`), incluyendo definición de etapas (`PipelineStage`).
* **Criterios de aceptación:**

  * Puede crear, editar y publicar vacantes.
  * Puede definir etapas personalizadas.
  * Estado `open` permite asociar candidatos.
* **Valor:** Alto
* **Notas:** Base del sistema y fuente de métricas.

---

### 🔍 Detalles de H2 – Mover candidatos entre etapas

* **Rol:** Reclutador
* **Descripción:** Permite mover candidatos entre etapas, generando eventos `ApplicationEvent`.
* **Criterios de aceptación:**

  * Al mover un candidato, se crea un evento `moved_stage`.
  * Se actualiza el timeline.
  * Se validan reglas de transición (`StageRules`).
* **Valor:** Alto
* **Notas:** Permite trazabilidad y medición de conversión.

---

### 🔍 Detalles de H6 – Buscar y filtrar vacantes o candidatos

* **Rol:** Reclutador
* **Descripción:** Búsqueda y filtrado por vacante, estado o etapa.
* **Criterios de aceptación:**

  * Filtro por estado “open”.
  * Filtro por etapa o texto libre.
  * Resultados ordenados y accesibles en 2 clics.
* **Valor:** Medio
* **Notas:** Mejora la eficiencia operativa.

---

## 🧩 **Epic 2 – Colaboración y Timeline**

| ID     | Historia                                  | Tipo    | Prioridad       | Dependencias | Estimación | Estado    | Responsable     |
| ------ | ----------------------------------------- | ------- | --------------- | ------------ | ---------- | --------- | --------------- |
| **H3** | Visualizar el timeline de una aplicación  | Feature | **Must Have**   | H2           | 8          | Pendiente | Equipo Frontend |
| **H4** | Añadir notas y valoraciones colaborativas | Feature | **Should Have** | H3           | 5          | Pendiente | Equipo Frontend |

### 🔍 Detalles de H3 – Timeline de aplicación

* **Rol:** Manager
* **Descripción:** Combina `ApplicationEvent` y `Note` para mostrar la historia completa del candidato.
* **Criterios de aceptación:**

  * Vista cronológica unificada.
  * Identifica fuente de notas (usuario o IA).
  * Soporte para múltiples autores.
* **Valor:** Alto
* **Notas:** Núcleo visual de colaboración.

---

### 🔍 Detalles de H4 – Notas y valoraciones colaborativas

* **Rol:** Reclutador / Manager
* **Descripción:** Permite comentarios en aplicaciones con trazabilidad.
* **Criterios de aceptación:**

  * Registro con autor y fecha.
  * Etiqueta visible si es generada por IA.
  * Listado cronológico.
* **Valor:** Medio
* **Notas:** Mejora decisiones sin reuniones.

---

## 🧩 **Epic 3 – Inteligencia Asistida (IA Generativa)**

| ID     | Historia                                       | Tipo    | Prioridad       | Dependencias | Estimación | Estado    | Responsable         |
| ------ | ---------------------------------------------- | ------- | --------------- | ------------ | ---------- | --------- | ------------------- |
| **H5** | Generar textos con IA (JD, correos, resúmenes) | Feature | **Should Have** | H4           | 13         | Pendiente | Equipo IA / Backend |

### 🔍 Detalles de H5 – IA generativa

* **Rol:** Reclutador
* **Descripción:** Generar textos mediante IA externa; los resultados se guardan como notas con trazabilidad.
* **Criterios de aceptación:**

  * Petición → generación → revisión → almacenamiento.
  * Etiqueta `source='ai'`.
  * Registro de latencia y errores del servicio externo.
* **Valor:** Medio
* **Notas:** Depende de integración externa, no bloqueante para MVP.

---

## 🧩 **Epic 4 – Analítica y Métricas**

| ID     | Historia                         | Tipo    | Prioridad     | Dependencias | Estimación | Estado    | Responsable           |
| ------ | -------------------------------- | ------- | ------------- | ------------ | ---------- | --------- | --------------------- |
| **H7** | Métricas de conversión y tiempos | Feature | **Must Have** | H2           | 5          | Pendiente | Equipo Backend / Data |

### 🔍 Detalles de H7 – Métricas del proceso

* **Rol:** Organización
* **Descripción:** Cálculo de métricas clave de rendimiento (`time-to-hire`, `conversion rate`).
* **Criterios de aceptación:**

  * Visualización de tiempos por etapa.
  * Exportación de datos a CSV.
  * Identificación de cuellos de botella.
* **Valor:** Alto
* **Notas:** Permite decisiones de optimización.

---

## ⚙️ **Resumen General del Backlog**

| ID | Epic                | Prioridad   | Estimación | Dependencias | Valor |
| -- | ------------------- | ----------- | ---------- | ------------ | ----- |
| H1 | Gestión de Vacantes | Must Have   | 8          | —            | Alto  |
| H2 | Pipeline            | Must Have   | 8          | H1           | Alto  |
| H3 | Timeline            | Must Have   | 8          | H2           | Alto  |
| H4 | Colaboración        | Should Have | 5          | H3           | Medio |
| H5 | IA Generativa       | Should Have | 13         | H4           | Medio |
| H6 | Búsqueda / Filtrado | Should Have | 5          | H1, H2       | Medio |
| H7 | Métricas            | Must Have   | 5          | H2           | Alto  |

---

## 🧩 **Backlog Visual (ordenado por prioridad y dependencia)**

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

**📦 Total de Historias:** 7
**Total estimado:** 52 puntos
