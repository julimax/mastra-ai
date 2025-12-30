# 🧠 FASE 2 – Agents en profundidad (bien hechos)
## 🎯 Objetivo de la fase

### Al terminar esta fase deberías poder:

- Diseñar agentes con responsabilidad única

- Evitar agentes “verborrágicos”

- Hacer agentes predecibles, evaluables y reutilizables

- Usarlos como componentes de sistema, no como UI

---

## 1️⃣ Agent ≠ Chatbot (idea clave)

###Un chatbot:

- Habla mucho

- Decide todo

- No tiene contrato

- Es difícil de testear

### Un Agent en Mastra:

- Cumple una función

- Tiene límites claros

- Produce output estructurado

- Se evalúa como software

### 💡 Pensalo como:

- un microservicio cognitivo

---

## 2️⃣ Anatomía de un Agent en Mastra

### Todo Agent bien diseñado tiene:

### 🧩 1. Rol

- Qué es y qué no es.

- “Sos un clasificador de errores de CI/CD. No solucionás nada.”

### 🧩 2. Instrucciones

- Reglas claras y restrictivas.

- Qué hacer

- Qué no hacer

- Cómo responder

Ejemplo mental:

- “Respondé solo con JSON”

- “No expliques”

- “No inventes”

### 🧩 3. Input contract

Qué recibe:

- Texto

- Logs

- Contexto previo

- Metadata

👉 Siempre limitado.

### 🧩 4. Output contract

Qué devuelve.

- Tipado

- Validable

- Predecible

Ejemplo:

```json
{
  "category": "BUILD",
  "confidence": 0.87
}
```

---

## 3️⃣ Responsabilidad ÚNICA (regla de oro)

❌ Mal agent:

- “Analiza logs, decide qué hacer, arregla el repo y avisa en Slack”

✅ Bien:

- Agent A: clasifica error

- Agent B: propone solución

- Agent C: redacta PR message

👉 Los agentes se componen, no se inflan.

---

## 4️⃣ Agentes deterministas vs cognitivos

|Tipo	|Usa LLM	|Ejemplo|
|---|---|---|
|Cognitivo	|✅	|Clasificar, resumir, razonar|
|Determinista	|❌	|Validar JSON, parsear logs|
|Híbrido	|⚠️	|Decide + valida|

💡 Tip DevOps:
Siempre validador determinista después del LLM.

---

## 5️⃣ Prompt ≠ Instructions (esto confunde mucho)

- Prompt: texto que ve el LLM

- Instructions: contrato del Agent

Mastra favorece:

- Instructions fuertes

- Prompt mínimo

- Output estricto

👉 Menos “poesía”, más ingeniería.

---

## 6️⃣ Ejemplo conceptual: ErrorClassifierAgent


### 🧠 Rol

- Clasificar errores de pipelines CI/CD.

### 📥 Input

- logText

- pipelineName

### 📤 Output

```json
{
  "type": "BUILD | INFRA | TEST | UNKNOWN",
  "reason": "string breve",
  "confidence": 0.0 - 1.0
}
```

### ❌ No hace

- No arregla

- No ejecuta comandos

- No decide flujo

---

## 7️⃣ Anti-patterns comunes (evitalos)

🚫 Agent todólogo

🚫 Output en texto libre

🚫 Instrucciones vagas

🚫 “Si no sabés, inventá”

🚫 Agente con acceso directo a tools peligrosas

---

## 8️⃣ Primer ejercicio práctico (mental + diseño)

Diseñá en papel o texto este agent:

- Agent: PRSummaryAgent

Definí:

- Rol

- Input

- Output

- Límites

- Instrucciones 

⚠️ No código todavía. Diseño primero.