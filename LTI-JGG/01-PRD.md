# 📘 Documento de Requisitos del Producto (PRD)

## 1. Introducción y Objetivos

### Propósito del producto

**LTI (Lean Talent Intelligence)** es un **Applicant Tracking System (ATS)** de nueva generación diseñado para acelerar los procesos de reclutamiento, mejorar la colaboración entre equipos de RR.HH. y managers técnicos, y reducir la carga administrativa mediante automatización pragmática e inteligencia artificial trazable.

### Metas y objetivos de negocio

* Reducir el **time-to-hire** mediante un flujo de trabajo optimizado y visible.
* Mejorar la **colaboración entre reclutadores y managers** con un timeline auditable.
* Integrar **IA asistiva** para generar descripciones, resúmenes y comunicaciones sin perder control ni trazabilidad.
* Ofrecer una base técnica escalable que permita evolucionar hacia automatización y analítica avanzada sin rediseñar el sistema.

### Problemas o necesidades que resuelve

* Procesos de selección lentos, fragmentados y difíciles de auditar.
* Comunicación dispersa y decisiones no registradas.
* Repetición de tareas operativas (emails, JD, resúmenes).
* Falta de visibilidad sobre el estado real de las vacantes.

---

## 2. Stakeholders

| Rol / Parte interesada                         | Descripción / Responsabilidad principal                                    |
| ---------------------------------------------- | -------------------------------------------------------------------------- |
| **Reclutadores (HR)**                          | Gestionan vacantes, crean pipelines, mueven candidatos y registran notas.  |
| **Hiring Managers**                            | Evalúan candidatos, añaden comentarios y toman decisiones finales.         |
| **Administradores de Organización**            | Configuran usuarios, permisos y seguimiento general.                       |
| **Equipo de Producto y Desarrollo**            | Diseñan, implementan y mantienen el sistema LTI.                           |
| **Stakeholders externos (IA / Integraciones)** | Servicios de IA y comunicación (Email/Slack) integrados de forma opcional. |

---

## 3. Historias de Usuarios

1. **Como reclutador**, quiero crear vacantes y definir etapas del pipeline, para organizar el proceso de selección de forma consistente.
2. **Como manager**, quiero visualizar el progreso de los candidatos en un timeline, para tomar decisiones rápidas y coordinadas.
3. **Como reclutador**, quiero generar descripciones de puestos o correos usando IA, para reducir tiempo y mantener coherencia en la comunicación.
4. **Como usuario**, quiero añadir notas o valoraciones directamente en la aplicación, para centralizar la colaboración sin depender de otros canales.
5. **Como organización**, quiero medir el tiempo y conversión por etapa, para optimizar el proceso de contratación.

---

## 4. Componentes Principales y Sitemaps

### Módulos principales

1. **Gestión de Vacantes**

   * Creación, edición y publicación.
   * Estados: `draft`, `open`, `on_hold`, `closed`.

2. **Pipeline de Selección**

   * Configuración de etapas (`screen`, `phone`, `tech`, `onsite`, `offer`).
   * Reordenación y validación de transiciones.

3. **Aplicaciones y Candidatos**

   * Asociación candidato↔vacante.
   * Movimiento entre etapas con eventos auditables.

4. **Timeline y Notas**

   * Registro de eventos (`moved_stage`, `note`, `scored`, etc.).
   * Comentarios de usuarios e IA con trazabilidad.

5. **IA para Textos Operativos**

   * Generación de JD, emails o resúmenes.
   * Almacenamiento como `Note (source='ai')`.

6. **Búsqueda y Filtrado**

   * Por vacante, estado y etapa.

### Estructura general (texto)

```text
Inicio
 ├── Vacantes
 │    ├── Pipeline
 │    │    └── Etapas
 │    └── Aplicaciones
 │         ├── Candidato
 │         ├── Timeline (eventos + notas)
 │         └── Acciones (mover, valorar)
 ├── IA (Textos)
 └── Administración (usuarios / organización)
```

---

## 5. Características y Funcionalidades

