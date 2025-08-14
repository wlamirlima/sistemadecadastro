# Documentação da API - Sistema de Cadastro de Itens

## Visão Geral

Esta API RESTful foi desenvolvida em Laravel 11 e fornece endpoints para gerenciar departamentos e itens em um sistema de cadastro.

**Base URL:** `http://localhost:8000/api`

**Formato de Resposta:** JSON

**Autenticação:** Não requerida (sistema básico)

## Estrutura de Resposta Padrão

### Sucesso
```json
{
  "success": true,
  "data": {...},
  "message": "Operação realizada com sucesso"
}
```

### Erro
```json
{
  "success": false,
  "message": "Mensagem de erro",
  "errors": {...}
}
```

## Endpoints de Departamentos

### 1. Listar Todos os Departamentos

**GET** `/api/departamentos`

**Descrição:** Retorna lista de todos os departamentos com seus itens.

**Parâmetros:** Nenhum

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "nome": "Tecnologia da Informação",
      "descricao": "Departamento responsável pela infraestrutura de TI",
      "ativo": true,
      "created_at": "2025-08-09T14:00:00.000000Z",
      "updated_at": "2025-08-09T14:00:00.000000Z",
      "itens": [
        {
          "id": 1,
          "nome": "Notebook Dell",
          "codigo": "TI001",
          "preco": "2500.00",
          "quantidade": 10
        }
      ]
    }
  ]
}
```

### 2. Buscar Departamento por ID

**GET** `/api/departamentos/{id}`

**Descrição:** Retorna um departamento específico com seus itens.

**Parâmetros:**
- `id` (integer, obrigatório): ID do departamento

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "nome": "Tecnologia da Informação",
    "descricao": "Departamento responsável pela infraestrutura de TI",
    "ativo": true,
    "created_at": "2025-08-09T14:00:00.000000Z",
    "updated_at": "2025-08-09T14:00:00.000000Z",
    "itens": [...]
  }
}
```

**Resposta de Erro (404):**
```json
{
  "success": false,
  "message": "Departamento não encontrado"
}
```

### 3. Criar Novo Departamento

**POST** `/api/departamentos`

**Descrição:** Cria um novo departamento.

**Headers:**
```
Content-Type: application/json
Accept: application/json
```

**Body:**
```json
{
  "nome": "Recursos Humanos",
  "descricao": "Departamento de gestão de pessoas",
  "ativo": true
}
```

**Campos:**
- `nome` (string, obrigatório, max: 255): Nome do departamento
- `descricao` (string, opcional): Descrição do departamento
- `ativo` (boolean, opcional, padrão: true): Status do departamento

**Resposta de Sucesso (201):**
```json
{
  "success": true,
  "data": {
    "id": 2,
    "nome": "Recursos Humanos",
    "descricao": "Departamento de gestão de pessoas",
    "ativo": true,
    "created_at": "2025-08-09T15:00:00.000000Z",
    "updated_at": "2025-08-09T15:00:00.000000Z"
  },
  "message": "Departamento criado com sucesso"
}
```

**Resposta de Erro (422):**
```json
{
  "success": false,
  "message": "Dados inválidos",
  "errors": {
    "nome": ["O campo nome é obrigatório."]
  }
}
```

### 4. Atualizar Departamento

**PUT** `/api/departamentos/{id}`

**Descrição:** Atualiza um departamento existente.

**Parâmetros:**
- `id` (integer, obrigatório): ID do departamento

**Body:** Mesmo formato do POST, todos os campos opcionais

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "nome": "TI - Atualizado",
    "descricao": "Nova descrição",
    "ativo": true,
    "created_at": "2025-08-09T14:00:00.000000Z",
    "updated_at": "2025-08-09T15:30:00.000000Z"
  },
  "message": "Departamento atualizado com sucesso"
}
```

### 5. Excluir Departamento

**DELETE** `/api/departamentos/{id}`

**Descrição:** Exclui um departamento e todos os seus itens.

**Parâmetros:**
- `id` (integer, obrigatório): ID do departamento

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "message": "Departamento excluído com sucesso"
}
```

**Resposta de Erro (404):**
```json
{
  "success": false,
  "message": "Departamento não encontrado"
}
```

### 6. Listar Itens de um Departamento

**GET** `/api/departamentos/{id}/itens`

**Descrição:** Retorna todos os itens de um departamento específico.

**Parâmetros:**
- `id` (integer, obrigatório): ID do departamento

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "nome": "Notebook Dell",
      "codigo": "TI001",
      "descricao": "Notebook para desenvolvimento",
      "preco": "2500.00",
      "quantidade": 10,
      "departamento_id": 1,
      "ativo": true,
      "created_at": "2025-08-09T14:00:00.000000Z",
      "updated_at": "2025-08-09T14:00:00.000000Z"
    }
  ]
}
```

## Endpoints de Itens

### 1. Listar Todos os Itens

**GET** `/api/itens`

**Descrição:** Retorna lista de todos os itens com informações do departamento.

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "nome": "Notebook Dell",
      "codigo": "TI001",
      "descricao": "Notebook para desenvolvimento",
      "preco": "2500.00",
      "quantidade": 10,
      "departamento_id": 1,
      "ativo": true,
      "created_at": "2025-08-09T14:00:00.000000Z",
      "updated_at": "2025-08-09T14:00:00.000000Z",
      "departamento": {
        "id": 1,
        "nome": "Tecnologia da Informação",
        "descricao": "Departamento de TI",
        "ativo": true
      }
    }
  ]
}
```

