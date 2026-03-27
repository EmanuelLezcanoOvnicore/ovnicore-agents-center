# 🔗 OVNICORE 2.0 — Flujos de Interacción entre Agentes

> **Propósito**: Este documento define EXACTAMENTE cómo interactúan los agentes de OVNICORE 2.0 entre sí, qué hace cada uno, y cuáles son todos los flujos operativos posibles. Es la guía definitiva para implementar la orquestación en OpenClaw.

---

## 1. CONTEXTO OPERATIVO ACTUAL DE OVNICORE

### 1.1 Stack Tecnológico
| Componente      | Tecnología                          |
|-----------------|-------------------------------------|
| Backend         | **Go (Golang)**                     |
| Frontend        | **Vue.js**                          |
| Base de datos   | **PostgreSQL**                      |
| Infraestructura | **VPS DonWeb** (apps estándar) / **AWS** (servicios críticos) |

### 1.2 Sistemas Existentes

**Back Office OVNICORE** (Go + Vue.js + PostgreSQL):
- Registro de gastos (datos ingresados manualmente).
- Generación automática de facturas de ARCA cuando se registra un ingreso de dinero.
- Los datos del cliente se completan automáticamente según el cliente al que se presta el servicio.
- CRUD completo de clientes, servicios, contratos.

**Canales de Comunicación OVNICORE:**
- 📱 **WhatsApp** — Número dedicado de OVNICORE para atención y consultas.
- 📸 **Instagram** — Cuenta de OVNICORE para presencia de marca.
- 🌐 **Landing Page** — Con formulario de consultas integrado (leads).
- Para cada app/producto que OVNICORE crea (que no son para clientes): Instagram y contactos propios.

### 1.3 Principios Inquebrantables
- **SUPER SEGURIDAD**: Encriptación, audit trails, validaciones en cada capa. Nada pasa sin registro.
- **ESCALABILIDAD**: Toda arquitectura debe soportar crecimiento exponencial.
- **UX INTUITIVA**: El usuario final siempre primero. La complejidad es interna, la experiencia del usuario es simple.

---

## 2. QUÉ HACE CADA AGENTE (Resumen Ejecutivo)

```
┌──────────────────────────────────────────────────────────────┐
│                    CEO + SOCIO (Humanos)                     │
│           Decisión final. Aprobación estratégica.            │
└────────────────────────┬─────────────────────────────────────┘
                         │
           ┌─────────────▼─────────────┐
           │    GESTOR PRINCIPAL        │
           │    (El cerebro estratégico)│
           │    Claude Opus             │
           └──┬──┬──┬──┬───────────────┘
              │  │  │  │
    ┌─────────┘  │  │  └──────────┐
    ▼            ▼  ▼             ▼
┌────────┐ ┌────────┐ ┌────────┐ ┌──────────────┐
│ ADMIN  │ │  DEV   │ │MARKET. │ │    LOCO DE   │
│        │ │        │ │        │ │ PROGRAMACIÓN │
│Sonnet  │ │Sonnet  │ │Sonnet  │ │    Opus      │
└────┬───┘ └────┬───┘ └────┬───┘ └──────┬───────┘
     │          │          │             │
     └──────────┴──────────┴─────────────┘
                    │
           ┌───────▼───────┐
           │  APROBACIONES  │
           │  (El guardián) │
           │  Sonnet        │
           └────────────────┘
```

### AGENTE GESTOR PRINCIPAL — "El Cerebro Estratégico"
**¿Qué hace?** Recibe las ideas del CEO, las analiza con datos reales (mercado, competencia, costos, ROI), y coordina a los demás agentes para ejecutarlas.

| Responsabilidad | Detalle |
|-----------------|---------|
| Evaluar ideas del CEO | Análisis de viabilidad completo (TAM/SAM/SOM, competencia, timeline, presupuesto, ROI) |
| Coordinar agentes | Distribuir tareas, monitorear progreso, resolver conflictos entre agentes |
| Reportes al CEO | Morning briefing diario (9 AM) + Weekly report (viernes) |
| Detección proactiva | Identificar oportunidades, riesgos, optimizaciones antes de que se pidan |
| Priorización | Decidir qué es urgente, qué puede esperar, qué se descarta |

