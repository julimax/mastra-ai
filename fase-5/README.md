# 🧠 FASE 5 – State, Context y Memory

## 🎯 Objetivo de la fase

Que puedas diseñar agentes/workflows que:

- no repitan cosas

- no alucinen por falta de contexto

- sean reproducibles

- sean auditables

- puedan escalar (multi-step, multi-agent, multi-run)

---

## 1) Diferencia CLAVE (muchos lo mezclan)

### 🧩 Context

Es lo que le pasás al agente/step en ese momento.

- input del usuario

- logs

- metadata

- resumen del state

👉 Es transitorio y “por ejecución”.

### 🧩 State

Es la memoria del workflow (la verdad del sistema).

- outputs de steps

- decisiones tomadas

- retries/attempts

- ids (PR, issue, branch, comment)

👉 Es estructurado, controlado por el workflow.

### 🧩 Memory

Persistencia entre ejecuciones (cross-run).

- historial de incidentes

- embeddings / RAG

- preferencias

- conocimiento acumulado

👉 Es “largo plazo”.

---

## 2) La regla más importante: “source of truth”

El State es la fuente de verdad, no el LLM.

El LLM puede:

- interpretar

- clasificar

- proponer

Pero el estado oficial lo guarda el workflow.

---

## 3) Qué va en State (y qué NO)

✅ State debe guardar

- run_id, repo, branch

- attempts por step

- classification (output del agent)

- selected_strategy

- tool_results

IDs reales:

- pr_number

- comment_id

- issue_id

- commit_sha

- diff_summary / files_changed

- timestamps

- cost/tokens (si medís)

❌ No guardes en State

- tokens/secrets

- logs completos enormes (guardá referencia/ubicación)

- “texto creativo”

-  prompts gigantes

📌 Tip DevOps: logs pesados → artifact store / S3 / GCS, y en state solo artifact_url.

---

## 4) Diseñar el State: schema simple y evolutivo

Ejemplo (conceptual) para Fixflow:

```json
{
  "input": {
    "repo": "org/app",
    "workflow_run_id": "12345",
    "log_artifact_url": "s3://.../logs.txt"
  },
  "analysis": {
    "error_type": "BUILD",
    "confidence": 0.86,
    "reason": "Gradle dependency not found"
  },
  "plan": {
    "strategy": "UPDATE_REPO_URL",
    "steps": ["edit gradle", "run tests", "commit"]
  },
  "execution": {
    "attempts": { "classify": 1, "apply_patch": 2 },
    "tool_results": {
      "git_diff": "...",
      "tests": { "ok": true, "report_url": "..." }
    }
  },
  "output": {
    "pr_number": 77,
    "pr_url": "https://...",
    "status": "SUCCESS"
  }
}
```
---

## 5) Context: cómo pasarlo sin romperte

### Problema típico

Si metés TODO el state como context → te explota el prompt, tokens y confusión.

### Solución

Pasar solo lo necesario, idealmente en 3 capas:

- Input actual

- Resumen del state relevante (pocas líneas)

- Artefactos puntuales (diff, log snippet)

### 📌 Pattern recomendado:

- State completo vive en el workflow

- El agent ve un “view model” reducido

---

## 6) Memory: cuándo realmente vale la pena

### ✅ Usá memory cuando:

- querés “aprender” de ejecuciones previas

- querés evitar fixes repetidos

- querés sugerencias basadas en historial

- querés RAG con docs internas (runbooks, ADRs, tickets)

### ❌ No la uses si:

- el problema es resoluble con state/context actual

- te suma complejidad sin valor

- necesitás determinismo fuerte

---

## 7) Patrones prácticos (nivel producción)

### ✅ Pattern A: “Decision Ledger”

Cada decisión importante queda registrada:

- “por qué branch tomó”

- “qué estrategia eligió”

- “qué evidencia usó”

Útil para auditoría y debugging.

### ✅ Pattern B: “Artifact pointers”

En vez de logs enormes:

guardás artifact_url + line_range + hash

### ✅ Pattern C: “Memory con retrieval controlado”

RAG sí, pero:

- top-k pequeño

- fuentes allowlisted

- citar qué se usó (internamente)

---

## 8) Mini-lab FASE 5 (para que lo sientas)

### Lab 1 — Definí tu State schema

Para un workflow: ci-failure-handler

- input

- analysis

- plan

- execution

- output

### Lab 2 — Diseñá el Context view

Qué le pasás a:

- ErrorClassifierAgent

- FixPlanAgent

- PRSummaryAgent

### Lab 3 — Definí una Memory mínima

Ejemplo:

- “soluciones previas por repo + tipo de error”

- “runbooks indexados”