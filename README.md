# microservice_v0.1

Projeto de estudo de uma arquitetura de microserviços escrita em **Go**, com cada serviço empacotado num contentor **Docker** e orquestrado com **Docker Compose**.

## Serviços

| Serviço | Pasta | Porta (host → contentor) | Estado |
| --- | --- | --- | --- |
| Mensagens | `message-service/` | `8080 → 8080` | Ativo |
| Autenticação | `auth-service/` | `9001 → 8080` | Em desenvolvimento (desativado no `docker-compose.yml`) |

### message-service

API HTTP construída com o framework [Gin](https://github.com/gin-gonic/gin).

Endpoints disponíveis:

| Método | Rota | Descrição | Resposta |
| --- | --- | --- | --- |
| `GET` | `/ping` | Verifica se o serviço está ativo | `{"message": "pong"}` |

### auth-service

Serviço de autenticação, ainda por implementar. As dependências já incluem o Gin, o driver do MongoDB (`go.mongodb.org/mongo-driver/v2`) e o `golang.org/x/crypto`.

## Tecnologias

- [Go](https://go.dev/) 1.27
- [Gin](https://github.com/gin-gonic/gin)
- [MongoDB Go Driver](https://www.mongodb.com/docs/drivers/go/current/) (auth-service)
- [Docker](https://www.docker.com/) e [Docker Compose](https://docs.docker.com/compose/)

## Estrutura do projeto

```
.
├── auth-service/
│   ├── Dockerfile
│   ├── go.mod
│   ├── go.sum
│   └── main.go
├── message-service/
│   ├── .dockerignore
│   ├── Dockerfile
│   ├── go.mod
│   ├── go.sum
│   └── main.go
└── docker-compose.yml
```

## Pré-requisitos

- Docker e Docker Compose **ou**
- Go 1.27+ instalado localmente

## Como executar

### Com Docker Compose

Na raiz do projeto:

```bash
docker compose up --build
```

O serviço de mensagens fica disponível em `http://localhost:8080`.

Para parar os contentores:

```bash
docker compose down
```

### Localmente (sem Docker)

```bash
cd message-service
go mod download
go run main.go
```

Por omissão, o Gin escuta na porta `8080` (pode ser alterada com a variável de ambiente `PORT`).

## Testar

```bash
curl http://localhost:8080/ping
```

Resposta esperada:

```json
{"message": "pong"}
```

## Adicionar um novo serviço

1. Crie uma pasta `<nome>-service/` com o seu `main.go`, `go.mod` e `Dockerfile`.
2. Adicione o serviço ao `docker-compose.yml`, definindo o contexto de build e uma porta livre no host.
3. Execute `docker compose up --build`.