**NO decide**: Inversiones > ARS 50K, cambios estratégicos, partnerships, precios → Eso es del CEO.

---

### AGENTE ADMINISTRACIÓN — "El Corazón Operativo"
**¿Qué hace?** Gestiona TODA la interacción con clientes (WhatsApp, Instagram, Email, Landing Page), tickets de soporte, contratos, facturación, cobranzas.

| Responsabilidad | Detalle |
|-----------------|---------|
| WhatsApp OVNICORE | Recibir consultas, responder FAQs, derivar problemas técnicos |
| Instagram OVNICORE | Responder DMs, derivar leads al flujo de ventas |
| Landing Page | Procesar formularios de consulta, registrar leads, responder |
| Facturación ARCA | Interactuar con el Back Office existente (Go+Vue.js) para facturas de ingresos |
| Gestión de tickets | Crear, asignar, monitorear SLA, escalar si se vence |
| Contratos | Alertar vencimientos (30 días antes), gestionar renovaciones |
| Cobranzas | Recordatorios automáticos a 7/14/30 días de atraso |
| Reportes | Diario, semanal, mensual de operaciones |

**Interacción con Back Office existente:**
- Cuando se registra un ingreso → El Back Office genera la factura ARCA automáticamente.
- El Agente Admin monitorea los pagos pendientes y envía recordatorios por WhatsApp/Email.
- Los datos del cliente se toman automáticamente del sistema existente.

**NO hace sin aprobación CEO**: Responder clientes nuevos (primera vez), ofrecer descuentos, comprometer plazos, cambiar contratos.

---

### AGENTE DESARROLLO — "El Guardián de la Infraestructura"
**¿Qué hace?** Mantiene TODO corriendo: deployments, seguridad, monitoreo, backups, performance. Es el SRE/DevOps.

| Responsabilidad | Detalle |
|-----------------|---------|
| Deployments | CI/CD con GitHub Actions, Docker, staging → producción |
| Infraestructura | VPS DonWeb (apps estándar), AWS (servicios críticos) |
| Seguridad | SSL/TLS, WAF, rate limiting, CORS, JWT, encriptación, patches |
| Monitoreo 24/7 | CPU, memoria, error rate, response time, DB health |
| Backups | Diarios encriptados (pg_dump), retención 30 días |
| Performance | Redis caching, CDN, query optimization, indexing |
| Resolución incidentes | Root cause analysis, fix, documentación, post-mortem |

**Stack que maneja directamente:**
- Backend: **Go** (APIs existentes y nuevas).
- Frontend: **Vue.js** (apps existentes).
- DB: **PostgreSQL** (optimización, réplicas, backups).
- Infra: **VPS DonWeb** / **AWS** según criticidad.

**PUEDE hacer solo**: Hotfixes de seguridad, rollbacks, escalar recursos, backups manuales.
**NO hace solo**: Deploy de app nueva a producción, cambios de arquitectura, migración de cloud.

---

### AGENTE MARKETING — "La Voz de OVNICORE"
**¿Qué hace?** Gestiona la presencia en redes sociales, genera contenido, detecta tendencias, capitaliza oportunidades, gestiona la reputación online.

| Responsabilidad | Detalle |
|-----------------|---------|
| Instagram OVNICORE | 2-3 posts/día, stories, reels, responder comentarios |
| Instagram de Apps propias | Contenido específico para cada producto de OVNICORE |
| Contenido | Copywriting, imágenes (DALL-E/Canva), scripts de video |
| Tendencias | Monitoreo continuo de trends tech/IA/mercado argentino |
| Reputación | Detectar quejas, NO responder sin OK del CEO, derivar a Admin |
| Lanzamientos | Campañas de teaser → reveal → launch para apps nuevas |
| Reportes | Métricas diarias, análisis semanal, strategy mensual |

**PUEDE publicar solo**: Posts sobre temas ya validados (IA, tips, educativo), respuestas a comentarios positivos.
**NO publica sin CEO**: Mención a competencia, temas polémicos, descuentos, respuestas a quejas.

