# Sistema de Cadastro de Itens por Departamento

Sistema completo para gerenciamento de itens organizados por departamentos, desenvolvido com Laravel 11 (back-end) e Vue 3 (front-end).

<img width="1064" height="772" alt="image" src="https://github.com/user-attachments/assets/1b564460-03e5-4846-8abc-b47684817dd9" />


## 🚀 Tecnologias Utilizadas

### Back-end

- **PHP 8.1+**

- **Laravel 11** - Framework PHP

- **MySQL/MariaDB** - Banco de dados

- **PHPUnit** - Testes unitários

- **PHPStan/Larastan** - Análise estática de código (nível 7)

### Front-end

- **Vue 3** - Framework JavaScript

- **Vite** - Build tool

- **Axios** - Cliente HTTP

- **CSS3** - Estilização responsiva

### DevOps

- **Docker** - Containerização

- **Docker Compose** - Orquestração de containers

- **Nginx** - Servidor web para produção

## 📋 Funcionalidades

### CRUD Departamentos

- ✅ Criar departamentos

- ✅ Listar departamentos

- ✅ Editar departamentos

- ✅ Excluir departamentos

- ✅ Ativar/Desativar departamentos

### CRUD Itens

- ✅ Criar itens associados a departamentos

- ✅ Listar itens com informações do departamento

- ✅ Editar itens

- ✅ Excluir itens

- ✅ Controle de estoque (quantidade)

- ✅ Controle de preços

- ✅ Ativar/Desativar itens

### Dashboard

- ✅ Estatísticas gerais do sistema

- ✅ Total de departamentos e itens

- ✅ Valor total em estoque

- ✅ Itens ativos

## 🛠️ Instalação e Configuração

### Pré-requisitos

- PHP 8.1 ou superior

- Composer

- Node.js 18+ e npm

- MySQL/MariaDB

- Docker e Docker Compose (opcional)

### Instalação Local

#### 1. Clone o repositório

```bash
git clone <url-do-repositorio>
cd sistema-cadastro-itens
```

#### 2. Configuração do Back-end

```bash
cd backend

# Instalar dependências
composer install

# Copiar arquivo de ambiente
cp .env.example .env

# Gerar chave da aplicação
php artisan key:generate

# Configurar banco de dados no .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sistema_cadastro
DB_USERNAME=seu_usuario
DB_PASSWORD=sua_senha

# Executar migrations e seeders
php artisan migrate --seed

# Iniciar servidor
php artisan serve
```

#### 3. Configuração do Front-end

```bash
cd ../frontend

# Instalar dependências
npm install

# Iniciar servidor de desenvolvimento
npm run dev
```

### Instalação com Docker

#### 1. Configurar variáveis de ambiente

```bash
# Copiar arquivo de ambiente do backend
cp backend/.env.example backend/.env

# Editar as configurações do banco para Docker
DB_HOST=database
DB_DATABASE=sistema_cadastro
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

#### 2. Executar com Docker Compose

```bash
# Construir e iniciar containers
docker-compose up -d --build

# Aguardar inicialização completa (cerca de 2 minutos)
# O sistema estará disponível em:
# - Frontend: http://localhost
# - Backend API: http://localhost:8000
```

## 📚 Documentação da API

### Base URL

```
http://localhost:8000/api
```

### Endpoints Departamentos

#### Listar Departamentos

```
GET /api/departamentos
```

**Resposta:**

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "nome": "Tecnologia da Informação",
      "descricao": "Departamento de TI",
      "ativo": true,
      "created_at": "2025-08-09T14:00:00.000000Z",
      "updated_at": "2025-08-09T14:00:00.000000Z",
      "itens": []
    }
  ]
}
```

#### Criar Departamento

```
POST /api/departamentos
Content-Type: application/json

{
  "nome": "Recursos Humanos",
  "descricao": "Departamento de RH",
  "ativo": true
}
```

#### Buscar Departamento

```
GET /api/departamentos/{id}
```

#### Atualizar Departamento

```
PUT /api/departamentos/{id}
Content-Type: application/json

{
  "nome": "Recursos Humanos Atualizado",
  "descricao": "Nova descrição",
  "ativo": false
}
```

#### Excluir Departamento

```
DELETE /api/departamentos/{id}
```

### Endpoints Itens

#### Listar Itens

```
GET /api/itens
```

**Resposta:**

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
        "nome": "Tecnologia da Informação"
      }
    }
  ]
}
```

#### Criar Item

```
POST /api/itens
Content-Type: application/json

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

#### Buscar Item

```
GET /api/itens/{id}
```

#### Atualizar Item

```
PUT /api/itens/{id}
Content-Type: application/json

{
  "nome": "Monitor LG 24\" 4K",
  "preco": 1200.00,
  "quantidade": 3
}
```

#### Excluir Item

```
DELETE /api/itens/{id}
```

#### Buscar Itens por Departamento

```
GET /api/departamentos/{id}/itens
```

## 🧪 Testes

### Executar Testes Unitários

```bash
cd backend

# Executar todos os testes
php artisan test

# Executar apenas testes unitários
php artisan test --testsuite=Unit

# Executar com cobertura (se configurado)
php artisan test --coverage
```

### Análise de Qualidade de Código

```bash
cd backend

# Executar PHPStan (nível 7)
./vendor/bin/phpstan analyse

# Verificar padrões PSR
./vendor/bin/phpcs --standard=PSR12 app/
```

## 📁 Estrutura do Projeto

```
sistema-cadastro-itens/
├── backend/                 # API Laravel
│   ├── app/
│   │   ├── Http/Controllers/Api/
│   │   │   ├── DepartamentoController.php
│   │   │   └── ItemController.php
│   │   └── Models/
│   │       ├── Departamento.php
│   │       └── Item.php
│   ├── database/
│   │   ├── factories/
│   │   ├── migrations/
│   │   └── seeders/
│   ├── tests/
│   │   ├── Feature/
│   │   └── Unit/
│   ├── Dockerfile
│   └── phpstan.neon
├── frontend/               # Interface Vue 3
│   ├── src/
│   │   ├── components/
│   │   ├── views/
│   │   │   ├── DashboardView.vue
│   │   │   ├── DepartamentosView.vue
│   │   │   └── ItensView.vue
│   │   ├── services/
│   │   │   └── api.js
│   │   └── router/
│   ├── Dockerfile
│   └── nginx.conf
├── docker-compose.yml
└── README.md
```

## 🔧 Configurações de Desenvolvimento

### Variáveis de Ambiente (Backend)

```
APP_NAME="Sistema de Cadastro de Itens"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sistema_cadastro
DB_USERNAME=root
DB_PASSWORD=
```

### Configuração da API (Frontend)

```javascript
// src/services/api.js
const baseURL = import.meta.env.VITE_API_URL || 'http://localhost:8000/api'
```

## 🚀 Deploy

### Usando Docker

```bash
# Produção
docker-compose -f docker-compose.prod.yml up -d --build
```

### Deploy Manual

1. Configure o servidor web (Apache/Nginx)

1. Configure o banco de dados

1. Execute as migrations

1. Build do frontend: `npm run build`

1. Configure as variáveis de ambiente

## 🤝 Contribuição

1. Fork o projeto

1. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)

1. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)

1. Push para a branch (`git push origin feature/AmazingFeature`)

1. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 👥 Autores

- **Wlamir Lima**[](https://github.com/wlamirlima)

