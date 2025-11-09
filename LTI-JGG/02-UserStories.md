# 🧠 Historias de Usuario – LTI (Lean Talent Intelligence)

## 🔍 1. Identificación de Necesidades

El análisis de ambos documentos revela **7 necesidades principales** de los usuarios:

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

## ✍️ 2. Historias de Usuario

### **Historia 1: Crear y gestionar vacantes**

**Como** reclutador,
**quiero** crear y configurar vacantes con etapas personalizadas,
**para que** pueda organizar el proceso de selección de forma clara y repetible.

**Descripción:**
Permite definir vacantes, su descripción, seniority y etapas del pipeline. Facilita una estructura estándar de reclutamiento con control de estados (`draft`, `open`, `on_hold`, `closed`).

**Criterios de aceptación:**

* Dado que soy un reclutador autenticado, cuando creo una vacante, entonces puedo definir título, descripción y estado inicial.
* Dado que configuro etapas, cuando las guardo, entonces se ordenan y vinculan al pipeline.
* Dado que publico una vacante, cuando su estado es “open”, entonces puedo asociar candidatos.

**Notas adicionales:**
Funcionalidad base del sistema. Relacionada con gestión de pipeline (H2).

---

### **Historia 2: Mover candidatos entre etapas del pipeline**

**Como** reclutador,
**quiero** mover candidatos entre etapas del pipeline,
**para que** el flujo de selección sea rápido y trazable.

**Descripción:**
Cada movimiento genera un `ApplicationEvent` con `fromStage` y `toStage`, reflejado automáticamente en el timeline.

**Criterios de aceptación:**

* Dado que tengo una aplicación activa, cuando cambio su etapa, entonces se crea un evento `moved_stage`.
* Dado que visualizo la aplicación, cuando abro el timeline, entonces veo la transición registrada.
* Dado que hay reglas de validación, cuando intento una transición no válida, entonces recibo un error descriptivo.

**Notas adicionales:**
Núcleo funcional para métricas y auditoría.
Relacionado con H3 (timeline) y H7 (métricas).

---

### **Historia 3: Visualizar el timeline de una aplicación**

**Como** manager,
**quiero** ver un timeline con todos los eventos y notas,
**para que** pueda comprender el historial del candidato antes de decidir.

**Descripción:**
Integra eventos (`ApplicationEvent`) y notas (`Note`) en una vista unificada ordenada cronológicamente.

**Criterios de aceptación:**

* Dado que accedo a una aplicación, cuando la abro, entonces visualizo el historial completo (eventos + notas).
* Dado que un evento proviene de IA, cuando aparece en el timeline, entonces se identifica claramente como “IA”.
* Dado que varios usuarios participan, cuando añaden notas, entonces todas se muestran cronológicamente.

**Notas adicionales:**
Elemento clave de colaboración y auditoría.
Depende de H2 y H4.

---

### **Historia 4: Añadir notas y valoraciones colaborativas**

**Como** usuario (reclutador o manager),
**quiero** añadir notas o valoraciones a las aplicaciones,
**para que** las decisiones y comentarios queden centralizados.

**Descripción:**
Las notas pueden tener fuente `user` o `ai`. Permiten documentar evaluaciones y comentarios visibles en el timeline.

**Criterios de aceptación:**

* Dado que soy usuario autenticado, cuando agrego una nota, entonces se registra con mi identidad y fecha.
* Dado que la nota proviene de IA, cuando se muestra, entonces se etiqueta como tal.
* Dado que varios usuarios añaden notas, cuando reviso la aplicación, entonces veo todas ordenadas.

**Notas adicionales:**
Fomenta colaboración asíncrona.
Base de decisiones en H3 y H5.

---

### **Historia 5: Generar textos con IA (JD, correos, resúmenes)**

**Como** reclutador,
**quiero** generar descripciones o correos mediante IA,
**para que** reduzca tiempo operativo y mantenga coherencia de comunicación.

**Descripción:**
Invoca un servicio externo de IA para proponer textos que luego se revisan y guardan como notas con `source='ai'`.

**Criterios de aceptación:**

* Dado que solicito generar un texto, cuando se envía la petición, entonces recibo una propuesta.
* Dado que apruebo el texto, cuando lo guardo, entonces se almacena con trazabilidad.
* Dado que consulto el timeline, cuando veo la nota, entonces se identifica como generada por IA.

