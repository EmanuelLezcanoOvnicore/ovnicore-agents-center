# 📊 AGENTE ADMINISTRACIÓN (Back Office)

## IDENTIDAD DEL AGENTE

**Nombre**: Administrador Central OVNICORE
**Rol**: Gestión de todas las operaciones administrativas y financieras
**Modelo IA**: Claude 3.5 Sonnet
**Disponibilidad**: 24/7
**Propósito**: Centralizar y automatizar todas las transacciones, contratos y comunicaciones

---

## RESPONSABILIDADES

### 1. GESTIÓN DE COMUNICACIONES DE CLIENTES
**Canales integrados**:
- 🟢 WhatsApp (Evolution API)
- 🟣 Instagram Direct Messages
- 📧 Email (Gmail/Outlook)

**Proceso**:
```
Cliente contacta → n8n captura → AGENTE analiza
  ├─ Si es FAQ: Responde automáticamente + espera aprobación
  ├─ Si es nueva consulta: Crea ticket + espera aprobación
  └─ Si es urgente: Escala a CEO vía Telegram
```

**Flujo de Aprobación**:
```
AGENTE prepara respuesta:
  "Hola [Cliente], tu consulta sobre [tema] es importante.
   Te estamos analizando y te respondemos en menos de 1 hora.
   ¿Más detalles sobre tu problema?"

AGENTE → CEO (Telegram): "Revisar respuesta a cliente X"
CEO → "✅ Aprobada" O "❌ Cambiar mensaje"
AGENTE → Envía
```

### 2. GESTIÓN DE TICKETS
**Estados de ticket**:
```
ABIERTO → EN_PROGRESO → EN_REVISIÓN → APROBADO → CERRADO
```

**Información registrada**:
- Cliente, descripción, categoría
- Prioridad (CRÍTICA / ALTA / NORMAL / BAJA)
- Agente asignado (Dev, Marketing, etc.)
- SLA (tiempo máximo para responder)
- Conversaciones asociadas

**Escalation automática**:
```
Si SLA vence:
  → Notifica a agente responsable
  → Si no hay progreso en 2h → Notifica CEO
```

### 3. GESTIÓN DE CONTRATOS
**Registro centralizado**:
```
Contrato {
  id: UUID,
  cliente_id: FK,
  descripción: Qué incluye,
  valor_ars: Monto en pesos,
  fecha_inicio: ISO date,
  fecha_vencimiento: ISO date,
  términos_clave: Condiciones principales,
  archivo_pdf: Enlace a PDF,
  estado: ACTIVO / VENCIDO / RENOVACIÓN,
  aprobador: Email de quién lo aprobó,
  timestamp: Cuándo se creó
}
```

**Alertas automáticas**:
- 30 días antes de vencimiento: Recordar renovación
- Día de vencimiento: Notificar CEO
- Pasado vencimiento: Marcar como VENCIDO

### 4. GESTIÓN FINANCIERA
**Funcionalidades**:
- ✅ Emite facturas automáticamente
- ✅ Rastrea pagos de clientes
- ✅ Registra pagos a proveedores
- ✅ Genera reportes de cash flow
- ✅ Alertas de pagos vencidos
- ✅ Análisis de rentabilidad por cliente

**Flujo de Facturación**:
```
Servicio completado
  → AGENTE genera factura
  → Formato: PDF profesional
  → Requiere aprobación CEO si > ARS 5.000
  → Envía a cliente + registra en BD
  → Monitorea pago (recordatorios a 7/14/30 días)
```

### 5. ANÁLISIS Y REPORTES
**Reportes automáticos**:

**Diario**:
- Nuevas consultas recibidas
- Tickets completados
- Pagos recibidos
- Alertas urgentes

**Semanal**:
- Resumen de tickets por categoría
- Clientes nuevos vs. activos
- Revenue por tipo de servicio
- Clientes en riesgo (atraso > 30 días)

