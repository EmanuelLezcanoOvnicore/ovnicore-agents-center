# ⚙️ GUÍA INTEGRACIÓN TÉCNICA - OVNICORE 2.0

## 1. EVOLUTION API (WhatsApp)

### ¿Por qué Evolution API?
- ✅ Open-source y gratuito
- ✅ No depende de WhatsApp Official API (menor costo)
- ✅ Fácil integración con n8n
- ✅ Soporta Baileys + métodos nativos
- ✅ Comunidad activa argentina

### Setup básico

**Opción A: Self-hosted (Recomendado para OVNICORE)**
```bash
# 1. Clone repo
git clone https://github.com/EvolutionAPI/evolution-api.git
cd evolution-api

# 2. Environment variables
cat > .env <<EOF
EVOLUTION_DB_URL=postgresql://user:pass@localhost/evolution
EVOLUTION_API_KEY=tu_api_key_secreto
EVOLUTION_API_PORT=3000
EVOLUTION_JWT_SECRET=jwt_secret_fuerte
EOF

# 3. Docker compose (recomendado)
docker-compose up -d

# 4. Verifica que esté corriendo
curl http://localhost:3000/health
```

**Opción B: Cloud (Más fácil, costo mínimo)**
```
Plataformas recomendadas:
- Railway.app (USD 5-10/mes)
- Render.com (USD 10-15/mes)
- Heroku (USD 50/mes, pero variable)

Elige Railway para más control.
```

### Integración con n8n

```
n8n Workflow:
1. Webhook: POST /webhook/whatsapp
   ├─ Recibe mensaje de Evolution API
   └─ Parsea: [cliente_id, contenido, timestamp]

2. Claude API call
   ├─ Input: mensaje del cliente
   └─ Output: análisis + respuesta

3. Approval node
   ├─ Espera: CEO aprobación en Telegram
   └─ Si rechaza: skip, si aprueba: continua

4. Send WhatsApp
   ├─ Evolution API: sendMessage()
   └─ Cliente recibe respuesta

5. Database
   ├─ Registra: conversación en PostgreSQL
   └─ Crea: ticket si es necesario
```

**Configuración n8n**:
```json
{
  "name": "WhatsApp Client Handler",
  "nodes": [
    {
      "name": "Webhook WhatsApp",
      "type": "webhook",
      "typeVersion": 1,
      "position": [100, 300],
      "parameters": {
        "path": "webhook/whatsapp",
        "method": "POST",
        "responseMode": "onReceived"
      }
    },
    {
      "name": "Claude API",
      "type": "httpRequest",
      "parameters": {
        "url": "https://api.anthropic.com/v1/messages",
        "method": "POST",
        "headers": {
          "x-api-key": "{{$env.ANTHROPIC_API_KEY}}"
        },
        "body": {
          "model": "claude-3-5-sonnet-20241022",
          "messages": [
            {
              "role": "user",
              "content": "{{$node['Webhook WhatsApp'].json.body.message}}"
            }
          ],
          "max_tokens": 1024
        }
      }
    },
    {
      "name": "Approval",
      "type": "manualTrigger",
      "parameters": {
        "message": "CEO debe aprobar respuesta"
      }
    },
    {
      "name": "Evolution Send",
      "type": "httpRequest",
      "parameters": {
        "url": "http://localhost:3000/message/sendText",
        "method": "POST",
        "headers": {
          "Content-Type": "application/json",
          "Authorization": "Bearer {{$env.EVOLUTION_API_KEY}}"
        },
        "body": {
          "number": "{{$node['Webhook WhatsApp'].json.body.from}}",
          "textMessage": {
            "text": "{{$node['Claude API'].json.content[0].text}}"
          }
        }
      }
    }
  ]
}
```

### Testing Evolution API
```bash
# Test de envío simple
curl -X POST http://localhost:3000/message/sendText \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer tu_api_key" \
  -d '{
    "number": "549876543210",
    "textMessage": {
      "text": "Hola desde OVNICORE!"
    }
  }'

# Obtener instancias conectadas
curl http://localhost:3000/instance/fetchInstances \
  -H "Authorization: Bearer tu_api_key"
```