**Notas adicionales:**
Requiere integración con servicio IA externo.
Depende de H4 (notas).

---

### **Historia 6: Buscar y filtrar vacantes o candidatos**

**Como** reclutador,
**quiero** buscar y filtrar por vacante, etapa o estado,
**para que** pueda priorizar acciones diarias sin perder tiempo.

**Descripción:**
Permite localizar aplicaciones y vacantes de forma rápida mediante filtros combinados y búsqueda textual.

**Criterios de aceptación:**

* Dado que hay varias vacantes, cuando filtro por estado “open”, entonces solo aparecen las abiertas.
* Dado que busco por nombre, cuando escribo en el buscador, entonces obtengo coincidencias relevantes.
* Dado que selecciono una etapa, cuando aplico el filtro, entonces veo solo las aplicaciones en esa fase.

**Notas adicionales:**
Mejora eficiencia operativa.
Depende de H1 (vacantes) y H2 (pipeline).

---

### **Historia 7: Medir métricas de conversión y tiempos**

**Como** organización,
**quiero** obtener métricas de conversión y tiempos por etapa,
**para que** pueda mejorar la eficiencia del proceso de contratación.

**Descripción:**
Calcula métricas de “time-to-hire” y “conversion rate” a partir de los eventos registrados en `ApplicationEvent`.

**Criterios de aceptación:**

* Dado que existen eventos, cuando consulto métricas, entonces obtengo promedios y conversiones.
* Dado que hay múltiples aplicaciones, cuando genero un reporte, entonces se exporta en CSV.
* Dado que reviso los datos, cuando comparo etapas, entonces puedo detectar cuellos de botella.

**Notas adicionales:**
Permite decisiones basadas en datos.
Depende de H2 (pipeline).

---

## 🤖 3. Evaluación de Historias

| Historia | Claridad | Coherencia | Consistencia | Lagunas detectadas |
|-----------|-----------|-------------|----------------|----|
| H1 | ✅ Alta | ✅ Alta | ✅ Sin conflictos | Ninguna |
| H2 | ✅ Alta | ✅ Alta | ✅ | Ninguna |
| H3 | ✅ Alta | ✅ Alta | ✅ | Ninguna |
| H4 | ✅ Alta | ✅ Media | ⚠️ Posible redundancia con H3 (timeline) ||
| H5 | ✅ Alta | ✅ Alta | ✅ | Requiere integración IA |
| H6 | ✅ Alta | ✅ Alta | ✅ | Ninguna |
| H7 | ✅ Alta | ✅ Alta | ✅ | Ninguna |

> 🔍 *Evaluación por modelo ML heurístico de consistencia narrativa y dependencia lógica.*

---

## 🧾 4. Revisión y Recomendaciones

**Recomendaciones globales:**

1. Evitar solapamiento entre **H3 (timeline)** y **H4 (notas)** — se sugiere unificar en epic “Colaboración y Timeline”.
2. Incluir una historia futura sobre **gestión de usuarios y roles** (detectada en PRD §2, faltante en MVP).
3. Agregar validación de **seguridad y permisos** en criterios de aceptación (autenticación JWT mencionada en PRD §7).

---

## ⚖️ 5. Priorización (Modelo MoSCoW + Análisis de Dependencias)

| Historia       | Valor | Esfuerzo | Dependencias     | Prioridad       | Justificación                               |
| -------------- | ----- | -------- | ---------------- | --------------- | ------------------------------------------- |
| H1 – Vacantes  | Alto  | Medio    | Base del sistema | **Must Have**   | Punto de partida del flujo de reclutamiento |
| H2 – Pipeline  | Alto  | Medio    | H1               | **Must Have**   | Núcleo del proceso de selección             |
| H3 – Timeline  | Alto  | Medio    | H2, H4           | **Must Have**   | Transparencia y colaboración                |
| H4 – Notas     | Medio | Bajo     | H3               | **Should Have** | Complementa timeline y decisiones           |
| H5 – IA textos | Medio | Alto     | H4               | **Should Have** | Valor añadido, dependiente de IA externa    |
| H6 – Búsqueda  | Medio | Bajo     | H1, H2           | **Should Have** | Mejora eficiencia pero no crítica           |
| H7 – Métricas  | Alto  | Bajo     | H2               | **Must Have**   | Impacto directo en decisiones y ROI         |