---

### AGENTE APROBACIONES — "El Sistema Inmunológico"
**¿Qué hace?** Evalúa CADA acción crítica de TODOS los agentes con un score de confianza y decide: auto-ejecutar, pedir aprobación al CEO, o bloquear.

| Responsabilidad | Detalle |
|-----------------|---------|
| Score de confianza | Evalúa cada acción (0-100): >85% auto, 60-85% pide OK, <60% bloquea |
| Audit trail | Registra CADA acción de CADA agente. Inmutable. |
| Detección anomalías | 100+ facturas en 10min, accesos no autorizados, patrones sospechosos |
| Compliance | RGPD, Ley 25.326, límites de autoridad, SLAs |
| Bloqueo de agentes | Si detecta anomalía → bloquea agente hasta que CEO decide |

**REGLA DE ORO**: Si hay duda entre aprobar y bloquear → BLOQUEAR y escalar al CEO.

---

### AGENTE LOCO DE PROGRAMACIÓN — "El Motor de Innovación"
**¿Qué hace?** Transforma ideas en productos vendibles. Es el CTO-emprendedor: propone, diseña, coordina desarrollo paralelo, y lanza MVPs.

| Responsabilidad | Detalle |
|-----------------|---------|
| Evaluación de ideas | En 30 min: análisis de mercado, modelo de negocio, MVP scope, stack, ROI |
| Arquitectura de apps | Diseña la estructura técnica completa de cada nueva app |
| Coordinación paralela | Divide en módulos, asigna a agentes, daily standups, integra |
| Stack decisions | Recomienda tecnologías basándose en contexto y negocio |
| Oportunidades | Propone 1+ oportunidades de negocio por semana (proactivamente) |
| Optimización | Root cause + fix + prevención + mejora cuantificable |

**Stack base que usa para apps nuevas:**
- Backend: **Go** (consistencia con stack existente).
- Frontend: **Vue.js** (consistencia con stack existente).
- DB: **PostgreSQL** (estándar OVNICORE).
- Deploy: **VPS DonWeb** (estándar) / **AWS** (crítico).

**PUEDE decidir solo**: Stack técnico (si no excede presupuesto), paralelización, refactoring.
**NO decide solo**: Inversiones > ARS 50K, lanzamiento de proyectos nuevos (siempre necesita GO del CEO).

---

## 3. TODOS LOS FLUJOS DE INTERACCIÓN

### 📌 FLUJO 1 — CEO TIENE UNA IDEA NUEVA

```
CEO envía idea (texto/audio/imagen) 
    │
    ▼
GESTOR PRINCIPAL
    │  Interpreta la idea, hace preguntas de clarificación si necesario (máx 3)
    │
    ├──→ LOCO DE PROGRAMACIÓN
    │       │  Genera análisis técnico COMPLETO en 30 min:
    │       │  - Mercado (TAM/SAM/SOM)
    │       │  - Stack recomendado (Go + Vue.js + PostgreSQL como base)
    │       │  - MVP features
    │       │  - Timeline (semanas)
    │       │  - Presupuesto
    │       │  - ROI proyectado
    │       │
    │       ▼
    │  GESTOR PRINCIPAL consolida:
    │    - Análisis técnico del LOCO
    │    - Su propio análisis estratégico
    │    - Recomendación GO ✅ / NO-GO ❌ / INVESTIGAR MÁS 🔍
    │    - Confianza: X%
    │
    ▼
CEO recibe propuesta completa → DECIDE
    │
    ├── SI APRUEBA (GO) ──────────────────────────────────┐
    │                                                       │
    │   LOCO coordina desarrollo paralelo:                 │
    │   ├── Módulo Backend → DESARROLLO (Go API)           │
    │   ├── Módulo Frontend → DESARROLLO (Vue.js)          │
    │   ├── Módulo Infra → DESARROLLO (VPS/AWS + CI/CD)    │
    │   ├── Pre-venta → MARKETING (landing, content)       │
    │   └── Setup admin → ADMIN (contratos, billing)       │
    │                                                       │
    │   Daily standups virtuales (15 min)                   │
    │   Integración semana 2-3                              │
    │   Testing exhaustivo                                  │
    │                                                       │
    │   APROBACIONES valida deploy a producción             │
    │   ├── Confianza > 85% → Auto-ejecuta                 │
    │   └── Confianza < 85% → Pide OK al CEO               │
    │                                                       │
    │   DESARROLLO hace deploy a producción                 │
    │   MARKETING lanza campaña                             │
    │   ADMIN onboarding primeros clientes                  │
    │                                                       │
    └── SI RECHAZA (NO-GO) ────────────────────────────────┘
        GESTOR registra motivo, archiva para referencia futura.
```

