# Conversor de Frontmatter para Opencode

Guia para converter frontmatter de outros formatos (Vercel AI SDK, LangChain, CrewAI, etc.) para o formato opencode.

## Formato Opencode

```yaml
---
description: Breve descrição do agente
mode: all | plan | execute
temperature: 0-2
permission:
  read: allow | deny
  write: allow | deny
  edit: allow | deny
  bash: allow | deny
  grep: allow | deny
  glob: allow | deny
  question: allow | deny
  task: allow | deny
---
```

---

## Vercel AI SDK

**De:**
```yaml
name: Coding Assistant
description: An expert programmer
config:
  maxTokens: 2048
  temperature: 0.7
```

**Para:**
```yaml
---
description: An expert programmer
mode: all
temperature: 0.7
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
---
```

---

## LangChain / LangGraph

**De:**
```yaml
name: sql-agent
role: Data Analyst
description: Expert in SQL and database queries
tools:
  - sql_executor
  - schema_viewer
configuration:
  model: gpt-4
  temperature: 0
```

**Para:**
```yaml
---
description: Expert in SQL and database queries
mode: all
temperature: 0
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
---
```

---

## CrewAI

**De:**
```yaml
role: Research Agent
goal: Gather accurate information
backstory: |
  You are an expert researcher
  with years of experience
verbose: true
allow_delegation: true
```

**Para:**
```yaml
---
description: Expert researcher gather accurate information
mode: all
temperature: 0
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
---
```

---

## AutoGen

**De:**
```yaml
name: coding-agent
description: A helpful coding assistant
model_config:
  temperature: 0.5
  max_tokens: 2000
code_execution_config:
  work_dir: ./execution
  use_docker: true
```

**Para:**
```yaml
---
description: A helpful coding assistant
mode: all
temperature: 0.5
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
---
```

---

## Formato Genérico / Personalizado

**De:**
```yaml
agent:
  name: Backend Developer
  type: expert
  expertise:
    - Node.js
    - Python
    - API Design
  capabilities:
    code_generation: true
    code_review: true
```

**Para:**
```yaml
---
description: Expert in Node.js, Python, and API Design
mode: all
temperature: 0
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
---
```

---

## Regras de Conversão

| Campo Original | Campo Opencode | Notas |
|----------------|----------------|-------|
| `name` | `--` | Nome vai no título do arquivo |
| `description` | `description` | Mantém |
| `role` | `description` | Converte para descrição |
| `goal` | `description` | Append ao final |
| `backstory` | `--` | Conteúdo vai no corpo do arquivo |
| `temperature` | `temperature` | Mantém |
| `model` | `--` | Não suportado |
| `maxTokens` / `max_tokens` | `--` | Não suportado |
| `tools` | `--` | Ferramentas definidas via habilidades |
| `verbose` | `--` | Não suportado |
| `allow_delegation` | `task` | `allow` → `allow`, `false` → `deny` |
| `code_execution_config` | `bash` | `true` → `allow` |
| `work_dir` | `--` | Não suportado |
| `use_docker` | `--` | Não suportado |

---

## Modo (mode)

| Valor | Comportamento |
|-------|----------------|
| `all` | Pode executar ações e criar código |
| `plan` | Apenas planeja, não executa |
| `execute` | Apenas executa o que foi planejado |

---

## Permissões (permission)

| Ferramenta | Quando usar |
|------------|--------------|
| `read` | Ler arquivos do projeto |
| `write` | Criar novos arquivos |
| `edit` | Modificar arquivos existentes |
| `bash` | Executar comandos no terminal |
| `grep` | Buscar conteúdo em arquivos |
| `glob` | Buscar arquivos por padrão |
| `question` | Fazer perguntas ao usuário |
| `task` | Chamar outros agentes |