# Guia de Instalação - Sistema de Cadastro de Itens

## Pré-requisitos

### Desenvolvimento Local
- **PHP 8.1 ou superior**
- **Composer** (gerenciador de dependências PHP)
- **Node.js 18+** e **npm**
- **MySQL 8.0** ou **MariaDB 10.9+**
- **Git** para versionamento

### Produção com Docker
- **Docker 20.10+**
- **Docker Compose 2.0+**

## Instalação para Desenvolvimento

### 1. Clonar o Repositório

```bash
git clone <url-do-repositorio>
cd sistema-cadastro-itens
```

### 2. Configurar Backend (Laravel)

```bash
cd backend

# Instalar dependências PHP
composer install

# Copiar arquivo de ambiente
cp .env.example .env

# Gerar chave da aplicação
php artisan key:generate
```

### 3. Configurar Banco de Dados

#### Opção A: MySQL/MariaDB Local

```bash
# Criar banco de dados
mysql -u root -p
CREATE DATABASE sistema_cadastro;
CREATE USER 'laravel'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON sistema_cadastro.* TO 'laravel'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### Opção B: Usar Docker apenas para o banco

```bash
docker run -d \
  --name sistema_cadastro_db \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=sistema_cadastro \
  -e MYSQL_USER=laravel \
  -e MYSQL_PASSWORD=password \
  -p 3306:3306 \
  mariadb:10.9
```

### 4. Configurar Variáveis de Ambiente

Edite o arquivo `backend/.env`:

```env
APP_NAME="Sistema de Cadastro de Itens"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sistema_cadastro
DB_USERNAME=laravel
DB_PASSWORD=password
```

### 5. Executar Migrations e Seeders

```bash
# Executar migrations
php artisan migrate

# Executar seeders (dados de teste)
php artisan db:seed

# Ou executar tudo de uma vez
php artisan migrate --seed
```

### 6. Iniciar Servidor Backend

```bash
php artisan serve --host=0.0.0.0 --port=8000
```

O backend estará disponível em: `http://localhost:8000`

### 7. Configurar Frontend (Vue 3)

```bash
cd ../frontend

# Instalar dependências
npm install

# Iniciar servidor de desenvolvimento
npm run dev
```

O frontend estará disponível em: `http://localhost:3000`

## Instalação com Docker (Recomendado para Produção)

### 1. Preparar Ambiente

```bash
# Clonar repositório
git clone <url-do-repositorio>
cd sistema-cadastro-itens

# Copiar arquivo de ambiente
cp backend/.env.example backend/.env
```

### 2. Configurar Variáveis para Docker

Edite `backend/.env`:

```env
APP_NAME="Sistema de Cadastro de Itens"
APP_ENV=production
APP_DEBUG=false
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=database
DB_PORT=3306
DB_DATABASE=sistema_cadastro
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

### 3. Executar com Docker Compose

```bash
# Construir e iniciar todos os serviços
docker-compose up -d --build

# Verificar status dos containers
docker-compose ps

# Visualizar logs
docker-compose logs -f
```

### 4. Aguardar Inicialização

O sistema pode levar até 2 minutos para inicializar completamente:

1. **MariaDB** inicia primeiro
2. **Backend** aguarda o banco e executa migrations
3. **Frontend** é servido pelo Nginx

### 5. Acessar o Sistema

- **Frontend**: http://localhost
- **Backend API**: http://localhost:8000
- **Banco de dados**: localhost:3306

## Verificação da Instalação

### Testar Backend

```bash
# Testar endpoint de departamentos
curl http://localhost:8000/api/departamentos

# Testar endpoint de itens
curl http://localhost:8000/api/itens
```

### Testar Frontend

Acesse `http://localhost:3000` (desenvolvimento) ou `http://localhost` (Docker) e verifique:

1. ✅ Dashboard carrega com estatísticas
2. ✅ Lista de departamentos funciona
3. ✅ CRUD de departamentos funciona
4. ✅ Lista de itens funciona
5. ✅ CRUD de itens funciona

## Comandos Úteis

### Desenvolvimento

```bash
# Backend
cd backend
php artisan serve                    # Iniciar servidor
php artisan migrate:fresh --seed     # Resetar banco com dados
php artisan test                     # Executar testes
./vendor/bin/phpstan analyse         # Análise de qualidade

# Frontend
cd frontend
npm run dev                          # Servidor desenvolvimento
npm run build                       # Build para produção
npm run preview                     # Preview do build
```

### Docker

```bash
# Gerenciar containers
docker-compose up -d                 # Iniciar em background
docker-compose down                  # Parar containers
docker-compose restart               # Reiniciar serviços
docker-compose logs backend          # Ver logs do backend

# Executar comandos nos containers
docker-compose exec backend php artisan migrate
docker-compose exec backend php artisan test
docker-compose exec database mysql -u laravel_user -p sistema_cadastro
```

## Solução de Problemas

### Erro de Conexão com Banco

1. Verificar se o MySQL/MariaDB está rodando
2. Confirmar credenciais no arquivo `.env`
3. Testar conexão: `php artisan tinker` → `DB::connection()->getPdo()`

### Erro de Permissões (Laravel)

```bash
cd backend
sudo chown -R $USER:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

### Frontend não Conecta com Backend

1. Verificar se backend está rodando na porta 8000
2. Confirmar configuração da API em `frontend/src/services/api.js`
3. Verificar CORS no backend

### Docker não Inicia

1. Verificar se Docker está rodando: `docker --version`
2. Verificar portas disponíveis: `netstat -tulpn | grep :80`
3. Limpar containers antigos: `docker-compose down -v`

## Próximos Passos

Após a instalação bem-sucedida:

1. 📖 Ler a documentação da API
2. 🧪 Executar os testes
3. 🔍 Analisar qualidade do código
4. 🚀 Fazer deploy em produção
5. 📊 Monitorar performance