---

### 📌 FLUJO 2 — LLEGA UN MENSAJE POR WHATSAPP (Cliente o Lead)

```
Cliente/Lead envía mensaje por WhatsApp OVNICORE
    │
    ▼
ADMINISTRACIÓN recibe (via Evolution API / webhook)
    │
    ├── Analiza mensaje internamente:
    │   - ¿Quién es? (cliente existente / nuevo / desconocido)
    │   - ¿Qué quiere? (soporte / consulta / queja / factura / venta)
    │   - ¿Qué urgencia tiene? (crítica / alta / normal / baja)
    │   - ¿Puedo responder solo? → Calcula CONFIANZA (0-100%)
    │
    ├── CONFIANZA > 90% y es FAQ ──────────────────────────┐
    │   Auto-responde al cliente directamente.              │
    │   Registra en BD (conversación + audit trail).        │
    │   APROBACIONES registra como AUTO_EJECUTADA.          │
    │                                                       │
    ├── CONFIANZA 60-90% ──────────────────────────────────┐
    │   Envía respuesta propuesta al CEO para aprobación.   │
    │   "Revisar respuesta a [Cliente X]: '[msg]'. ✅ o ❌?"│
    │   CEO aprueba → Envía al cliente.                     │
    │   CEO rechaza → Modifica y repite.                    │
    │   APROBACIONES registra como APROBADA_CEO.            │
    │                                                       │
    ├── CONFIANZA < 60% ───────────────────────────────────┐
    │   BLOQUEA. No responde al cliente.                    │
    │   Escala al CEO con contexto completo.                │
    │   APROBACIONES registra como BLOQUEADA.               │
    │   CEO define qué responder.                           │
    │                                                       │
    ├── ES PROBLEMA TÉCNICO ───────────────────────────────┐
    │   Crea TICKET y lo asigna a DESARROLLO.               │
    │   DESARROLLO diagnostica y resuelve.                  │
    │   ADMIN notifica al cliente cuando está resuelto.     │
    │                                                       │
    ├── ES LEAD (potencial cliente nuevo) ─────────────────┐
    │   Registra en BD como lead.                           │
    │   Notifica al CEO: "Nuevo lead: [datos]. ¿Respondo?" │
    │   CEO autoriza → ADMIN responde con info de servicios.│
    │   MARKETING puede ser notificado para remarketing.    │
    │                                                       │
    └── ES QUEJA ──────────────────────────────────────────┐
        NO responde. Escala al CEO inmediatamente.          │
        Adjunta historial completo del cliente.             │
        CEO decide cómo responder.                          │
        APROBACIONES registra todo.                         │
```

---

### 📌 FLUJO 3 — LLEGA UN FORMULARIO DESDE LA LANDING PAGE

```
Lead llena formulario en landing page de OVNICORE
    │
    ▼
ADMINISTRACIÓN recibe (via webhook del formulario)
    │
    ├── Registra lead en PostgreSQL (tabla leads)
    ├── Categoriza: ¿qué servicio le interesa?
    ├── Notifica al CEO: "Nuevo lead desde landing: [Nombre], [Email], [Consulta]"
    │
    ├── CEO autoriza respuesta ─────────────────────────────┐
    │   ADMIN responde por email con info de servicios.      │
    │   Si el lead responde → Se inicia flujo normal de      │
    │   atención (WhatsApp o Email).                         │
    │                                                        │
    └── MARKETING recibe notificación ──────────────────────┐
        Puede agregar al lead a lista de remarketing.        │
        Puede personalizar contenido futuro basado en el lead.│
```

