# 🔗 FLUJOS DE INTEGRACIÓN Y COORDINACIÓN DE AGENTES

## VISIÓN GENERAL

Los 6 agentes trabajan de forma orquestada pero autónoma. Este documento mapea cómo se comunican, qué información intercambian, y cómo escalan problemas.

---

## 1. FLUJOS PRINCIPALES

### FLUJO A: CEO Presenta Nueva Aplicación

```
INICIO: CEO → Agente Gestor Principal
  │
  ├─ ENTRADA: "Quiero una app de X"
  │
  ├─ AGENTE GESTOR PRINCIPAL analiza
  │  ├─ Investiga mercado
  │  ├─ Analiza competencia
  │  ├─ Calcula viabilidad
  │  └─ Genera reporte
  │
  ├─ AGENTE GESTOR → AGENTE LOCO DE PROGRAMACIÓN
  │  ├─ ENTRADA: Descripción, objetivos
  │  ├─ LOCO: Propone estrategia técnica completa
  │  │  ├─ Stack recomendado
  │  │  ├─ Timeline estimado
  │  │  ├─ Presupuesto
  │  │  └─ ROI proyectado
  │  └─ LOCO → GESTOR: Retorna reporte técnico
  │
  ├─ AGENTE GESTOR PRINCIPAL → CEO
  │  ├─ Presenta análisis completo
  │  └─ Recomendación: GO / NO-GO
  │
  ├─ SI GO (CEO aprueba):
  │  │
  │  └─ AGENTE LOCO inicia COORDINACIÓN PARALELA
  │     │
  │     ├─ LOCO → AGENTE DESARROLLO
  │     │  ├─ ENTRADA: "Setup infraestructura para [app]"
  │     │  ├─ Parámetros: Stack, servicios necesarios, deadline
  │     │  └─ DEV: Configura + retorna estado
  │     │
  │     ├─ LOCO → AGENTE MARKETING
  │     │  ├─ ENTRADA: "Prepara pre-venta para [app]"
  │     │  ├─ Parámetros: Features, pricing, timeline
  │     │  └─ MARKETING: Crea estrategia contenido
  │     │
  │     └─ LOCO → AGENTE ADMINISTRACIÓN
  │        ├─ ENTRADA: "Prepara setup administrativo"
  │        ├─ Parámetros: Estructura contratos, billing
  │        └─ ADMIN: Configura flujos
  │
  └─ SEGUIMIENTO: LOCO hace daily standups hasta launch
     └─ Escala blockers a GESTOR PRINCIPAL si necesario

FIN: App en producción
```

### FLUJO B: Cliente Contacta por WhatsApp

```
INICIO: Cliente → WhatsApp (Evolution API)
  │
  ├─ Evolution API recibe mensaje
  │
  ├─ n8n captura evento
  │  └─ Envía a AGENTE ADMINISTRACIÓN via Claude API
  │
  ├─ AGENTE ADMINISTRACIÓN analiza
  │  ├─ ¿Es FAQ? → Auto-responde (espera aprobación)
  │  ├─ ¿Es ticket nuevo? → Crea ticket
  │  └─ ¿Es urgente? → Escala a CEO
  │
  ├─ SI ES RESPUESTA ESTÁNDAR:
  │  │
  │  ├─ ADMIN prepara mensaje
  │  │
  │  ├─ ADMIN → TELEGRAM (CEO para aprobación)
  │  │  └─ CEO: "✅ Aprobada" O "❌ Cambiar"
  │  │
  │  ├─ SI APROBADA:
  │  │  └─ ADMIN → Evolution API → Cliente
  │  │
  │  └─ Registra en BD
  │
  ├─ SI ES TICKET TÉCNICO:
  │  │
  │  ├─ ADMIN crea ticket
  │  │
  │  ├─ ADMIN asigna a AGENTE DESARROLLO
  │  │  └─ DEV: Analiza + resuelve
  │  │
  │  └─ DEV → ADMIN: "Resuelto"
  │     └─ ADMIN: Notifica cliente
  │
  └─ SI ES URGENTE:
     └─ ADMIN escala a CEO inmediatamente

FIN: Ticket cerrado, cliente satisfecho
```

### FLUJO C: Detección de Oportunidad de Mercado

```
INICIO: AGENTE MARKETING monitorea redes + noticias
  │
  ├─ MARKETING detecta oportunidad
  │  ├─ Evento relevante
  │  ├─ Trend en redes
  │  └─ Gap en mercado
  │
  ├─ MARKETING → AGENTE GESTOR PRINCIPAL
  │  ├─ ENTRADA: "Detecté oportunidad en [sector]"
  │  ├─ Datos: Mercado, tendencia, oportunidad
  │  └─ GESTOR: Analiza viabilidad
  │
  ├─ SI GESTOR recomienda "investigar":
  │  │
  │  └─ GESTOR → LOCO
  │     ├─ ENTRADA: "¿Podríamos hacer app para [oportunidad]?"
  │     └─ LOCO: Propone estrategia
  │
  ├─ LOCO → GESTOR: Reporte técnico + oportunidad
  │
  ├─ GESTOR → CEO: Recomendación
  │
  └─ CEO decide: GO / NO-GO / INVESTIGAR MÁS

FIN: Nueva app en pipeline o descartada
```