### 2. Buscar Item por ID

**GET** `/api/itens/{id}`

**Descrição:** Retorna um item específico com informações do departamento.

**Parâmetros:**
- `id` (integer, obrigatório): ID do item

**Resposta:** Mesmo formato do endpoint de listagem, mas retorna apenas um item.

### 3. Criar Novo Item

**POST** `/api/itens`

**Descrição:** Cria um novo item associado a um departamento.

**Body:**
```json
{
  "nome": "Monitor LG 24\"",
  "codigo": "TI002",
  "descricao": "Monitor para desenvolvimento",
  "preco": 800.00,
  "quantidade": 5,
  "departamento_id": 1,
  "ativo": true
}
```

**Campos:**
- `nome` (string, obrigatório, max: 255): Nome do item
- `codigo` (string, obrigatório, único, max: 50): Código identificador
- `descricao` (string, opcional): Descrição detalhada
- `preco` (decimal, obrigatório, min: 0): Preço unitário
- `quantidade` (integer, obrigatório, min: 0): Quantidade em estoque
- `departamento_id` (integer, obrigatório): ID do departamento
- `ativo` (boolean, opcional, padrão: true): Status do item

**Resposta de Sucesso (201):**
```json
{
  "success": true,
  "data": {
    "id": 2,
    "nome": "Monitor LG 24\"",
    "codigo": "TI002",
    "descricao": "Monitor para desenvolvimento",
    "preco": "800.00",
    "quantidade": 5,
    "departamento_id": 1,
    "ativo": true,
    "created_at": "2025-08-09T15:00:00.000000Z",
    "updated_at": "2025-08-09T15:00:00.000000Z",
    "departamento": {
      "id": 1,
      "nome": "Tecnologia da Informação"
    }
  },
  "message": "Item criado com sucesso"
}
```

**Resposta de Erro (422):**
```json
{
  "success": false,
  "message": "Dados inválidos",
  "errors": {
    "nome": ["O campo nome é obrigatório."],
    "codigo": ["O código já está em uso."],
    "departamento_id": ["O departamento selecionado é inválido."]
  }
}
```

### 4. Atualizar Item

**PUT** `/api/itens/{id}`

**Descrição:** Atualiza um item existente.

**Parâmetros:**
- `id` (integer, obrigatório): ID do item

**Body:** Mesmo formato do POST, todos os campos opcionais

### 5. Excluir Item

**DELETE** `/api/itens/{id}`

**Descrição:** Exclui um item do sistema.

**Parâmetros:**
- `id` (integer, obrigatório): ID do item

**Resposta de Sucesso (200):**
```json
{
  "success": true,
  "message": "Item excluído com sucesso"
}
```

## Códigos de Status HTTP

- **200 OK**: Operação realizada com sucesso
- **201 Created**: Recurso criado com sucesso
- **404 Not Found**: Recurso não encontrado
- **422 Unprocessable Entity**: Dados de entrada inválidos
- **500 Internal Server Error**: Erro interno do servidor

## Exemplos de Uso

### Criar um Departamento e Adicionar Itens

```bash
# 1. Criar departamento
curl -X POST http://localhost:8000/api/departamentos \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Almoxarifado",
    "descricao": "Controle de estoque geral",
    "ativo": true
  }'

# 2. Criar item no departamento (assumindo ID 3)
curl -X POST http://localhost:8000/api/itens \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Papel A4",
    "codigo": "ALM001",
    "descricao": "Resma de papel A4 500 folhas",
    "preco": 25.90,
    "quantidade": 100,
    "departamento_id": 3,
    "ativo": true
  }'
```

### Buscar Itens de um Departamento

```bash
curl -X GET http://localhost:8000/api/departamentos/1/itens
```

### Atualizar Quantidade de um Item

```bash
curl -X PUT http://localhost:8000/api/itens/1 \
  -H "Content-Type: application/json" \
  -d '{
    "quantidade": 15
  }'
```

## Validações

### Departamento
- **nome**: obrigatório, string, máximo 255 caracteres, único
- **descricao**: opcional, texto
- **ativo**: opcional, boolean, padrão true

### Item
- **nome**: obrigatório, string, máximo 255 caracteres
- **codigo**: obrigatório, string, máximo 50 caracteres, único
- **descricao**: opcional, texto
- **preco**: obrigatório, decimal (10,2), mínimo 0
- **quantidade**: obrigatório, integer, mínimo 0
- **departamento_id**: obrigatório, deve existir na tabela departamentos
- **ativo**: opcional, boolean, padrão true

## Relacionamentos

- Um **Departamento** pode ter muitos **Itens**
- Um **Item** pertence a um **Departamento**
- Exclusão de departamento remove todos os itens associados (CASCADE)

## Considerações Técnicas

### Performance
- Relacionamentos são carregados automaticamente (eager loading)
- Queries otimizadas para evitar N+1 problems
- Índices configurados em campos de busca frequente

### Segurança
- Validação de entrada em todos os endpoints
- Sanitização de dados antes da persistência
- Tratamento adequado de erros sem exposição de informações sensíveis

### Padrões
- Segue padrões PSR-12 para código PHP
- Implementa padrões RESTful para endpoints
- Usa Conventional Commits para versionamento