---

## 2. n8n (ORQUESTACIÓN CENTRAL)

### Setup

**Instalación local (desarrollo)**:
```bash
npm install -g n8n
n8n start
# Accede en http://localhost:5678
```

**Instalación Docker (producción)**:
```dockerfile
# docker-compose.yml
version: '3'
services:
  n8n:
    image: n8nio/n8n:latest
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=${DB_PASSWORD}
      - DB_POSTGRESDB_DATABASE=n8n
      - WEBHOOK_URL=https://yourdomain.com/
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    environment:
      POSTGRES_USER: n8n
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  n8n_data:
  postgres_data:
```

### Flujos principales n8n

**Flujo 1: WhatsApp → Claude → Response + Approval**
```
Trigger: Webhook POST /whatsapp
  ↓
Parse message
  ↓
Claude API call (análisis)
  ↓
If confidence > 80%:
  ├─ Send to client
  └─ Log in DB
Else:
  ├─ Wait CEO approval (Telegram)
  ├─ If approved: send
  └─ If rejected: store draft
```

**Flujo 2: Daily Reports**
```
Cron: 9 AM Argentina
  ↓
DB query: tickets, métricas
  ↓
Claude API: generar report
  ↓
Email to CEO + Slack notification
```

**Flujo 3: Invoice Generation**
```
Trigger: Contrato activo vencido
  ↓
Validate: cliente, monto
  ↓
Claude API: generar factura PDF
  ↓
Approval required if > ARS 5.000
  ↓
Send to client + DB record
```

### Webhooks n8n principales
```
POST /webhook/whatsapp          ← Evolution API
POST /webhook/instagram         ← Meta APIs
POST /webhook/email             ← Gmail
POST /webhook/payment           ← Stripe/MercadoPago
GET  /webhook/daily-report      ← Cron job
```

---

## 3. CLAUDE API (INTELIGENCIA)

### Configuración

**Autenticación**:
```bash
# Guardar en variables de ambiente
export ANTHROPIC_API_KEY="sk-ant-..."
```

**Setup Node.js**:
```javascript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY
});

// Uso básico
const response = await client.messages.create({
  model: "claude-3-5-sonnet-20241022",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: "Analiza este mensaje de cliente..."
    }
  ]
});

console.log(response.content[0].text);
```

### Modelos recomendados por agente

```
AGENTE GESTOR PRINCIPAL:
  Model: claude-3-5-opus-20250805 (ó claude-4-5-opus si disponible)
  Costo: $15 / 1M input, $75 / 1M output
  Razón: Mejor razonamiento, análisis complejo

AGENTE ADMINISTRACIÓN:
  Model: claude-3-5-sonnet-20241022
  Costo: $3 / 1M input, $15 / 1M output
  Razón: Balance velocidad/costo

AGENTE DESARROLLO:
  Model: claude-3-5-sonnet-20241022
  Razón: Análisis técnico

AGENTE MARKETING:
  Model: claude-3-5-sonnet-20241022 + Vision
  Razón: Generación de contenido + análisis imágenes

AGENTE APROBACIONES:
  Model: claude-3-5-sonnet-20241022
  Razón: Análisis rápido

AGENTE LOCO:
  Model: claude-3-5-opus-20250805
  Razón: Mejor para ideación y planificación
```

### Prompts especializados

**Prompt: Agente Administración (Análisis de mensaje)**
```
Eres AGENTE ADMINISTRACIÓN de OVNICORE. Tu tarea es analizar
mensajes de clientes y decidir qué acción tomar.

Contexto:
- Empresa: OVNICORE (automatización IA)
- Clientes: PyMEs Argentina
- Canales: WhatsApp, Instagram, Email

Mensaje recibido:
{{MENSAJE_CLIENTE}}

Analiza el mensaje y:
1. Categoriza el tipo (SOPORTE, VENTA, FACTURA, OTRO)
2. Detecta urgencia (CRÍTICA, ALTA, NORMAL, BAJA)
3. Sugiere respuesta (si es FAQ)
4. Propone ticket si es necesario
5. Estima confianza de autorespuesta (0-100%)

Formato de salida:
{
  "categoria": "...",
  "urgencia": "...",
  "confianza": XX,
  "respuesta_sugerida": "...",
  "necesita_ticket": true/false,
  "escalacion_necesaria": true/false
}
```

