# 🧠 FASE 1 – Fundamentos de Mastra AI

## 🎯 Objetivo de la fase

### Que al terminar esta fase:

- Entiendas cómo piensa Mastra

- Sepas qué problema resuelve

- Puedas leer un proyecto Mastra y no perderte

- Tengas tu primer workflow ejecutable

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

2️⃣ Modelo mental de Mastra (esto es CLAVE)

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
![Agente RAG workflow](fase1/images/agentic-RAG-workflow.jpg)
