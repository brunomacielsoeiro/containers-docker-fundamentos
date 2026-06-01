# 🛠️ Comandos Docker — Referência Completa

## Imagens

| Comando | Descrição |
|---------|-----------|
| `docker build -t nome .` | Constrói imagem a partir do Dockerfile |
| `docker images` | Lista imagens locais |
| `docker pull nome:tag` | Baixa imagem do registry |
| `docker push nome:tag` | Envia imagem para o registry |
| `docker rmi nome` | Remove imagem |
| `docker image prune` | Remove imagens não utilizadas |

```bash
# Build com tag
docker build -t minha-app:1.0 .

# Build sem cache (força rebuild)
docker build --no-cache -t minha-app .

# Listar imagens
docker images

# Remover imagens dangling (sem tag)
docker image prune
```

---

## Containers

### Executar

| Comando | Descrição |
|---------|-----------|
| `docker run nome` | Cria e inicia container |
| `docker run -d nome` | Roda em background (detached) |
| `docker run -p 8080:80 nome` | Mapeia porta host:container |
| `docker run --name meu nome` | Define nome do container |
| `docker run -e VAR=valor nome` | Define variável de ambiente |
| `docker run -v vol:/path nome` | Monta volume |
| `docker run --rm nome` | Remove container ao parar |
| `docker run -it nome bash` | Modo interativo com terminal |

```bash
# Rodar app com porta mapeada
docker run -d -p 5000:5000 --name api flask-app

# Rodar com variáveis de ambiente
docker run -d -e DATABASE_URL=postgres://... minha-app

# Rodar interativo (entrar no container)
docker run -it python:3.9 bash
```

### Gerenciar

| Comando | Descrição |
|---------|-----------|
| `docker ps` | Lista containers rodando |
| `docker ps -a` | Lista todos (incluindo parados) |
| `docker stop nome` | Para container gracefully |
| `docker start nome` | Inicia container parado |
| `docker restart nome` | Reinicia container |
| `docker rm nome` | Remove container parado |
| `docker rm -f nome` | Força remoção (mesmo rodando) |

```bash
# Ver containers rodando
docker ps

# Parar todos os containers
docker stop $(docker ps -q)

# Remover todos os containers parados
docker container prune
```

### Inspecionar

| Comando | Descrição |
|---------|-----------|
| `docker logs nome` | Ver logs do container |
| `docker logs -f nome` | Seguir logs em tempo real |
| `docker exec -it nome bash` | Entrar em container rodando |
| `docker inspect nome` | Detalhes completos (JSON) |
| `docker stats` | Uso de CPU/memória em tempo real |
| `docker top nome` | Processos rodando no container |

```bash
# Ver logs
docker logs -f --tail 100 minha-app

# Entrar no container
docker exec -it minha-app bash

# Ver uso de recursos
docker stats
```

---

## Volumes

| Comando | Descrição |
|---------|-----------|
| `docker volume create nome` | Cria volume |
| `docker volume ls` | Lista volumes |
| `docker volume inspect nome` | Detalhes do volume |
| `docker volume rm nome` | Remove volume |
| `docker volume prune` | Remove volumes não utilizados |

```bash
# Criar e usar volume
docker volume create dados-app
docker run -v dados-app:/data minha-app

# Bind mount (desenvolvimento)
docker run -v $(pwd):/app -p 5000:5000 flask-app
```

---

## Redes

| Comando | Descrição |
|---------|-----------|
| `docker network create nome` | Cria rede |
| `docker network ls` | Lista redes |
| `docker network inspect nome` | Detalhes da rede |
| `docker network connect rede container` | Conecta container à rede |
| `docker network rm nome` | Remove rede |

```bash
# Criar rede e conectar containers
docker network create app-net
docker run -d --network app-net --name api flask-app
docker run -d --network app-net --name db postgres
```

---

## Docker Compose

| Comando | Descrição |
|---------|-----------|
| `docker compose up` | Sobe todos os serviços |
| `docker compose up -d` | Sobe em background |
| `docker compose down` | Para e remove containers |
| `docker compose logs` | Ver logs de todos os serviços |
| `docker compose ps` | Lista serviços rodando |
| `docker compose build` | Rebuilda imagens |
| `docker compose exec serviço cmd` | Executa comando em serviço |

```bash
# Subir ambiente completo
docker compose up -d

# Rebuildar e subir
docker compose up -d --build

# Ver logs de um serviço
docker compose logs -f api

# Parar e limpar tudo
docker compose down -v  # -v remove volumes também
```

---

## Limpeza

```bash
# Remover TUDO não utilizado (containers, imagens, volumes, redes)
docker system prune -a

# Apenas containers parados
docker container prune

# Apenas imagens sem tag
docker image prune

# Apenas volumes órfãos
docker volume prune

# Ver uso de disco
docker system df
```

---

## Resumo Visual

```
┌─────────────────────────────────────────────────────────────┐
│                 COMANDOS MAIS USADOS                          │
│                                                              │
│  BUILD             RUN                MANAGE                  │
│  ─────             ───                ──────                  │
│  docker build      docker run -d      docker ps              │
│  docker pull       docker run -p      docker stop            │
│  docker push       docker run -v      docker logs            │
│                    docker run -e      docker exec            │
│                                                              │
│  COMPOSE           CLEAN              INSPECT                │
│  ───────           ─────              ───────                │
│  compose up -d     system prune       docker stats           │
│  compose down      container prune    docker inspect         │
│  compose logs      image prune        docker top             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```