### FLUJO D: Crisis Management

```
INICIO: AGENTE MARKETING detecta crítica negativa en redes
  │
  ├─ MARKETING → CEO (URGENTE vía Telegram)
  │  └─ Adjunta screenshot + contexto
  │
  ├─ CEO responde: "Investigar" / "Responder" / "Ignorar"
  │
  ├─ SI "INVESTIGAR":
  │  │
  │  ├─ MARKETING → AGENTE ADMINISTRACIÓN
  │  │  ├─ "¿Hay tickets relacionados de este cliente?"
  │  │  └─ ADMIN retorna contexto
  │  │
  │  ├─ MARKETING + ADMIN → CEO: Análisis completo
  │  │
  │  └─ CEO autoriza respuesta
  │
  ├─ SI "RESPONDER":
  │  │
  │  ├─ MARKETING prepara respuesta profesional
  │  │
  │  ├─ MARKETING → CEO: "Revisar respuesta"
  │  │
  │  ├─ CEO: "OK" / "Cambiar"
  │  │
  │  └─ MARKETING publica
  │
  └─ SEGUIMIENTO: ADMIN rastrea si problema se resuelve
     └─ Si persiste: Escala a GESTOR PRINCIPAL

FIN: Crisis manejada
```

### FLUJO E: Pago Atrasado / Cobranza

```
INICIO: AGENTE ADMINISTRACIÓN detecta pago > 30 días atrasado
  │
  ├─ ADMIN prepara recordatorio
  │  ├─ "Hola [Cliente], nota que aún no hemos recibido pago..."
  │  └─ Espera aprobación CEO
  │
  ├─ CEO aprueba (o modifica)
  │
  ├─ ADMIN envía recordatorio
  │
  ├─ SI NO HAY RESPUESTA EN 7 DÍAS:
  │  │
  │  └─ ADMIN escala a CEO
  │     ├─ "Cliente XYZ debe ARS 50K hace 37 días"
  │     └─ Opciones: Llamada, suspender servicio, cobrador
  │
  ├─ CEO decide acción
  │
  └─ ADMIN registra follow-up en CRM
     └─ Monitorea continuamente

FIN: Pago resuelto o escalado
```

---

## 2. MATRIZ DE COMUNICACIÓN

### Quién se comunica con quién

```
┌──────────────────┬────────────────────────────────────────────────┐
│ ORIGEN           │ DESTINO(S) + MÉTODO                            │
├──────────────────┼────────────────────────────────────────────────┤
│ CEO              │ Gestor (Telegram/WhatsApp)                     │
│                  │ Aprobaciones (órdenes críticas)                │
│                  │ Admin (cuando necesita)                        │
├──────────────────┼────────────────────────────────────────────────┤
│ Gestor Principal │ CEO (reportes, recomendaciones)                │
│                  │ Loco (coordinación técnica)                    │
│                  │ Admin (solicita datos)                         │
│                  │ Aprobaciones (valida decisiones)               │
├──────────────────┼────────────────────────────────────────────────┤
│ Loco Programación│ Gestor (propuestas, recomendaciones)           │
│                  │ Dev (órdenes técnicas)                         │
│                  │ Marketing (info de app nueva)                  │
│                  │ Admin (setup administrativo)                   │
├──────────────────┼────────────────────────────────────────────────┤
│ Administración   │ CEO (escalations urgentes)                     │
│                  │ Gestor (datos de clientes)                     │
│                  │ Marketing (leads, comentarios)                 │
│                  │ Dev (tickets técnicos)                         │
│                  │ Aprobaciones (valida transacciones)            │
├──────────────────┼────────────────────────────────────────────────┤
│ Desarrollo       │ Loco (status de infraestructura)               │
│                  │ Admin (escalations de seguridad)               │
│                  │ Aprobaciones (antes de deployments críticos)   │
├──────────────────┼────────────────────────────────────────────────┤
│ Marketing        │ Gestor (propuestas de oportunidades)           │
│                  │ Loco (info de nuevas apps)                     │
│                  │ Admin (clientes para campaigns)                │
│                  │ CEO (crisis, decisiones importantes)           │
├──────────────────┼────────────────────────────────────────────────┤
│ Aprobaciones     │ CEO (escalations críticas)                     │
│                  │ Todos (validación de acciones)                 │
└──────────────────┴────────────────────────────────────────────────┘
```

---

## 3. CANALES DE COMUNICACIÓN

### Métodos por prioridad

