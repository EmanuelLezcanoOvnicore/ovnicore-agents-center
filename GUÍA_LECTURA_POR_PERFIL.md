# 📖 GUÍA DE LECTURA PERSONALIZADA
## ¿Quién eres? Selecciona tu perfil

---

## 👨‍💼 PERFIL: CEO / SOCIO / DECISOR

**Tiempo disponible**: 15 minutos
**Objetivo**: Entender visión, ROI, próximos pasos

**Tu lectura en orden**:

1. **[`RESUMEN_EJECUTIVO_PARA_CEO.md`](./RESUMEN_EJECUTIVO_PARA_CEO.md)** ⭐ COMIENZA AQUÍ
   - 5 min: Qué es OVNICORE 2.0
   - Los 6 agentes en 1 página
   - Flujos operacionales simples
   - Inversión vs ROI
   - P&R

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (secciones 1-3)
   - Visión empresarial
   - Descripción de agentes
   - Flujos básicos
   - Skip: Stack tecnológico, database schema

3. **Si te interesa más**:
   - [`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md) (Flujo A-E nada más)
   - Uno de los manuales de agentes que te interese

**Tiempo total**: 15-20 minutos
**Siguiente acción**: Llamar a DevOps/CTO para Fase 1

---

## 💻 PERFIL: DevOps / Backend Engineer / CTO

**Tiempo disponible**: 2-3 horas
**Objetivo**: Entender arquitectura técnica, comenzar implementación

**Tu lectura en orden**:

1. **[`README.md`](./README.md)** (5 min)
   - Overview rápido
   - Links a secciones relevantes

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (20 min)
   - Arquitectura completa (todas las secciones)
   - Stack recomendado
   - Database schema

3. **[`integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md`](./integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md)** ⭐ CRÍTICO (90 min)
   - Evolution API setup
   - n8n workflows
   - Claude API
   - PostgreSQL schema
   - Checklist de implementación
   - **Este es tu roadmap de desarrollo**

4. **[`agents/AGENTE_DESARROLLO.md`](./agents/AGENTE_DESARROLLO.md)** (15 min)
   - Tu responsabilidad como Agente Dev
   - Métricas de éxito

5. **[`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md)** (30 min)
   - Cómo se integra con otros agentes
   - Protocolos de escalation

**Tiempo total**: 2.5-3 horas
**Siguiente acción**: Crear tickets de desarrollo basados en Guía Técnica

---

## 🎨 PERFIL: Frontend / Full Stack Developer

**Tiempo disponible**: 1-2 horas
**Objetivo**: Entender arquitectura, comenzar desarrollo de UI

**Tu lectura en orden**:

1. **[`README.md`](./README.md)** (5 min)

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (secciones 1-3, 7) (20 min)
   - Visión
   - Agentes
   - Skip: database schema completo

3. **[`integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md`](./integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md)** (secciones 2, 7) (30 min)
   - n8n integration (te pasaremos webhooks)
   - Vercel deployment
   - Skip: Evolution API, PostgreSQL detalles

4. **[`agents/AGENTE_ADMINISTRACIÓN.md`](./agents/AGENTE_ADMINISTRACIÓN.md)** (sección 2 nada más) (15 min)
   - Qué necesita la UI del Admin

5. **Si necesitas**: Stack Tech específico → contacta CTO

**Tiempo total**: 1-1.5 horas
**Siguiente acción**: Start Next.js repo con Vercel deployment

---

## 📱 PERFIL: Product Manager / Product Owner

**Tiempo disponible**: 1.5 horas
**Objetivo**: Entender cómo funciona, proponer mejoras

**Tu lectura en orden**:

1. **[`RESUMEN_EJECUTIVO_PARA_CEO.md`](./RESUMEN_EJECUTIVO_PARA_CEO.md)** (5 min)
   - Context rápido

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (20 min)
   - Agentes
   - Flujos (secciones 2)
   - Skip: Stack tecnológico, database

3. **[`agents/AGENTE_GESTOR_PRINCIPAL.md`](./agents/AGENTE_GESTOR_PRINCIPAL.md)** ⭐ COMPLETO (25 min)
   - Tu rol en OVNICORE
   - Cómo funciona el análisis de viabilidad
   - Coordinación de agentes

4. **[`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md)** (25 min)
   - Los 5 flujos principales
   - Cómo escalar problemas

5. **Otros agentes** (según necesidad):
   - [`agents/AGENTE_MARKETING.md`](./agents/AGENTE_MARKETING.md) (si marketing es prioridad)
   - [`agents/AGENTE_ADMINISTRACIÓN.md`](./agents/AGENTE_ADMINISTRACIÓN.md) (si operaciones)

**Tiempo total**: 1.5-2 horas
**Siguiente acción**: Proponer primeras 3 ideas de apps para Trimestre 1

---

## 📊 PERFIL: Data Analyst / BI Engineer

**Tiempo disponible**: 1 hora
**Objetivo**: Entender qué datos se generan, cómo reportar

**Tu lectura en orden**:

1. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (sección Estructura de BD nada más) (10 min)
   - Qué datos se capturan
   - Tablas principales

2. **[`integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md`](./integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md)** (sección 4) (10 min)
   - PostgreSQL schema completo
   - Índices recomendados

3. **[`agents/AGENTE_ADMINISTRACIÓN.md`](./agents/AGENTE_ADMINISTRACIÓN.md)** (sección 5) (15 min)
   - Qué reportes genera
   - Métricas que monitorea

4. **[`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md)** (solo formato de mensajes) (10 min)
   - JSON structure que necesitas parsear

