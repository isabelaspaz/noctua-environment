# Noctua Environment

Este projeto visa estabelecer um ambiente de desenvolvimento integrado para aplicações full-stack, contemplando a camada de aplicação (Spring Boot) e a camada de visualização (React e Vite). O repositório principal organiza e orquestra esses módulos de forma modular, facilitando o desenvolvimento, testes e implantação.

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

É necessário adicionar ao arquivo .env nossas credenciais.

## Execução do Projeto

O ambiente pode ser executado de duas formas:

### 1. Produção — Docker Compose completo

Sobe todos os serviços (banco, backend e frontend) de forma integrada.

Na primeira execução:

```bash
docker compose -f docker-compose.dev.yml up
```

Nas demais execuções, sem necessidade de rebuild:

```bash
docker compose up
```

**Em outra terminal:**

```bash
cd source/noctua-frontend
npm install   # apenas na primeira vez
npm run dev
```

O Vite está configurado para redirecionar automaticamente as chamadas `/api/*` para o backend em `http://localhost:8080`, sem necessidade de configurações adicionais.

# Deploy da Infraestrutura Noctua

## 1. Criar a instância EC2

Criar uma instância EC2 utilizando Amazon Linux.

### Configurações recomendadas

* Nome da instância: `noctua`
* Chave SSH: `noctua-key.pem`

Após a criação da instância, copiar o endpoint público da EC2 e configurar a seguinte secret no GitHub:

### GitHub Secrets

#### EC2_HOST

```text
<endpoint-publico-da-ec2>
```

---

## 2. Criar o banco de dados Aurora

Criar um cluster Aurora MySQL.

### Configurações

* Nome do cluster: `noctua-aurora`
* Usuário administrador: `noctua`

Após a criação do cluster, obter o endpoint do banco e configurar a seguinte secret:

### DB_URL

```text
jdbc:mysql://<endpoint-do-aurora>:3306/noctua?serverTimezone=UTC&createDatabaseIfNotExist=true
```

---

## 3. Configurar conectividade entre EC2 e Aurora

Após a criação dos recursos:

1. Verificar se a instância EC2 e o cluster Aurora estão na mesma VPC.
2. Configurar os Security Groups para permitir acesso ao MySQL (porta 3306).
3. Garantir que a EC2 consiga se comunicar com o Aurora/RDS através da rede privada da AWS.

---

## 4. Configurar a EC2

Conecte-se à instância via SSH:

```bash
ssh -i noctua-key.pem ec2-user@<endpoint-publico-da-ec2>
```

### Atualizar o sistema

```bash
sudo dnf update -y
```

### Instalar Git

```bash
sudo dnf install git -y
```

### Instalar Docker

```bash
sudo dnf install docker -y
```

```bash
sudo systemctl enable docker
```

```bash
sudo systemctl start docker
```

Adicionar o usuário ao grupo Docker:

```bash
sudo usermod -aG docker ec2-user
```

Após executar o comando acima, faça logout e login novamente para aplicar as permissões.

---

## 5. Instalar Docker Compose

```bash
sudo mkdir -p /usr/local/lib/docker/cli-plugins

sudo curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 \
-o /usr/local/lib/docker/cli-plugins/docker-compose

sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

Verificar instalação:

```bash
docker compose version
```

---

## 6. Instalar Docker Buildx

```bash
mkdir -p ~/.docker/cli-plugins

curl -L https://github.com/docker/buildx/releases/download/v0.25.0/buildx-v0.25.0.linux-amd64 \
-o ~/.docker/cli-plugins/docker-buildx

chmod +x ~/.docker/cli-plugins/docker-buildx
```

Verificar instalação:

```bash
docker buildx version
```

A saída deve ser semelhante a:

```text
github.com/docker/buildx v0.25.0
```

---

## 7. Executar o Deploy

O deploy poderá ser realizado automaticamente através do GitHub Actions neste repositório.