---

## 4. POSTGRESQL (BASE DE DATOS)

### Setup inicial

```sql
-- Crear BD
CREATE DATABASE ovnicore;

-- Tabla de clientes
CREATE TABLE clientes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  nombre VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE,
  whatsapp VARCHAR(20),
  empresa VARCHAR(255),
  perfil_riesgo VARCHAR(20),
  valor_total_vida DECIMAL(10,2),
  fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_whatsapp (whatsapp)
);

-- Tabla de conversaciones
CREATE TABLE conversaciones (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID REFERENCES clientes(id),
  canal VARCHAR(50), -- 'whatsapp', 'email', 'instagram'
  contenido TEXT,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  agente_responsable VARCHAR(50),
  sentimiento VARCHAR(20), -- 'positivo', 'negativo', 'neutral'
  tags JSONB,
  estado VARCHAR(20), -- 'abierto', 'cerrado'
  INDEX idx_cliente (cliente_id),
  INDEX idx_canal (canal)
);

-- Tabla de tickets
CREATE TABLE tickets (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID REFERENCES clientes(id),
  conversacion_id UUID REFERENCES conversaciones(id),
  prioridad VARCHAR(20), -- 'crítica', 'alta', 'normal', 'baja'
  categoria VARCHAR(100),
  descripcion TEXT,
  fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  fecha_resolucion TIMESTAMP,
  agente_asignado VARCHAR(50),
  aprobador VARCHAR(50),
  estado VARCHAR(20), -- 'abierto', 'en_progreso', 'resuelto', 'cerrado'
  sla_vencimiento TIMESTAMP,
  INDEX idx_cliente (cliente_id),
  INDEX idx_estado (estado)
);

-- Tabla de contratos
CREATE TABLE contratos (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID REFERENCES clientes(id),
  tipo VARCHAR(100),
  valor_ars DECIMAL(10,2),
  fecha_inicio DATE,
  fecha_vencimiento DATE,
  terminos_clave JSONB,
  archivo_pdf_url VARCHAR(500),
  status VARCHAR(20), -- 'activo', 'vencido', 'renovación'
  aprobador VARCHAR(100),
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de aprobaciones (audit trail)
CREATE TABLE aprobaciones (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tipo_accion VARCHAR(100),
  recurso_id UUID, -- referencia a ticket, contrato, etc
  propuesto_por_agente VARCHAR(50),
  aprobado_por_humano VARCHAR(100),
  resultado VARCHAR(20), -- 'aprobado', 'rechazado'
  motivo_rechazo TEXT,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_recurso (recurso_id)
);

-- Tabla de auditoría (logs de agentes)
CREATE TABLE auditoria_agentes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  agente_id VARCHAR(50),
  accion VARCHAR(100),
  parametros JSONB,
  resultado VARCHAR(20), -- 'exitoso', 'error'
  aprobacion_id UUID REFERENCES aprobaciones(id),
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_agente (agente_id),
  INDEX idx_timestamp (timestamp)
);

-- Crear índices para performance
CREATE INDEX idx_conversaciones_timestamp ON conversaciones(timestamp DESC);
CREATE INDEX idx_tickets_estado_prioridad ON tickets(estado, prioridad);
CREATE INDEX idx_clientes_perfil ON clientes(perfil_riesgo);
```

### Backups

```bash
# Backup diario a S3
0 2 * * * pg_dump -Fc ovnicore | aws s3 cp - s3://ovnicore-backups/db_$(date +%Y%m%d).sql.gz
```

---

## 5. REDIS (CACHE Y SESIONES)

### Setup
```bash
docker run -d -p 6379:6379 redis:7-alpine
```

### Uso en n8n
```javascript
// Cache de respuestas FAQ
const redis = new Redis({
  host: 'localhost',
  port: 6379
});

// Almacenar
await redis.setex(`faq:${pregunta_hash}`, 86400, respuesta);

// Recuperar
const respuesta_cached = await redis.get(`faq:${pregunta_hash}`);
```