**Tiempo total**: 45 min - 1 hora
**Siguiente acción**: Diseñar dashboards de KPIs

---

## 🎯 PERFIL: Gerente de Proyecto / Scrum Master

**Tiempo disponible**: 1-1.5 horas
**Objetivo**: Entender timeline, dependencias, blockers

**Tu lectura en orden**:

1. **[`RESUMEN_EJECUTIVO_PARA_CEO.md`](./RESUMEN_EJECUTIVO_PARA_CEO.md)** (5 min)

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (sección 9: Plan de Implementación) (10 min)
   - Fases 1-5
   - Timelines estimados

3. **[`integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md`](./integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md)** (sección 10) (20 min)
   - Checklist de implementación
   - Dependencias

4. **[`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md)** (sección 5) (15 min)
   - Daily standups
   - Weekly syncs
   - Protocolos de comunicación

5. **[`agents/AGENTE_APROBACIONES.md`](./agents/AGENTE_APROBACIONES.md)** (sección 3) (10 min)
   - Procesos de aprobación

**Tiempo total**: 1-1.5 horas
**Siguiente acción**: Crear épicas/stories en Jira basados en plan

---

## 🚀 PERFIL: Nuevo Developer / Trainee

**Tiempo disponible**: 2-3 horas
**Objetivo**: Entender el sistema completo antes de codificar

**Tu lectura en orden**:

1. **[`README.md`](./README.md)** ⭐ COMIENZA AQUÍ (10 min)

2. **[`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md)** (COMPLETO) (30 min)
   - Lee TODO esto
   - Entiende cada agente
   - Visualiza flujos

3. **[`FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md`](./FLUJOS_DE_INTEGRACIÓN_Y_COORDINACIÓN.md)** (COMPLETO) (30 min)
   - Cómo funciona todo junto
   - Templates de mensajes

4. **[`integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md`](./integrations/GUÍA_INTEGRACIÓN_TÉCNICA.md)** (30 min)
   - Cada sección
   - Entiende la pila tech completa

5. **Los 6 manuales de agentes** (selecciona los que te interesen) (60 min)
   - Al menos 2: Admin + uno más

6. **Preguntas a CTO/Lead**
   - ¿Por dónde empiezo a codificar?
   - ¿Cuál es mi primer ticket?

**Tiempo total**: 2.5-3 horas
**Siguiente acción**: Recibir asignación de ticket del CTO

---

## ❓ PREGUNTAS MÁS FRECUENTES POR PERFIL

### CEO
- P: ¿Cuándo vemos ROI? → RESUMEN_EJECUTIVO
- P: ¿Qué pasa si falla? → MAESTRO_VISIÓN (Riesgos)
- P: ¿Cómo apruebo cosas? → FLUJOS (Flujo A-E)

### DevOps
- P: ¿Qué herramientas instalo? → GUÍA_INTEGRACIÓN_TÉCNICA (sección 1-7)
- P: ¿Cuál es el timeline? → MAESTRO_VISIÓN (Plan de implementación)
- P: ¿Cómo deployaré esto? → AGENTE_DESARROLLO.md

### Product Manager
- P: ¿Cuál es el flujo de nuevas apps? → FLUJOS (Flujo A)
- P: ¿Cómo priorizo tickets? → AGENTE_GESTOR_PRINCIPAL.md
- P: ¿Dónde reporto KPIs? → AGENTE_ADMINISTRACIÓN.md (sección 5)

### Frontend Dev
- P: ¿Qué tecnología uso? → GUÍA_INTEGRACIÓN_TÉCNICA (sección 7)
- P: ¿Quién me pasa APIs? → DevOps/CTO
- P: ¿Cómo empiezo? → Next.js + Vercel

### Data Analyst
- P: ¿Qué datos tengo? → MAESTRO_VISIÓN (Database schema)
- P: ¿Qué reporto? → AGENTE_ADMINISTRACIÓN (sección 5)
- P: ¿SQL disponible? → GUÍA_INTEGRACIÓN_TÉCNICA (sección 4)

---

## 🎯 REGLA DE ORO

**Si no sabes qué leer**:
1. Comienza con [`README.md`](./README.md) (2 min)
2. Luego [`MAESTRO_VISIÓN_Y_ARQUITECTURA.md`](./MAESTRO_VISIÓN_Y_ARQUITECTURA.md) (10 min)
3. Luego el documento específico de tu rol

**Si te pierdes**:
- Consulta [`ÍNDICE_MAESTRO_OVNICORE_2.0.md`](./ÍNDICE_MAESTRO_OVNICORE_2.0.md)
- Es como el "Buscar" para todo el proyecto

---

## 📞 SOPORTE EN LECTURA

¿Te cuesta entender algo?

1. Búsqueda: Usa Ctrl+F en los archivos
2. Índice: Mira [`ÍNDICE_MAESTRO_OVNICORE_2.0.md`](./ÍNDICE_MAESTRO_OVNICORE_2.0.md)
3. Preguntas: Escala a tu líder de equipo
4. Feedback: Ayuda a mejorar la documentación

---

**¡Feliz lectura!** 🚀

*Selecciona tu perfil arriba y comienza ahora.*