| Feature                               | Descripción                                                               | Valor                                    |
| ------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------- |
| **Pipeline inteligente**              | Flujo visual con etapas personalizables y registro automático de eventos. | Reduce errores y facilita seguimiento.   |
| **Timeline unificado**                | Combina eventos y notas por aplicación.                                   | Permite decisiones rápidas y auditables. |
| **Notas y valoraciones**              | Comentarios colaborativos con origen (usuario/IA).                        | Centraliza la comunicación.              |
| **Generación asistida por IA**        | Textos generados (JD, correos, resúmenes).                                | Ahorra tiempo y mantiene consistencia.   |
| **Búsqueda y filtrado rápido**        | Acceso inmediato a candidatos relevantes.                                 | Mejora eficiencia diaria.                |
| **Arquitectura modular y extensible** | Basada en eventos (`ApplicationEvent`).                                   | Permite evolución sin deuda técnica.     |

---

## 6. Diseño y Experiencia de Usuario

* **Diseño centrado en la velocidad operativa:** cada acción clave debe realizarse con un máximo de 2 clics.
* **Timeline como eje visual:** combina transparencia y colaboración sin sobrecargar la interfaz.
* **IA integrada como asistente contextual**, no como sustituto del usuario.
* **Accesibilidad:** soporte para teclado y lectores de pantalla básicos.
* **Consistencia UI:** misma estructura en vacantes, aplicaciones y notas.

---

## 7. Requisitos Técnicos

| Aspecto                     | Detalle                                             |
| --------------------------- | --------------------------------------------------- |
| **Lenguaje / Framework**    | .NET 9 (Minimal API)                                |
| **Base de datos**           | PostgreSQL 16                                       |
| **Frontend**                | SPA React/Next.js                                   |
| **Autenticación**           | JWT                                                 |
| **Versionado API**          | Header `x-api-version`                              |
| **Persistencia de eventos** | Tabla `ApplicationEvent` para auditoría y analítica |
| **Observabilidad**          | OpenTelemetry + ProblemDetails                      |
| **Infraestructura inicial** | Monolito modular (sin colas ni workers)             |
| **Escalabilidad futura**    | Outbox + Worker (para notificaciones y reglas)      |
| **Integraciones MVP**       | Slack/Email manuales, IA externa opcional           |

---

## 8. Planificación del Proyecto

### Fases

1. **Fase 1 – Fundamentos (MVP Core)**

   * Entidades base (8 tablas).
   * CRUD de vacantes, pipeline, candidatos y aplicaciones.
   * Timeline y notas.

2. **Fase 2 – Integraciones e IA**

   * Asistente de IA para JD, resúmenes y correos.
   * Integración manual con Email/Slack.

3. **Fase 3 – Observabilidad y Métricas**

   * OpenTelemetry y ProblemDetails.
   * Reportes básicos de conversión por etapa.

4. **Fase 4 – Escalabilidad progresiva**

   * Incorporación de Outbox + Worker.
   * Extensión hacia automatizaciones y notificaciones.

### Riesgos y dependencias

* Dependencia de servicio externo de IA.
* Latencia o coste en generación de textos.
* Necesidad futura de colas o workers para escalabilidad.

---

## 9. Criterios de Aceptación

| Requisito                | Criterio de aceptación                                                       |
| ------------------------ | ---------------------------------------------------------------------------- |
| Creación de vacantes     | El usuario puede crear una vacante y definir etapas del pipeline.            |
| Movimiento de candidatos | El cambio de etapa genera un `ApplicationEvent` y se refleja en el timeline. |
| Registro de notas        | Se pueden añadir notas de usuario y IA visibles en el timeline.              |
| Generación IA            | Los textos generados se guardan como `Note (source='ai')` con trazabilidad.  |
| Autenticación            | Las acciones requieren JWT válido.                                           |
| Auditoría                | Cada acción relevante queda registrada con `CreatedBy` y `OccurredAt`.       |

---

## 10. Apéndices y Recursos Adicionales

### Glosario

* **ATS:** Applicant Tracking System.
* **Pipeline:** Flujo de etapas por las que pasa una aplicación.
* **ApplicationEvent:** Registro de eventos que describe la historia de una aplicación.
* **Timeline:** Vista combinada de eventos y notas.
* **MVP:** Producto mínimo viable.

### Referencias

* Documento fuente: *LTI — ATS de nueva generación (Documento MVP)*
* Diagramas: C4 y ER incluidos en la especificación original.
* Repositorio de referencia: ⚠️ [Falta información sobre la URL o repositorio técnico asociado].

---

✅ **Resumen ejecutivo:**
El PRD sintetiza un MVP de ATS pragmático y escalable, centrado en **rapidez, colaboración mínima y trazabilidad**.
La arquitectura modular y la base de eventos garantizan evolución sin deuda técnica, permitiendo validar hipótesis de negocio con bajo riesgo.
