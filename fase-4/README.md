# 🧰 FASE 4 – Tools (conectar Mastra al mundo real)

## 🎯 Objetivo de la fase

Al terminar, vas a poder diseñar tools que sean:

- seguras (no destructivas por accidente)

- confiables (outputs consistentes)

- testeables (mocks / fixtures)

- observables (logs + métricas)

- idempotentes (re-ejecutables sin romper)

---

## 1) Qué es una Tool (en serio)

Una Tool NO es “cualquier función”.

Una Tool es un adaptador determinista que:

- recibe inputs validados

- ejecuta una acción (API/CLI/DB/FS)

- devuelve output estructurado y confiable

- falla con errores tipados

📌 Regla de oro:

- El LLM propone; la Tool ejecuta con reglas.

---

## 2) Tipos de Tools (las 4 que vas a usar en AI-Ops)

### A) HTTP / API Tools

- GitHub REST/GraphQL

- Jira

- Slack/Teams

- SonarQube

- ArgoCD

Riesgos: rate limits, auth, payloads raros.

### B) CLI Tools

- kubectl

- terraform

- helm

- git

- gh

Riesgos: comandos peligrosos, parsing frágil, permisos del runner.

### C) Data Tools

- Postgres / DynamoDB

- Vector DB (pgvector / qdrant / etc.)

Riesgos: queries no sanitizadas, latencia, consistencia.

### D) File System Tools

- leer logs / artifacts

- modificar archivos del repo (para PR auto-fix)

Riesgos: path traversal, cambios no deseados, diffs gigantes.

---

## 3) Contrato de Tool (input/output) — lo más importante

### ✅ Inputs

- Tipados

- Validados (schema)

- Default seguros

Ejemplo conceptual:

- namespace obligatorio

- selector opcional

- timeout con límite max

### ✅ Outputs

- Estructurados

- Estables

- Con metadata útil

Ejemplo:

```json
{
  "ok": true,
  "items": [{"name": "pod-1", "status": "Running"}],
  "raw": "...",
  "warnings": []
}
```
---

## 4) Seguridad: “guardrails” obligatorios (modo DevOps)

### 🔐 1) Allowlist de acciones

No aceptes “cualquier comando”.

- ✅ kubectl get, kubectl describe

- ✅ terraform plan

- ❌ kubectl delete

- ❌ terraform apply (a menos que haya aprobaciones explícitas)

### 🧨 2) Modo “dry-run” por defecto

- primero simular

- luego ejecutar si el workflow lo autoriza

### 🪪 3) Permisos mínimos

- service accounts con RBAC limitado

- tokens con scopes mínimos

- separar lectura/escritura

### ⏱️ 4) Timeouts + límites

- evitar cuelgues

- evitar loops

- rate limit controlado

---

## 5) Idempotencia (clave para retries)

Como el workflow puede hacer retry, tu tool debe ser re-ejecutable.

Ejemplos:

- “crear issue si no existe” (busca primero)

- “comentar en PR” con comment_id o “si ya comenté, update”

- “aplicar patch” verificando si ya está aplicado

📌 Regla:

Retry sin idempotencia = duplicados / caos.

---

## 6) Observabilidad: cada Tool tiene que contar lo que hizo

Mínimo:

- tool_name

- inputs (sin secretos)

- duration_ms

- ok/fail

- error_type

- correlation_id (workflow run id)

Esto te deja auditar: “qué se ejecutó y por qué”.

---

## 7) Testing de Tools (sin dolor)

- Unit tests con mocks (HTTP, FS)

- Fixtures de outputs (logs reales)

- “Golden files” para diffs

Estrategia muy DevOps:

Tool real en integración (staging)

Tool mock en unit

---

✅ Checklist para dar por cerrada FASE 4

- Definís input/output contract claro

- Tenés allowlist / guardrails

- Tu tool es idempotente

- Tenés timeouts y límites

- Logueás y podés auditar