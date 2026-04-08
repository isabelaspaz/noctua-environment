# Noctua Environment

Este projeto visa estabelecer um ambiente de desenvolvimento integrado para aplicações full-stack, contemplando um backend (Spring Boot) e um frontend (React). O repositório principal organiza e orquestra esses módulos de forma modular, facilitando o desenvolvimento, testes e implantação.

## Clonagem e Configuração do Repositório

1. **Clone o repositório principal em seu diretório de preferência:**

   ```bash
   git clone https://github.com/vitornsc7/noctua-environment.git
   ```

2. **Acesse o diretório recém-criado:**

   ```bash
   cd noctua-environment
   ```

3. **Clone os submódulos responsáveis pelo backend e frontend no diretório `source/`:**

   ```bash
   git clone https://github.com/vitornsc7/noctua-backend.git source/noctua-backend
   git clone https://github.com/vitornsc7/noctua-frontend.git source/noctua-frontend
   ```

## Configuração de Variáveis de Ambiente

Antes de executar o projeto pela primeira vez, copie o arquivo de exemplo e preencha os valores:

```bash
cp .env.example .env
```

O arquivo `.env` contém as credenciais do banco de dados e a chave JWT utilizadas pelos serviços.

## Execução do Projeto

O ambiente pode ser executado de duas formas:

### 1. Produção — Docker Compose completo

Sobe todos os serviços (banco, backend e frontend) de forma integrada.

Na primeira execução ou após alterações nos `Dockerfile`s:

```bash
docker compose up --build
```

Nas demais execuções, sem necessidade de rebuild:

```bash
docker compose up
```

Para reconstruir apenas um serviço específico:

```bash
docker compose up --build noctua-backend
```

> O `Dockerfile` do backend usa cache de camadas: as dependências Maven só são baixadas novamente se o `pom.xml` mudar. Alterações apenas no código-fonte são muito mais rápidas.

### 2. Desenvolvimento — Docker Compose parcial + Vite local (recomendado)

Sobe somente o banco de dados e o backend. O frontend é executado localmente com hot-reload instantâneo via Vite, sem necessidade de rebuild Docker a cada alteração.

**Terminal 1 — banco + backend:**

```bash
docker compose -f docker-compose.dev.yml up
```

**Terminal 2 — frontend:**

```bash
cd source/noctua-frontend
npm install   # apenas na primeira vez
npm run dev
```

O Vite está configurado para redirecionar automaticamente as chamadas `/api/*` para o backend em `http://localhost:8080`, sem necessidade de configurações adicionais.

Alterações nos arquivos Java dentro de `source/noctua-backend/src/` são detectadas pelo Spring Boot DevTools e reiniciam o servidor automaticamente.

## Considerações Finais

Este ambiente foi projetado com enfoque acadêmico e modularidade, visando facilitar a evolução dos serviços, testes isolados e implantação contínua. Recomenda-se o fluxo de desenvolvimento parcial (opção 2) para o dia a dia, reservando o Docker Compose completo para validação final ou deploy.