---

### 📌 FLUJO 4 — LLEGA UN DM POR INSTAGRAM

```
Usuario envía DM por Instagram de OVNICORE
    │
    ▼
ADMINISTRACIÓN recibe (via Meta API / webhook)
    │
    ├── ¿Es consulta simple? → Responde (si confianza > 90%)
    ├── ¿Es lead? → Registra y notifica al CEO para autorización
    ├── ¿Es queja pública? → NO responde, escala a CEO + notifica MARKETING
    ├── ¿Es comentario en post? → MARKETING responde (si es positivo/neutral)
    │
    └── Si es consulta que necesita seguimiento → 
        ADMIN propone continuar conversación por WhatsApp.
```

---

### 📌 FLUJO 5 — ALERTA DE INFRAESTRUCTURA (Monitored 24/7)

```
Sistema de monitoreo detecta problema
    │
    ├── CPU > 80% por 5 min ───────────────────────────────┐
    │   DESARROLLO diagnostica automáticamente.              │
    │   Si es resoluble → Auto-escala o optimiza.            │
    │   Si persiste → Notifica CEO.                          │
    │                                                        │
    ├── Error rate > 5% ───────────────────────────────────┐
    │   DESARROLLO intenta auto-fix o rollback.              │
    │   Notifica CEO inmediatamente.                         │
    │   APROBACIONES registra incidente.                     │
    │                                                        │
    ├── Servicio caído ────────────────────────────────────┐
    │   DESARROLLO actúa en < 5 minutos.                     │
    │   Notifica CEO INMEDIATAMENTE.                         │
    │   Intenta rollback o fix.                              │
    │   Post-mortem obligatorio.                             │
    │   APROBACIONES registra todo.                          │
    │                                                        │
    ├── Intento de acceso no autorizado ───────────────────┐
    │   DESARROLLO bloquea IP/acceso inmediatamente.         │
    │   Notifica CEO + APROBACIONES.                         │
    │   APROBACIONES activa protocolo de seguridad.          │
    │   Investigación de breach.                             │
    │                                                        │
    └── Backup fallido ────────────────────────────────────┐
        DESARROLLO re-ejecuta backup.                        │
        Si falla 2 veces → Notifica CEO.                     │
        NUNCA dejar pasar un backup fallido sin resolución.   │
```

---

### 📌 FLUJO 6 — GESTIÓN DE CONTRATOS Y COBRANZAS

```
DIARIO: ADMIN revisa contratos y pagos pendientes
    │
    ├── Contrato vence en 30 días ─────────────────────────┐
    │   ADMIN notifica CEO:                                  │
    │   "Contrato con [Empresa] vence el [fecha].            │
    │    Valor: ARS [monto]. ¿Renovamos?"                    │
    │   Opciones: Renovar | Consultar cliente | Dejar vencer │
    │   APROBACIONES registra decisión.                      │
    │                                                        │
    ├── Contrato vence HOY ────────────────────────────────┐
    │   Notificación URGENTE al CEO.                         │
    │   Si no hay respuesta → Marcar VENCIDO automáticamente.│
    │                                                        │
    ├── Pago atrasado 7 días ──────────────────────────────┐
    │   ADMIN envía recordatorio AMIGABLE por WhatsApp/Email.│
    │   (Auto-envío si el tipo de recordatorio fue pre-aprobado)│
    │                                                        │
    ├── Pago atrasado 14 días ─────────────────────────────┐
    │   ADMIN envía segundo recordatorio, tono más directo.  │
    │                                                        │
    └── Pago atrasado 30+ días ────────────────────────────┐
        ADMIN escala al CEO:                                 │
        "Cliente [X] debe ARS [Y] hace 30 días.              │
         3 recordatorios enviados sin respuesta.              │
         Opciones: Llamada telefónica | Suspender servicio | │
         Derivar a cobrador"                                  │
        CEO elige → ADMIN ejecuta.                            │
        APROBACIONES registra todo.                           │
```

---

### 📌 FLUJO 7 — FACTURACIÓN (Integración con Back Office Existente)

