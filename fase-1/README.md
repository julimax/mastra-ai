# 🧠 FASE 1 – Fundamentos de Mastra AI

## 🎯 Objetivo de la fase

### Que al terminar esta fase:

- Entiendas cómo piensa Mastra

- Sepas qué problema resuelve

- Puedas leer un proyecto Mastra y no perderte

- Tengas tu primer workflow ejecutable

---

## 1️⃣ El problema que Mastra viene a resolver

### Antes de Mastra, en sistemas LLM suele pasar esto:

- Prompts gigantes

- Lógica mezclada con texto

- Flujos frágiles

- Difícil retry

- Difícil observabilidad

- Imposible escalar a producción

### Mastra propone:

#### Separar inteligencia (LLM) de control (software)

#### 💡 El LLM no decide el flujo, solo aporta razonamiento.

---

## 2️⃣ Modelo mental de Mastra (esto es CLAVE)

Pensá Mastra así:

```vbnet
Workflow
 ├── Step
 │    └── Agent
 │         └── LLM
 ├── Step
 │    └── Tool
 └── Step
      └── Lógica determinística
```
![Agente RAG workflow](images/agentic-RAG-workflow.jpg)
![Header](images/header.png)
![Sequencial chains](images/sequential-chains.webp)

### 👉 Reglas de oro

- El workflow manda

- El agent razona

- La tool ejecuta

- El estado es explícito

---

## 3️⃣ Conceptos fundamentales (uno por uno)

### 🧩 Workflow

- Es el orquestador.

- Define pasos

- Controla el orden

- Maneja errores

- Decide retries

### 👉 Similar a:

- GitHub Actions

- Temporal

- Argo Workflows

### 🧩 Step

- Un paso atómico del workflow.

- Tipos comunes:

- Step que llama a un Agent

- Step que ejecuta una Tool

- Step puramente lógico

👉 Un step no piensa, ejecuta.

### 🧩 Agent

- Un worker cognitivo.

- Tiene un rol

- Tiene instrucciones claras

- Recibe input

- Devuelve output estructurado

❌ NO es un chatbot
✅ Es un componente de sistema

### 🧩 Tool

- Puente con el mundo real.

- Función bien definida

- Inputs validados

- Output confiable

### 👉 Ejemplos:

- API call

- CLI

- DB query

- File system

### 🧩 Context / State

- La memoria del sistema.

- State del workflow

- Inputs / outputs de steps

- Decisiones previas

👉 Nada implícito, todo visible.

---

## 4️⃣ Determinismo vs LLM (muy importante)

|Parte	| Determinista	| Probabilística|
|---|---|---|
|Control de flujo	| ✅	| ❌
|Retries	| ✅	| ❌
|Decisiones críticas	| ❌	| ✅
|Lógica de negocio	| ✅	| ❌
|Razonamiento	| ❌	| ✅

### 💡 Nunca:

- Dejes al LLM decidir si borrar algo

- Ejecutar infra

- Hacer cambios sin validación

---

## 5️⃣ Primer ejercicio mental (sin código)

### 👉 Caso simple:

“Quiero analizar un texto y decir si es un error de build o de infra”

### Cómo lo piensa Mastra:

1. Workflow recibe texto

2. Step 1 → Agent clasifica error

3. Step 2 → Switch según resultado

4. Step 3 → Output final

### 💡 El LLM no controla el flujo, solo clasifica.

---

## 6️⃣ Primera práctica mínima (pseudo-código)

No importa el lenguaje todavía, importa el concepto.

```ts
workflow "analyze-error" {
  step "classify" {
    agent: ErrorClassifierAgent
    input: logText
  }

  step "route" {
    if output.type == "BUILD" -> buildHandler
    if output.type == "INFRA" -> infraHandler
  }
}
```
### 👉 Esto no es chat, es software.

---

