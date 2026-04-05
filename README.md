# OpenClaude com Docker

Ambiente Docker para rodar o **OpenClaude** com **Codex**, com workspace persistente e configuração simples para uso local. O projeto usa um container baseado em Node.js, instala o Codex CLI e o pacote `@gitlawb/openclaude`, e monta uma pasta local para seus arquivos de trabalho.

---

## Visão geral

Este repositório foi criado para facilitar a configuração de um ambiente local com:

- **Docker**
- **OpenClaude**
- **Codex CLI**
- **workspace persistente**
- **volume persistente para configuração do Codex**

A ideia é que você clone o projeto, configure as variáveis de ambiente, suba o container e entre nele para autenticar o Codex e começar a usar o OpenClaude.

---

## Estrutura do projeto

```txt
.
├─ docker-compose.yml
├─ Dockerfile
├─ .env.example
├─ .gitignore
└─ workspace/
   └─ .gitkeep
```

---

## Pré-requisitos

Antes de começar, você precisa instalar:

- **Git**
- **Docker**
- **Docker Compose**

Hoje, no ecossistema Docker, o caminho mais comum em **Windows** e **macOS** é instalar o **Docker Desktop**. No **Linux**, você pode usar tanto o **Docker Desktop** quanto o **Docker Engine**.

---

# Instalação do Docker

## Windows

A forma mais simples é instalar o **Docker Desktop for Windows**.

### Passo a passo

1. Acesse o site oficial da Docker.
2. Baixe o **Docker Desktop for Windows**.
3. Execute o instalador.
4. Se solicitado, habilite o **WSL 2**.
5. Finalize a instalação e abra o Docker Desktop.
6. Aguarde a inicialização completa.

### Teste da instalação

Abra o terminal e rode:

```bash
docker --version
docker compose version
```

Se ambos responderem com a versão instalada, o ambiente está pronto.

---

## macOS

No macOS, a forma mais simples também é instalar o **Docker Desktop for Mac**.

### Passo a passo

1. Acesse o site oficial da Docker.
2. Baixe a versão correta para o seu processador:
   - **Apple Silicon**
   - **Intel**
3. Instale o aplicativo.
4. Abra o Docker Desktop.
5. Aguarde a inicialização completa.

### Teste da instalação

No terminal:

```bash
docker --version
docker compose version
```

Se os comandos retornarem a versão instalada, o Docker está funcionando corretamente.

---

## Linux (Ubuntu)

No Ubuntu, você pode usar **Docker Desktop** ou **Docker Engine**. Para ambientes de desenvolvimento e servidores, normalmente faz mais sentido instalar o **Docker Engine** diretamente.

### Passo a passo com Docker Engine no Ubuntu

#### 1. Remova pacotes antigos ou conflitantes

```bash
sudo apt remove docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc
```

#### 2. Atualize o sistema e instale dependências

```bash
sudo apt update
sudo apt install -y ca-certificates curl
```

#### 3. Crie a pasta da keyring

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

#### 4. Adicione a chave GPG oficial da Docker

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

#### 5. Adicione o repositório oficial

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 6. Atualize os índices

```bash
sudo apt update
```

#### 7. Instale o Docker Engine e plugins

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Teste da instalação

```bash
docker --version
docker compose version
```

---

# Como usar este projeto

## 1. Clone o repositório

```bash
git clone https://github.com/SEU-USUARIO/openclaude-docker.git
cd openclaude-docker
```

---

## 2. Crie o arquivo `.env`

Copie o arquivo de exemplo:

### Linux / macOS

```bash
cp .env.example .env
```

### Windows PowerShell

```powershell
copy .env.example .env
```

Depois edite o arquivo `.env` e defina sua configuração. Exemplo:

```env
OPENAI_API_KEY=sua_chave_aqui
CLAUDE_CODE_USE_OPENAI=1
OPENAI_MODEL=codexplan
```

---

## 3. Suba o projeto

```bash
docker compose up -d --build
```

Esse comando faz o build da imagem e sobe o serviço em segundo plano.

---

## 4. Verifique se o container está rodando

```bash
docker compose ps
```

Você também pode ver logs com:

```bash
docker compose logs -f
```

---

## 5. Entre no container

```bash
docker compose exec openclaude bash
```

Ao entrar, você estará no diretório `/workspace`, que é a pasta montada do projeto local.

---

# Configuração do Codex

## 1. Entre no container

```bash
docker compose exec openclaude bash
```

## 2. Rode o login do Codex

```bash
codex login
```

## 3. Se quiser remover as credenciais salvas

```bash
codex logout
```

---

# Executando o OpenClaude

Depois de subir o projeto, entrar no container e autenticar o Codex, rode:

```bash
openclaude
```

Se o ambiente estiver configurado corretamente, o OpenClaude deverá iniciar dentro do container.

---

# Comandos básicos do dia a dia

## Subir o projeto

```bash
docker compose up -d --build
```

## Parar o projeto

```bash
docker compose down
```

## Reiniciar o projeto

```bash
docker compose down
docker compose up -d --build
```

## Entrar no container

```bash
docker compose exec openclaude bash
```

## Ver logs

```bash
docker compose logs -f
```

## Ver containers em execução

```bash
docker compose ps
```

## Remover também os volumes

```bash
docker compose down -v
```

Esse último comando apaga também os volumes associados ao projeto, então use com cuidado se quiser preservar a configuração do Codex.

---

# Persistência

## Workspace

A pasta local `./workspace` é montada dentro do container em:

```bash
/workspace
```

Isso significa que tudo que você criar ou editar nessa pasta continuará salvo no host.

## Configuração do Codex

A configuração do Codex é persistida em um volume Docker chamado:

```txt
codex_config
```

Assim, a autenticação e parte da configuração sobrevivem a reinicializações do container, desde que o volume não seja apagado.

---

# Fluxo rápido de uso

Depois de instalar Docker e clonar o repo, o fluxo mais comum fica assim:

```bash
cp .env.example .env
docker compose up -d --build
docker compose exec openclaude bash
codex login
openclaude
```

---

# Solução de problemas

## `docker: command not found`

O Docker não foi instalado corretamente ou não foi reiniciado após a instalação.

## `docker compose` não funciona

No Linux, normalmente isso indica que o plugin `docker-compose-plugin` não foi instalado junto com o Docker Engine.

## Problemas com WSL no Windows

Confirme se o Docker Desktop foi instalado com suporte ao WSL 2 ativado.

## Preciso usar API key ou login?

O fluxo depende de como você quer operar seu ambiente. Se quiser usar autenticação do Codex, faça `codex login`. Se preferir usar variáveis de ambiente, mantenha seu `.env` configurado corretamente.

---

# Observações finais

- O arquivo `.env` não deve ser enviado para o GitHub.
- O projeto foi pensado para uso local.
- A pasta `workspace/` pode ser usada para armazenar seus arquivos de trabalho.
- O volume `codex_config` ajuda a preservar a autenticação do Codex entre reinicializações do container.

---

# Licença

Você pode adicionar uma licença como **MIT** caso queira disponibilizar o projeto publicamente no GitHub.