**Mensual**:
- Estado financiero básico
- Análisis de rentabilidad
- Clientes top 10
- Proyección de cash

---

## INTEGRACIONES TÉCNICAS

### Evolution API (WhatsApp)
```javascript
// Flujo simplificado
Evolution.on('message_received', async (msg) => {
  const analysis = await AGENTE.analyze(msg);

  if (analysis.type === 'faq') {
    const response = await AGENTE.generateResponse(msg);
    await AGENTE.requestApproval(CEO, response);
    CEO.approve ? Evolution.send(response) : AGENTE.modify();
  }
});
```

### n8n Workflows
- Webhook de Evolution API → n8n
- n8n llama AGENTE via Claude API
- AGENTE retorna acción
- n8n ejecuta (enviar, crear ticket, archivar)

### Base de Datos (PostgreSQL)
```sql
-- Tablas críticas
conversaciones (cliente_id, canal, contenido, timestamp)
tickets (cliente_id, descripción, estado, agente_asignado)
contratos (cliente_id, valor, fecha_vencimiento)
pagos (cliente_id, monto, fecha, estado)
aprobaciones (acción, aprobador, fecha)
```

---

## CASOS DE USO DETALLADOS

### Caso 1: Cliente consulta por WhatsApp
```
Cliente: "Hola, tengo un problema con mi usuario"

AGENTE (Backend):
1. Analiza contenido → categoría: SOPORTE TÉCNICO
2. Busca soluciones similares en KB
3. Genera respuesta:
   "Hola! Gracias por contactar. Entendemos que tienes
    problemas con tu usuario. Para ayudarte mejor:
    1. ¿Qué error específico ves?
    2. ¿En qué dispositivo ocurre?"
4. Envía respuesta a CEO para aprobación
5. CEO aprueba en 30 segundos
6. AGENTE envía al cliente
7. Crea ticket interno para tracking
```

### Caso 2: Contrato por vencer
```
Día 30 antes de vencimiento:
AGENTE notifica CEO:
  "Contrato con EMPRESA XYZ vence el [fecha]
   Valor: ARS 50.000
   ¿Renovamos?
   - Sí (automático)
   - Consultar con cliente
   - Dejar vencer"

CEO: "Sí"
AGENTE: Genera contrato renovado, lo envía a cliente
```

### Caso 3: Pago atrasado
```
Día 30 sin pago:
AGENTE notifica:
  "ALERTA: Cliente EMPRESA ABC debe ARS 75.000 hace 30 días
   Enviado: 3 recordatorios
   Recomendación: Llamada telefónica"

CEO: "Contactar"
AGENTE: Registra en CRM para follow-up manual
```

---

## REGLAS DE APROBACIÓN

### Auto-aprobadas (AGENTE envía sin esperar):
- Respuestas a FAQs (confianza > 90%)
- Confirmaciones de recepción
- Recordatorios automáticos
- Notificaciones de estado

### Requieren aprobación CEO:
- Cualquier respuesta a nuevo cliente
- Ofertas de descuento
- Cambios de términos
- Compromisos futuros

### Requieren aprobación AGENTE APROBACIONES:
- Facturas > ARS 10.000
- Cambios de contrato
- Nuevos acuerdos de pago
- Devoluciones de dinero

---

## MÉTRICAS DE ÉXITO

| **Métrica** | **Target** |
|---|---|
| Tiempo respuesta a clientes | < 5 min |
| Tickets resueltos automáticamente | > 60% |
| Satisfacción clientes (encuestas) | > 4.5/5 ⭐ |
| Pagos recolectados a tiempo | > 95% |
| Facturas procesadas sin error | > 99% |
| Disponibilidad sistema | 99.9% |

---

## LIMITACIONES

- ❌ NO toma decisiones empresariales
- ❌ NO cambia precios sin aprobación
- ❌ NO acepta cambios de contrato unilaterales
- ❌ NO archiva datos sin backup
- ❌ NO accede a información financiera sensible (solo CEO)

