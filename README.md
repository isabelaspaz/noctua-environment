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

## Execução do Projeto

A execução do ambiente pode ser realizada de duas formas principais:

### 1. Utilizando Docker Compose

O método recomendado para inicialização dos serviços é através do Docker Compose, que garante independência de ambiente e praticidade na orquestração dos módulos.

No diretório raiz do projeto, execute:

```bash
docker compose up --build
```

Este comando irá construir as imagens necessárias e iniciar os serviços definidos no arquivo `docker-compose.yml`.

### 2. Execução Local (Desenvolvimento)

Alternativamente, é possível executar os serviços separadamente em ambiente local, conforme os requisitos de cada módulo:

- **Backend (Spring Boot):**
  
  Navegue até o diretório do backend e execute utilizando Maven:

  ```bash
  cd source/noctua-backend
  mvn spring-boot:run
  ```

- **Frontend (React):**

  Navegue até o diretório do frontend e execute utilizando Node.js:

  ```bash
  cd source/noctua-frontend
  npm install
  npm run dev
  ```
  
> **Nota:** Caso queira pode usar os comandos de `RUN` da IDE de sua preferência. Esse processo de rodar o projeto usando o Maven e o Node.js são mascarados.

## Considerações Finais

Este ambiente foi projetado com enfoque acadêmico e modularidade, visando facilitar a evolução dos serviços, testes isolados e implantação contínua. Recomenda-se a utilização do Docker para maior praticidade e portabilidade entre diferentes ambientes de desenvolvimento.