```
CRÍTICA (respuesta en < 5 min):
├─ Telegram → CEO
├─ WhatsApp → CEO
└─ Llamada de teléfono (si es realmente urgente)

ALTA (respuesta en < 1 hora):
├─ Telegram → Agentes
├─ Email + notificación Slack
└─ Webhook de n8n

NORMAL (respuesta en < 4 horas):
├─ Email
├─ Slack message
└─ Dashboard de n8n

BAJA (respuesta cuando sea):
├─ Reportes automáticos
├─ Notificaciones programadas
└─ Audit trail en base de datos
```

---

## 4. PROTOCOLOS DE ESCALATION

### Cuándo escala cada agente

**AGENTE ADMINISTRACIÓN escala cuando:**
```
├─ Cliente tiene problema no resolvible en SLA
├─ Transacción > ARS 10.000
├─ Cambio de contrato solicitado
├─ Pago > 30 días atrasado sin respuesta
└─ Datos sensibles comprometidos

ESCALATION → CEO o Gestor según severidad
```

**AGENTE DESARROLLO escala cuando:**
```
├─ Error crítico en producción
├─ Breach de seguridad potencial
├─ Infraestructura no disponible
├─ Necesita decisión de arquitectura
└─ Presupuesto de infraestructura excedido

ESCALATION → CEO o Loco según tipo
```

**AGENTE LOCO escala cuando:**
```
├─ Presupuesto de desarrollo > ARS 50K
├─ Timeline imposible de cumplir
├─ Cambio de dirección estratégica solicitado
├─ Blocker externo no resolvible
└─ Necesita decisión empresarial

ESCALATION → CEO o Gestor Principal
```

**AGENTE GESTOR PRINCIPAL escala cuando:**
```
├─ Decisión estratégica > su nivel
├─ Inversión > ARS 50K
├─ Cambio de dirección
├─ Partnership importante
└─ Crisis empresarial

ESCALATION → CEO + Socio
```

---

## 5. SINCRONIZACIÓN Y COORDINACIÓN

### Daily Standup (si aplica)
```
Horario: 9 AM Argentina
Duración: 15 minutos
Participantes: Loco, Dev, Marketing, Admin, Gestor
Plataforma: Slack o video call

Cada uno reporta:
- ✅ ¿Qué completé ayer?
- 🔄 ¿Qué hago hoy?
- ⚠️ ¿Qué blockers tengo?
- 🔗 ¿Necesito input de alguien?

Loco: Coordina y resuelve dependencias
```

### Weekly Sync (si aplica)
```
Horario: Viernes 4 PM Argentina
Duración: 30 minutos
Participantes: Todos + CEO

Agenda:
1. Resumen semanal por agente
2. Proyectos en progreso
3. KPIs vs targets
4. Nuevas oportunidades
5. Blockers/riesgos
6. Q&A
```

---

## 6. FORMATO DE MENSAJES ENTRE AGENTES

### Template estándar

```json
{
  "from_agent": "AGENTE_ORIGEN",
  "to_agent": "AGENTE_DESTINO",
  "priority": "CRÍTICA|ALTA|NORMAL|BAJA",
  "timestamp": "2026-03-18T14:23:00Z",
  "subject": "Descripción breve de la solicitud",
  "message": "Descripción detallada",
  "data": {
    "cliente_id": "UUID",
    "project_id": "UUID",
    "amount": 50000,
    "...": "otros datos relevantes"
  },
  "required_action": "Analizar|Ejecutar|Aprobar|Reportar",
  "deadline": "2026-03-19T09:00:00Z",
  "escalation_path": "CEO|Gestor|Loco",
  "approval_required": true|false
}
```

### Ejemplo real
```json
{
  "from_agent": "ADMINISTRACIÓN",
  "to_agent": "DESARROLLO",
  "priority": "ALTA",
  "timestamp": "2026-03-18T11:30:00Z",
  "subject": "Bug crítico en módulo de pagos",
  "message": "Cliente reportó que no puede procesar pagos mayores a ARS 100K. Error 500 en endpoint /payments/process. Ticket #1234 adjunto.",
  "data": {
    "cliente_id": "c_5847",
    "ticket_id": "t_1234",
    "endpoint": "/payments/process",
    "error_code": 500,
    "affected_users": 15,
    "revenue_impact": "alto"
  },
  "required_action": "Ejecutar",
  "deadline": "2026-03-18T14:00:00Z",
  "escalation_path": "CEO",
  "approval_required": false
}
```

---

## 7. RESUMEN RÁPIDO

| **Flujo** | **Inicio** | **Participantes** | **Duración** | **Output** |
|---|---|---|---|---|
| Nueva App | CEO | Gestor→Loco→Dev→Marketing | 4 semanas | App en venta |
| Cliente contacta | Cliente | Admin→Dev(si tech) | 5 min-2h | Ticket cerrado |
| Crisis redes | Marketing | Marketing→Admin→CEO | 10 min-1h | Respuesta publicada |
| Pago atrasado | Admin | Admin→CEO | Variable | Pago recibido |
| Oportunidad detectada | Marketing | Marketing→Gestor→Loco | 2 semanas | Decisión GO/NO-GO |