```
Se registra un ingreso en el Back Office (Go + Vue.js)
    │
    ▼
Back Office genera FACTURA ARCA automáticamente
    │  - Datos del cliente se completan automáticamente
    │  - Tipo de factura según configuración
    │
    ├── ADMIN monitorea el estado de la factura
    │   - ¿Fue emitida correctamente?
    │   - ¿El cliente la recibió?
    │   - ¿El pago correspondiente fue registrado?
    │
    ├── Si monto > ARS 10.000 ─────────────────────────────┐
    │   APROBACIONES valida antes de emitir.                  │
    │   Si confianza > 85% → Auto-ejecuta.                   │
    │   Si confianza < 85% → CEO aprueba.                    │
    │                                                         │
    └── ADMIN registra en audit trail completo
        - Quién emitió, cuándo, a quién, por cuánto, estado.
```

---

### 📌 FLUJO 8 — MARKETING DETECTA OPORTUNIDAD / TENDENCIA

```
MARKETING detecta trend o evento relevante
    │
    ├── ¿Es oportunidad de CONTENIDO? ─────────────────────┐
    │   Genera post/reel/story.                              │
    │   Si es tema ya validado → Auto-publica.               │
    │   Si es tema nuevo → Envía a CEO para aprobación.      │
    │                                                        │
    ├── ¿Es oportunidad de NEGOCIO (nueva app/servicio)? ──┐
    │   Notifica al GESTOR PRINCIPAL:                        │
    │   "Detecté que [mercado X] tiene [gap Y].              │
    │    OVNICORE podría [propuesta Z]."                      │
    │   GESTOR evalúa → Si interesante → Pasa al LOCO.      │
    │   LOCO genera análisis completo.                       │
    │   CEO decide GO/NO-GO.                                 │
    │                                                        │
    └── ¿Es crisis/queja pública? ─────────────────────────┐
        NO responde en público.                              │
        Notifica CEO + ADMIN inmediatamente.                 │
        ADMIN busca historial del cliente.                   │
        CEO autoriza respuesta.                              │
        MARKETING publica respuesta profesional.             │
        ADMIN toma seguimiento en privado.                   │
```

---

### 📌 FLUJO 9 — LOCO PROPONE OPORTUNIDAD PROACTIVAMENTE (Semanal)

```
Cada lunes, LOCO DE PROGRAMACIÓN genera:
    │
    ▼
REPORTE DE OPORTUNIDADES
    ├── Oportunidad 1: [App/Servicio] — TAM, esfuerzo, ROI
    ├── Oportunidad 2: [App/Servicio] — TAM, esfuerzo, ROI
    └── Oportunidad 3: [App/Servicio] — TAM, esfuerzo, ROI
    │
    ▼
GESTOR PRINCIPAL evalúa viabilidad estratégica
    │  Filtra las más prometedoras
    │
    ▼
CEO recibe top oportunidades con recomendación del GESTOR
    │
    ├── CEO elige una → Se activa FLUJO 1 (Idea Nueva)
    └── CEO descarta → Se archiva para referencia futura
```

---

### 📌 FLUJO 10 — REPORTES AUTOMÁTICOS

```
DIARIOS (9 AM Argentina):
    │
    ├── GESTOR PRINCIPAL → Morning Briefing al CEO:
    │   ✅ 3 cosas completadas ayer
    │   🔄 3 cosas en progreso hoy
    │   ⚠️ 2 alertas que requieren atención
    │
    ├── ADMIN → Resumen operativo:
    │   - Mensajes recibidos/respondidos
    │   - Tickets abiertos/cerrados
    │   - Pagos recibidos
    │
    └── MARKETING → Métricas del día:
        - Posts publicados, alcance, engagement
        - Top post, comentarios respondidos

SEMANALES (Viernes PM):
    │
    ├── GESTOR → Weekly Report completo al CEO
    ├── ADMIN → Tickets por categoría, clientes en riesgo
    ├── MARKETING → Growth, top posts, trends, recomendaciones
    ├── DESARROLLO → Uptime, performance, incidentes, deployments
    ├── LOCO → Oportunidades nuevas, progreso de proyectos
    └── APROBACIONES → Compliance report: acciones por agente,
        aprobaciones, rechazos, anomalías detectadas
```