---

## 6. AWS S3 (ARCHIVOS Y BACKUPS)

### Configuración

```javascript
import AWS from 'aws-sdk';

const s3 = new AWS.S3({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: 'sa-east-1' // São Paulo (más cercano a Argentina)
});

// Upload PDF factura
const params = {
  Bucket: 'ovnicore-factures',
  Key: `facturas/${cliente_id}/${factura_id}.pdf`,
  Body: pdfBuffer,
  ContentType: 'application/pdf',
  ServerSideEncryption: 'AES256'
};

await s3.upload(params).promise();
```

---

## 7. VERCEL (HOSTING FRONTEND)

### Deployment automático

```bash
# 1. Conectar repo GitHub
vercel link

# 2. Setup environment variables
vercel env add ANTHROPIC_API_KEY
vercel env add NEXT_PUBLIC_API_URL

# 3. Deploy
vercel deploy --prod
```

### Configuración vercel.json
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "env": {
    "ANTHROPIC_API_KEY": "@anthropic_api_key"
  },
  "functions": {
    "api/**/*.ts": {
      "memory": 512,
      "maxDuration": 30
    }
  }
}
```

---

## 8. DIAGRAMA DE FLUJO TÉCNICO COMPLETO

```
┌─────────────────┐
│    Cliente      │
│   WhatsApp      │
└────────┬────────┘
         │
         ▼
┌──────────────────────┐
│  Evolution API       │
│  (Self-hosted)       │
└─────────┬────────────┘
          │
          ▼
┌──────────────────────┐
│    n8n (Central)     │
│  (Webhook receiver)  │
└─────────┬────────────┘
          │
          ├─────────────────────────┐
          │                         │
          ▼                         ▼
    ┌──────────────┐      ┌──────────────────┐
    │ Claude API   │      │   PostgreSQL     │
    │ (Analysis)   │      │   (Store data)   │
    └──────┬───────┘      └──────────────────┘
           │
           ▼
    ┌──────────────┐
    │ Approval     │
    │ (CEO Telegram)
    └──────┬───────┘
           │
           ▼
    ┌──────────────────────┐
    │ Send response        │
    │ (Evolution API)      │
    └──────────────────────┘
```

---

## 9. VARIABLES DE AMBIENTE (Referencia)

```bash
# Anthropic
ANTHROPIC_API_KEY=sk-ant-...

# Evolution API
EVOLUTION_API_URL=http://localhost:3000
EVOLUTION_API_KEY=tu_api_key

# n8n
N8N_HOST=n8n.tudominio.com
N8N_PORT=5678

# PostgreSQL
DB_HOST=localhost
DB_PORT=5432
DB_USER=ovnicore
DB_PASSWORD=contraseña_fuerte
DB_NAME=ovnicore

# Redis
REDIS_URL=redis://localhost:6379

# AWS
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_REGION=sa-east-1

# Vercel
VERCEL_TOKEN=...

# Meta (Instagram/WhatsApp Official API - si lo usas)
META_ACCESS_TOKEN=...
META_BUSINESS_ACCOUNT_ID=...

# Telegram (aprobaciones CEO)
TELEGRAM_BOT_TOKEN=...
TELEGRAM_CEO_CHAT_ID=...
```

---

## 10. CHECKLIST DE IMPLEMENTACIÓN

- [ ] Setup PostgreSQL local/cloud
- [ ] Setup Evolution API
- [ ] Setup n8n
- [ ] Crear primer webhook WhatsApp
- [ ] Conectar Claude API
- [ ] Setup S3 para backups
- [ ] Configurar Redis
- [ ] Deploy frontend Vercel
- [ ] Crear usuario CEO en n8n
- [ ] Configurar Telegram bot
- [ ] Testing end-to-end: Cliente → Response
- [ ] Implementar approval flow
- [ ] Crear reportes automáticos
- [ ] Setup monitoreo (Datadog/Sentry)
- [ ] Go live! 🚀

