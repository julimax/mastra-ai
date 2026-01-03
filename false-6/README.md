# 👁️ FASE 6 – Observabilidad y Control

## 🎯 Objetivo de la fase

Al terminar esta fase vas a poder:

- ver qué hizo cada agente y por qué

- detectar loops, alucinaciones y retries locos

- medir costos y latencia

- auditar decisiones

- operar Mastra como un sistema productivo

---

## 1️⃣ Principio rector

- Si no lo podés observar, no lo podés confiar.

En IA esto es todavía más crítico que en microservicios.

---

## 2️⃣ Qué tenés que observar (mínimo indispensable)

### 🧠 Nivel Workflow

- workflow_name

- run_id

- start_time / end_time

- status (SUCCESS / FAILED / PARTIAL)

- total_duration_ms

- total_tokens

- total_cost_estimated

### 🔁 Nivel Step

- step_name

- step_type (agent / tool / logic)

- attempt

- duration_ms

- result (ok / fail)

- error_type (si falla)

### 🤖 Nivel Agent

- agent_name

- model

- input_size

- output_size

- tokens_prompt

- tokens_completion

- confidence (si aplica)

- output_valid (true/false)

### 🧰 Nivel Tool

- tool_name

- inputs_sanitized

- duration_ms

- exit_code / http_status

- retries

- idempotent (true/false)

---

## 3️⃣ Tracing: seguir el hilo completo

### Pensalo como OpenTelemetry mental, pero aplicado a agentes.

Cada ejecución tiene:

- trace_id (workflow)

- span_id (step / agent / tool)

Así podés responder:

- “Este PR se creó porque este agent clasificó así usando este input”

---

## 4️⃣ Logs estructurados (no texto libre)

### ❌ Mal:

```text
Agent finished successfully
```

### ✅ Bien:

```json
{
  "trace_id": "abc-123",
  "step": "classify-error",
  "agent": "ErrorClassifierAgent",
  "result": "BUILD",
  "confidence": 0.91,
  "duration_ms": 842
}
```

📌 Tip DevOps: logs JSON-first, texto solo para humans.

---

## 5️⃣ Métricas que sí importan (las otras no)

### 📊 Métricas clave

- Tokens por workflow

- Tokens por agent

- Costo por run

- Latencia por step

- Ratio de retries

- Ratio de outputs inválidos

- Loops detectados

### 🚨 Alertas útiles

- Más de N retries en un step

- Token usage anormal

- Output inválido consecutivo

- Workflow > X minutos

- Mismo fix aplicado 2 veces

---

## 6️⃣ Control de loops y runaway agents 😱

### Señales de loop

- Mismo agent + mismo output

- Retry sin cambio de input

- Tokens creciendo en cada intento

- Tiempo total disparado

### Guardrails obligatorios

- Max retries por step

- Max tokens por workflow

- Max duration por run

- “Circuit breaker” global

### 📌 Regla:

Un workflow debe poder abortarse solo.

---

## 7️⃣ Cost control (esto es FinOps + AI)

### Estrategias sanas

- Presupuesto por workflow

- Presupuesto por repo / team

- Model downgrade automático (fallback)

- Cache de resultados (cuando aplica)

### Ejemplo:

- Clasificación → modelo chico

- Planificación → modelo medio

- Redacción PR → modelo grande (solo si llegó ahí)

---

## 8️⃣ Auditoría y explicabilidad (clave para empresas)

### Tenés que poder responder:

- ¿Quién ejecutó esto?

- ¿Con qué inputs?

- ¿Qué agente decidió?

- ¿Qué evidencia usó?

- ¿Qué cambió realmente?

### 📌 Esto es lo que te permite vender Mastra en empresas.

---

## 9️⃣ Mini-lab FASE 6 (muy práctico)

### Lab 1 — Diseño de observabilidad

Para un workflow ci-fixflow:

- lista de métricas

- logs por step

- alertas mínimas

### Lab 2 — Loop detection rule

Definí:

- qué es “loop”

- cómo se detecta

- qué acción toma el workflow

### Lab 3 — Cost budget

Definí:

- tokens max por run

- fallback de modelo

- abort condition