---

### 📌 FLUJO 11 — TICKET DE SOPORTE (Ciclo de Vida Completo)

```
Cliente reporta problema (WhatsApp/Instagram/Email/Landing)
    │
    ▼
ADMIN crea ticket en PostgreSQL:
    │  - Prioridad: CRÍTICA | ALTA | NORMAL | BAJA
    │  - Categoría: TÉCNICO | FACTURACIÓN | CONSULTA | QUEJA
    │  - Asigna a agente responsable
    │
    ▼
TICKET: ABIERTO
    │
    ├── Si es TÉCNICO → Asigna a DESARROLLO
    ├── Si es FACTURACIÓN → ADMIN resuelve directamente
    ├── Si es CONSULTA → ADMIN responde
    └── Si es QUEJA → Escala al CEO
    │
    ▼
TICKET: EN_PROGRESO (agente confirma que trabaja en ello)
    │
    │  ⏰ Si SLA vence (2h) sin progreso → ADMIN notifica al agente
    │  ⏰ Si SLA+2h (4h) sin progreso → ADMIN escala al CEO
    │
    ▼
TICKET: EN_REVISIÓN (solución lista, validando)
    │
    ▼
TICKET: APROBADO (CEO/agente valida solución)
    │
    ▼
ADMIN notifica al cliente: "Tu problema fue resuelto: [solución]"
    │
    ▼
TICKET: CERRADO (cliente confirma o SLA cumplido)
    │
    └── APROBACIONES registra ciclo completo en audit trail
```

---

## 4. MATRIZ DE INTERACCIONES DIRECTAS

Esta tabla muestra QUIÉN puede hablar con QUIÉN y PARA QUÉ:

| Origen → Destino                   | Tipo de Interacción                                      |
|------------------------------------|----------------------------------------------------------|
| **CEO → GESTOR**                   | Ideas, preguntas, órdenes, feedback                      |
| **GESTOR → CEO**                   | Análisis, reportes, recomendaciones, escalaciones        |
| **GESTOR → LOCO**                  | "Evalúa esta idea", "¿Cómo avanza el proyecto X?"       |
| **GESTOR → ADMIN**                 | "¿Estado de tickets?", "¿Cobranzas al día?"              |
| **GESTOR → DEV**                   | "¿Uptime esta semana?", "¿Status deploy?"               |
| **GESTOR → MARKETING**             | "¿Métricas de la semana?", "Prepará contenido de [tema]" |
| **LOCO → DEV**                     | "Implementá este módulo", specs técnicas, API contracts  |
| **LOCO → MARKETING**              | "App lista para beta, prepará campaña"                   |
| **LOCO → ADMIN**                   | "Setup administrativo necesario: contratos, billing"     |
| **LOCO → GESTOR**                  | "Propuesta lista, datos adjuntos, necesito OK del CEO"   |
| **ADMIN → DEV**                    | Tickets técnicos asignados                               |
| **ADMIN → MARKETING**             | Leads para remarketing, datos de clientes para contenido |
| **ADMIN → CEO**                    | Solicitudes de aprobación de mensajes/facturas           |
| **DEV → CEO**                      | Alertas de incidentes críticos (servicio caído)          |
| **DEV → LOCO**                     | Status de módulos, blockers técnicos                     |
| **MARKETING → ADMIN**             | Leads detectados en redes, quejas públicas para gestión  |
| **MARKETING → GESTOR**            | Oportunidades de negocio detectadas en tendencias        |
| **MARKETING → CEO**               | Aprobación de posts polémicos/nuevos                     |
| **APROBACIONES → CEO**            | Escalaciones cuando confianza < 85%                      |
| **APROBACIONES → TODOS**          | Resultado de evaluación (aprobado/rechazado/bloqueado)   |
| **TODOS → APROBACIONES**          | Solicitud de validación de acciones críticas             |

---

## 5. REGLAS DE SEGURIDAD (Aplican a TODOS los agentes)

