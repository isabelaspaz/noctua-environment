# Noctua — Sistema de Gestão e Acompanhamento Acadêmico

O **Noctua** é uma aplicação web desenvolvida como Trabalho de Conclusão de Curso (TCC) em Análise e Desenvolvimento de Sistemas, com o objetivo de facilitar a gestão acadêmica e o acompanhamento do desempenho estudantil.

A plataforma centraliza informações sobre alunos, turmas, avaliações, notas e frequência, permitindo que professores acompanhem o progresso acadêmico e identifiquem estudantes que necessitam de atenção pedagógica.

Um dos principais diferenciais do Noctua é a **classificação automatizada do desempenho**, que combina indicadores de notas e frequência para auxiliar na identificação de situações de risco acadêmico e apoiar decisões pedagógicas.

O projeto foi desenvolvido em equipe, contemplando frontend, backend, banco de dados, autenticação e infraestrutura de execução.

## Funcionalidades

- **Gestão de turmas e alunos:** cadastro, organização e acompanhamento de informações acadêmicas.
- **Avaliações e notas:** registro de avaliações e acompanhamento das médias dos estudantes.
- **Controle de frequência:** registro de faltas e monitoramento da frequência escolar.
- **Classificação automatizada:** categorização do desempenho acadêmico conforme critérios de notas e frequência.
- **Identificação de risco acadêmico:** apoio à identificação de estudantes que podem necessitar de intervenção pedagógica.
- **Autenticação e controle de acesso:** gerenciamento de usuários com perfis de administrador e professor.
- **Autenticação em dois fatores (2FA):** camada adicional de segurança com suporte a aplicativos autenticadores.
- **Recuperação de senha:** fluxo de redefinição de credenciais por e-mail.

## Tecnologias utilizadas

| Camada | Tecnologias |
|---|---|
| Frontend | React, Vite, Tailwind CSS |
| Backend | Java, Spring Boot, Spring Security |
| Banco de dados | MySQL 8 |
| Autenticação | JWT e TOTP (2FA) |
| Infraestrutura | Docker, Docker Compose, Nginx |
| Deploy original | AWS EC2, Amazon Aurora, GitHub Actions |
| Testes | JUnit, banco H2 em memória |

## Arquitetura e organização do projeto

O Noctua utiliza uma arquitetura modular, com separação entre interface, API e infraestrutura.

O frontend é responsável pela experiência do usuário e pela comunicação com a API REST. O backend concentra as regras de negócio, a autenticação e a persistência de informações acadêmicas.

O banco de dados MySQL armazena as informações utilizadas pela aplicação, enquanto o Docker Compose facilita a configuração e execução dos serviços.

### Repositórios

O código-fonte está organizado em três repositórios:

- **[Noctua Frontend](https://github.com/isabelaspaz/noctua-frontend)** — Interface web desenvolvida com React e Vite.
- **[Noctua Backend](https://github.com/isabelaspaz/noctua-backend)** — API REST, regras de negócio, autenticação e persistência de dados.
- **[Noctua Environment](https://github.com/isabelaspaz/noctua-environment)** — Configuração dos ambientes, integração dos serviços e infraestrutura Docker.

Este repositório (`noctua-environment`) concentra os arquivos necessários à organização e orquestração da aplicação.

## Como executar o projeto localmente

### Pré-requisitos

Para executar o Noctua em ambiente de desenvolvimento, é necessário ter instalado:

- Git
- Docker e Docker Compose
- Node.js e npm

### 1. Clonar o repositório principal

```bash
git clone https://github.com/isabelaspaz/noctua-environment.git
```

Acesse o diretório:

```bash
cd noctua-environment
```

### 2. Clonar o frontend e o backend

Os módulos são mantidos em repositórios independentes e devem ser clonados dentro do diretório `source/`.

```bash
git clone https://github.com/isabelaspaz/noctua-backend.git source/noctua-backend
git clone https://github.com/isabelaspaz/noctua-frontend.git source/noctua-frontend
```

A estrutura resultante será:

```text
noctua-environment/
├── .github/
│   └── workflows/
├── source/
│   ├── noctua-backend/
│   └── noctua-frontend/
├── .env.dev.example
├── .env.example
├── docker-compose.dev.yml
└── docker-compose.yml
```

### 3. Configurar as variáveis de ambiente

Para o ambiente de desenvolvimento, copie o arquivo de exemplo:

```bash
cp .env.dev.example .env
```

Edite o `.env` e configure as variáveis necessárias, incluindo:

- `DB_URL`: endereço de conexão com o MySQL.
- `DB_USER`: usuário do banco.
- `DB_PASSWORD`: senha do banco.
- `DB_ROOT_PASSWORD`: senha do administrador do MySQL.
- `JWT_SECRET`: chave utilizada para assinatura dos tokens JWT.
- `GOOGLE_API_KEY`: chave de integração com a API do Google, quando necessária.
- `SPRING_MAIL_*`: configurações do serviço de envio de e-mails.
- `FRONTEND_URL`: endereço do frontend.

**Segurança:** o arquivo `.env` contém informações sensíveis e não deve ser versionado. Os arquivos `.env.example` e `.env.dev.example` servem apenas como modelos e não devem conter credenciais reais.

As senhas e chaves ilustrativas devem ser substituídas por valores seguros antes da utilização da aplicação.

### 4. Iniciar o banco de dados e o backend

Com o Docker em execução, utilize:

```bash
docker compose -f docker-compose.dev.yml up --build
```

Esse comando inicia:

- O banco de dados MySQL 8, com porta local `3307`.
- O backend Spring Boot, disponível na porta `8080`.

Para inicializar novamente sem solicitar uma reconstrução explícita das imagens:

```bash
docker compose -f docker-compose.dev.yml up
```

### 5. Iniciar o frontend

Abra outro terminal e acesse o diretório:

```bash
cd source/noctua-frontend
```

Instale as dependências:

```bash
npm install
```

Inicie o servidor de desenvolvimento:

```bash
npm run dev
```

O Vite disponibilizará o endereço local da aplicação no terminal.

O frontend utiliza um proxy para encaminhar chamadas `/api/*` ao backend em `http://localhost:8080`, conforme a configuração do projeto.

### 6. Encerrar os serviços

Para interromper e remover os contêineres do ambiente de desenvolvimento:

```bash
docker compose -f docker-compose.dev.yml down
```

Os volumes persistentes não são removidos por esse comando.

## Execução com Docker Compose completo

O arquivo `docker-compose.yml` define os serviços de frontend e backend para execução em contêineres, incluindo a interface servida pelo Nginx.

Diferentemente da configuração de desenvolvimento, esse arquivo não cria um serviço MySQL local. É necessário configurar um banco de dados externo acessível ao backend.

Após configurar o arquivo `.env` com as credenciais e os endereços adequados, os serviços podem ser iniciados com:

```bash
docker compose up --build -d
```

Nessa configuração, o frontend publica a porta `80` e o backend publica a porta `8000`.

Essa modalidade exige configurações adicionais de banco de dados, rede e segurança para utilização fora de um ambiente local.

## Infraestrutura AWS — Documentação do projeto original

Durante o desenvolvimento, o Noctua contou com uma arquitetura de implantação baseada em serviços da Amazon Web Services (AWS), incluindo EC2, Aurora MySQL e GitHub Actions.

**Status atual:** a infraestrutura AWS original foi desativada. As instruções a seguir documentam a configuração utilizada anteriormente e não representam um ambiente de produção ativo.

### 1. Instância EC2

A infraestrutura previa uma instância EC2 com Amazon Linux para executar os serviços da aplicação.

Configurações de referência:

- Nome da instância: `noctua`
- Chave SSH: `noctua-key.pem`

A conexão à instância poderia ser realizada com:

```bash
ssh -i noctua-key.pem ec2-user@<endpoint-publico-da-ec2>
```

### 2. Banco de dados Amazon Aurora

O banco de dados era baseado no Amazon Aurora, compatível com MySQL.

Configurações de referência:

- Nome do cluster: `noctua-aurora`
- Banco de dados: `noctua`
- Porta: `3306`

Formato de conexão:

```text
jdbc:mysql://<endpoint-do-aurora>:3306/noctua?serverTimezone=UTC
```

As credenciais e os endereços reais não devem ser armazenados diretamente no código-fonte.

### 3. Conectividade entre EC2 e Aurora

A configuração da infraestrutura exigia:

1. Configurar a instância EC2 e o cluster Aurora em redes compatíveis dentro da VPC.
2. Definir Security Groups para autorizar o tráfego necessário na porta MySQL `3306`.
3. Restringir o acesso ao banco de dados às origens autorizadas.
4. Garantir a comunicação privada entre a aplicação e o serviço de banco de dados.

### 4. Preparação da instância EC2

Atualização do sistema operacional:

```bash
sudo dnf update -y
```

Instalação do Git:

```bash
sudo dnf install git -y
```

Instalação do Docker:

```bash
sudo dnf install docker -y
sudo systemctl enable docker
sudo systemctl start docker
```

Adição do usuário ao grupo Docker:

```bash
sudo usermod -aG docker ec2-user
```

Após essa configuração, é necessário encerrar e iniciar novamente a sessão SSH para aplicar as permissões do grupo.

### 5. Instalação do Docker Compose

```bash
sudo mkdir -p /usr/local/lib/docker/cli-plugins
```

```bash
sudo curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 \
-o /usr/local/lib/docker/cli-plugins/docker-compose
```

```bash
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

Verificação:

```bash
docker compose version
```

### 6. Instalação do Docker Buildx

```bash
mkdir -p ~/.docker/cli-plugins
```

Download do Buildx, conforme a versão utilizada na configuração original:

```bash
curl -L https://github.com/docker/buildx/releases/download/v0.25.0/buildx-v0.25.0.linux-amd64 \
-o ~/.docker/cli-plugins/docker-buildx
```

Permissão de execução:

```bash
chmod +x ~/.docker/cli-plugins/docker-buildx
```

Verificação:

```bash
docker buildx version
```

### 7. Automação com GitHub Actions

O projeto também possui um workflow de deploy localizado em:

```text
.github/workflows/prod.yml
```

O workflow foi desenvolvido para automatizar a implantação do ambiente com base nas configurações e credenciais fornecidas por meio do GitHub Actions.

O processo original contemplava a integração com a infraestrutura AWS e o acesso aos repositórios que compõem o sistema.

**Observação:** os workflows permanecem desabilitados neste fork. Para reutilizá-los, seria necessário revisar os comandos, permissões, secrets e recursos de infraestrutura antes de qualquer execução.

## Segurança

O Noctua incorpora funcionalidades de autenticação e controle de acesso, incluindo JWT e autenticação em dois fatores.

As configurações sensíveis devem ser mantidas fora do controle de versão, utilizando variáveis de ambiente e mecanismos seguros de gerenciamento de credenciais.

Os arquivos de exemplo não contêm credenciais destinadas ao uso em produção.

O projeto foi desenvolvido com finalidade acadêmica. Qualquer implantação pública deve ser precedida de revisão das dependências, configurações de segurança, controle de acesso e infraestrutura.

## Contexto acadêmico

O Noctua foi desenvolvido como projeto de conclusão do curso de Análise e Desenvolvimento de Sistemas, integrando conhecimentos de desenvolvimento web, modelagem de dados, engenharia de software, autenticação, testes e infraestrutura.

Além da implementação técnica, o projeto envolveu estudos sobre acompanhamento acadêmico, experiência do usuário e organização de informações para apoiar processos de ensino e aprendizagem.

O desenvolvimento em equipe permitiu integrar diferentes componentes em uma aplicação full-stack, desde a interface até a infraestrutura necessária ao funcionamento do sistema.

---

**Noctua — Projeto acadêmico de desenvolvimento full-stack.**