# n8n Setup com Docker Compose

Este repositório contém a configuração para executar o **n8n** utilizando Docker Compose, com suporte a PostgreSQL para banco de dados e Redis para fila de execução. O setup inclui containers separados para o editor, worker e webhook do n8n, garantindo escalabilidade e organização.

---

## Estrutura do ambiente

- **Redis**: Banco de dados em memória utilizado para gerenciamento de filas.
- **PostgreSQL**: Banco de dados relacional utilizado para armazenar dados do n8n.
- **n8n-editor**: Interface web do n8n para criação e gerenciamento de workflows.
- **n8n-worker**: Container que processa os workflows assincronamente.
- **n8n-webhook**: Container responsável por receber triggers externos via webhooks.

Todos os serviços estão conectados a uma network Docker chamada `n8n-network`.

---

## Pré-requisitos

- Docker instalado ([Docker Desktop](https://www.docker.com/products/docker-desktop) recomendado)
- Docker Compose instalado
- Arquivo `.env` configurado com as variáveis necessárias (exemplo abaixo)

---

## Configuração do arquivo `.env`

Copie o arquivo `.env.example` para `.env` e ajuste as variáveis conforme seu ambiente:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=n8n

DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=postgres
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=postgres
DB_POSTGRESDB_PASSWORD=postgres

QUEUE_MODE=redis
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
N8N_RUNNERS_ENABLED=true
N8N_REDIS_HOST=redis
N8N_REDIS_PORT=6379
N8N_REDIS_DB=0
N8N_REDIS_TIMEOUT_THRESHOLD=6000
````

---

## Como executar

1. Clone este repositório:

   ```bash
   git clone <URL_DO_REPOSITORIO>
   cd <NOME_DO_REPOSITORIO>
   ```

2. Crie o arquivo `.env` a partir do `.env.example` e ajuste as variáveis.

3. Inicie os containers com Docker Compose:

   ```bash
   docker-compose up -d
   ```

4. Acesse o editor do n8n pelo navegador:

   ```
   http://localhost:5678
   ```

---

## Persistência de dados

* Os dados do Redis ficam persistidos em `./redis_data`
* Os dados do PostgreSQL ficam persistidos em `./postgres_data`
* Os dados do n8n (configurações e workflows) ficam em `./n8n_data`

Esses diretórios são ignorados pelo Git conforme `.gitignore`.

---

## Parar e remover containers

Para parar e remover todos os containers, volumes e network:

```bash
docker-compose down -v
```


## Considerações

* A porta padrão do editor n8n é a **5678**.
* O worker e webhook possuem portas internas mapeadas para facilitar debug, mas normalmente não são acessadas diretamente.
* Caso precise escalar o worker, basta rodar múltiplos containers `n8n-worker` usando a mesma network e volumes.

---

## Links úteis

* [Documentação oficial n8n](https://docs.n8n.io/)
* [Imagem oficial n8n no Docker Hub](https://hub.docker.com/r/n8nio/n8n)
* [Redis](https://redis.io/)
* [PostgreSQL](https://www.postgresql.org/)

---

## Licença

Este projeto está aberto para uso pessoal e profissional conforme a licença do seu ambiente.