```
┌──────────────────────────────────────────────────────────────┐
│                REGLAS DE SEGURIDAD OVNICORE                  │
│                   (INQUEBRANTABLES)                           │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  1. AUDIT TRAIL: Toda acción de todo agente se registra     │
│     en PostgreSQL (tabla auditoria_agentes). INMUTABLE.      │
│                                                              │
│  2. APROBACIONES: Toda acción financiera > ARS 5.000 o      │
│     que afecte datos de clientes pasa por el Agente          │
│     Aprobaciones ANTES de ejecutarse.                        │
│                                                              │
│  3. ESCALACIÓN: Si confianza < 60% → BLOQUEAR y escalar     │
│     al CEO. NUNCA ejecutar con baja confianza.               │
│                                                              │
│  4. DATOS: Nunca compartir datos de un cliente con otro.     │
│     Nunca exponer datos sensibles en logs o mensajes.        │
│                                                              │
│  5. IDENTIDAD: Los agentes NUNCA revelan que son IA         │
│     cuando hablan con clientes. Si preguntan, derivar        │
│     a soporte humano.                                        │
│                                                              │
│  6. INFRAESTRUCTURA:                                         │
│     - HTTPS obligatorio en todo endpoint.                    │
│     - Variables de ambiente NUNCA hardcodeadas.               │
│     - Backups diarios encriptados y verificados.             │
│     - Patches de seguridad aplicados < 24h si críticos.      │
│     - JWT con expiración + refresh tokens.                   │
│     - Rate limiting en todas las APIs.                       │
│     - CORS estricto (solo dominios permitidos).              │
│                                                              │
│  7. COMPLIANCE: RGPD (si hay clientes EU) + Ley 25.326     │
│     (Argentina) para protección de datos personales.         │
│                                                              │
│  8. ANOMALÍAS: Si se detecta patrón anómalo (100+           │
│     acciones en minutos, accesos no autorizados, montos      │
│     10x mayores al promedio) → BLOQUEAR agente +             │
│     ESCALAR al CEO inmediatamente.                           │
│                                                              │
│  9. ROLLBACK: Siempre tener plan de rollback antes de       │
│     cada deploy. Si error rate > 5% → rollback automático.   │
│                                                              │
│ 10. PRINCIPIO DE MÍNIMO PRIVILEGIO: Cada agente solo        │
│     accede a los datos que necesita para su función.         │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## 6. RESUMEN VISUAL — TODOS LOS CANALES Y FLUJOS

```
           CANALES EXTERNOS                    AGENTES OVNICORE
         ─────────────────                   ──────────────────

    📱 WhatsApp OVNICORE  ──────────→  ADMIN ──→ [TICKETS]
                                         │         [LEADS]
    📸 Instagram OVNICORE ──────────→  ADMIN ──→ [QUEJAS → CEO]
         (posts)          ──────────→  MARKETING
                                         │
    🌐 Landing Page       ──────────→  ADMIN ──→ [LEADS → CEO]
         (formularios)
                                         │
    📧 Email              ──────────→  ADMIN ──→ [CONTRATOS]
                                         │        [FACTURAS]
                                         │
    💰 Back Office        ──────────→  ADMIN ──→ [FACTURA ARCA]
       (Go + Vue.js)                     │        [COBRANZAS]
                                         │
    🖥️ Infraestructura    ──────────→  DEV   ──→ [ALERTAS → CEO]
       (VPS/AWS)                         │        [BACKUPS]
                                         │        [DEPLOYS]
                                         │
    💡 Ideas del CEO      ──────────→  GESTOR ──→ LOCO ──→ [MVP]
                                         │
    📊 Tendencias         ──────────→  MARKETING ──→ [CONTENIDO]
       (Redes/Noticias)                  │              [OPORTUNIDADES]
                                         │
                              APROBACIONES ◄── TODOS
                                  │
                                  ▼
                            [AUDIT TRAIL]
                            [COMPLIANCE]
                            [SEGURIDAD]
```

---

**Versión**: 1.0  
**Fecha**: Marzo 2026  
**Estado**: 🟢 Listo para implementación en OpenClaw
