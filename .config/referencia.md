# Referência de Frontmatter Opencode

Referência completa dos campos suportados no frontmatter dos agentes opencode.

---

## Campos Suportados

### description

**Tipo:** `string`  
**Obrigatório:** Sim  
**Descrição:** Breve descrição do agente (1-2 frases)

```yaml
description: Expert backend architect for Node.js and Python APIs
```

---

### mode

**Tipo:** `string`  
**Valores:** `all`, `plan`, `execute`  
**Padrão:** `all`  
**Descrição:** Define o modo de operação do agente

| Valor | Comportamento |
|-------|---------------|
| `all` | Pode planejar E executar ações |
| `plan` | Apenas gera planos, não executa |
| `execute` | Executa apenas o que foi planejado |

```yaml
mode: plan
```

---

### temperature

**Tipo:** `number`  
**Intervalo:** `0` - `2`  
**Padrão:** `0`  
**Descrição:** Controla a criatividade das respostas

| Valor | Comportamento |
|-------|----------------|
| `0` | Respostas determinísticas, exatas |
| `1` | Equilíbrio creativity/precisão |
| `2` | Máxima criatividade |

```yaml
temperature: 0.7
```

---

### permission

**Tipo:** `object`  
**Obrigatório:** Sim  
**Descrição:** Define quais ferramentas o agente pode usar

```yaml
permission:
  read: allow
  write: allow
  edit: allow
  bash: allow
  grep: allow
  glob: allow
  question: allow
  task: allow
```

#### Campos de permission

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `read` | `allow` \| `deny` | Ler arquivos do projeto |
| `write` | `allow` \| `deny` | Criar novos arquivos |
| `edit` | `allow` \| `deny` | Modificar arquivos |
| `bash` | `allow` \| `deny` | Executar comandos |
| `grep` | `allow` \| `deny` | Buscar em arquivos |
| `glob` | `allow` \| `deny` | Buscar arquivos |
| `question` | `allow` \| `deny` | Fazer perguntas |
| `task` | `allow` \| `deny` | Chamar agentes |

---

## Exemplo Completo

```yaml
---
description: Senior Frontend Architect with expertise in React and Next.js
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

# Senior Frontend Architect

You are...
```

---

## Campos Não Suportados

Os seguintes campos são ignorados pelo opencode (devem ser removidos ou convertidos):

- `name` - Nome deve estar no título do arquivo
- `model` - Modelo definido pelo sistema
- `maxTokens` - Limite definido pelo sistema
- `tools` - Ferramentas definidas via habilidades
- `verbose` - Logging controlado pelo sistema
- `allow_delegation` - Usar `permission.task`
- `temperature` dentro de objetos - Usar campo principal

---

## Validação

O frontmatter é validado na carga do agente. Erros comuns:

1. **Campo ausente:** `description` e `permission` são obrigatórios
2. **Valor inválido:** `mode` deve ser `all`, `plan` ou `execute`
3. **Temperatura fora do intervalo:** `0` - `2`
4. **Permissão inválida:** Valores devem ser `allow` ou `deny`