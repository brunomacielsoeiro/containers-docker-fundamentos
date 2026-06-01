# 📖 Conceitos Docker

## 1. O que é Docker?

Docker é uma plataforma que permite **criar, distribuir e executar** aplicações em containers — ambientes isolados, leves e portáteis.

### Problema que resolve

| Sem Docker | Com Docker |
|-----------|-----------|
| "Funciona na minha máquina" | Funciona em qualquer lugar |
| Instalar dependências manualmente | Tudo declarado no Dockerfile |
| Conflito de versões | Cada container isolado |
| Setup demorado para novos devs | `docker run` e pronto |

---

## 2. Containers

### O que é?

Um container é um **processo isolado** que roda com seu próprio filesystem, rede e espaço de processos, mas compartilha o kernel do host.

### Características

| Propriedade | Valor |
|-------------|-------|
| Isolamento | Filesystem, rede, processos separados |
| Peso | Megabytes (vs Gigabytes de VMs) |
| Startup | Segundos (vs minutos de VMs) |
| Portabilidade | Roda igual em qualquer host com Docker |
| Efêmero | Pode ser destruído e recriado a qualquer momento |

### Ciclo de vida

```
Imagem → Container (running) → Container (stopped) → Removido
         docker run            docker stop            docker rm
```

---

## 3. Imagens

### O que é?

Uma imagem é um **template read-only** que define o que vai dentro do container. É como uma "receita" ou "snapshot" do ambiente.

### Camadas (Layers)

```
┌─────────────────────────────┐
│  CMD ["python", "app.py"]   │  ← Layer 5 (comando)
├─────────────────────────────┤
│  COPY app/ /app             │  ← Layer 4 (seu código)
├─────────────────────────────┤
│  RUN pip install flask      │  ← Layer 3 (dependências)
├─────────────────────────────┤
│  WORKDIR /app               │  ← Layer 2 (config)
├─────────────────────────────┤
│  python:3.9-slim            │  ← Layer 1 (base image)
└─────────────────────────────┘
```

> Cada instrução no Dockerfile cria uma **camada**. Camadas são cacheadas — se não mudar, não rebuilda.

---

## 4. Dockerfile

### O que é?

Arquivo de texto com instruções para construir uma imagem Docker.

### Instruções principais

| Instrução | Função | Exemplo |
|-----------|--------|---------|
| `FROM` | Imagem base | `FROM python:3.9-slim` |
| `WORKDIR` | Diretório de trabalho | `WORKDIR /app` |
| `COPY` | Copia arquivos para a imagem | `COPY . /app` |
| `RUN` | Executa comando durante build | `RUN pip install flask` |
| `EXPOSE` | Documenta a porta | `EXPOSE 5000` |
| `CMD` | Comando ao iniciar container | `CMD ["python", "app.py"]` |
| `ENV` | Variável de ambiente | `ENV PORT=5000` |
| `ENTRYPOINT` | Comando fixo (não sobrescrevível) | `ENTRYPOINT ["python"]` |

### Exemplo deste projeto

```dockerfile
FROM python:3.9-slim       # Imagem base leve com Python
WORKDIR /app               # Define diretório de trabalho
COPY app/ /app             # Copia código para dentro da imagem
RUN pip install flask      # Instala dependências
EXPOSE 5000                # Documenta porta exposta
CMD ["python", "app.py"]   # Comando para iniciar a aplicação
```

---

## 5. Registry

### O que é?

Um registry é um **repositório de imagens Docker**. O mais popular é o Docker Hub.

### Fluxo

```
Desenvolvedor → docker build → Imagem local → docker push → Registry (Docker Hub)
                                                                    │
Servidor     ← docker run   ← Imagem local ← docker pull  ←────────┘
```

### Registries populares

| Registry | Uso |
|---------|-----|
| Docker Hub | Público, imagens oficiais |
| Amazon ECR | AWS, privado |
| GitHub Container Registry | GitHub, integrado com Actions |
| Google Artifact Registry | GCP |

---

## 6. Volumes

### O que é?

Volumes permitem **persistir dados** fora do container. Sem volume, dados são perdidos quando o container é removido.

### Tipos

| Tipo | Comando | Uso |
|------|---------|-----|
| Named Volume | `-v meu-vol:/data` | Dados persistentes (DB) |
| Bind Mount | `-v ./local:/container` | Desenvolvimento (hot reload) |
| tmpfs | `--tmpfs /tmp` | Dados temporários em memória |

```bash
# Volume nomeado (persiste dados)
docker run -v postgres-data:/var/lib/postgresql/data postgres

# Bind mount (desenvolvimento)
docker run -v $(pwd)/app:/app flask-app
```

---

## 7. Networking

### Tipos de rede

| Tipo | Descrição | Uso |
|------|-----------|-----|
| `bridge` | Rede padrão, containers se comunicam | Maioria dos casos |
| `host` | Container usa rede do host | Performance |
| `none` | Sem rede | Isolamento total |

```bash
# Criar rede
docker network create minha-rede

# Rodar containers na mesma rede
docker run --network minha-rede --name api flask-app
docker run --network minha-rede --name db postgres
# "api" pode acessar "db" pelo nome
```

---

## 8. Docker Compose

### O que é?

Ferramenta para definir e rodar **múltiplos containers** com um único arquivo YAML.

### Exemplo

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - pg-data:/var/lib/postgresql/data

volumes:
  pg-data:
```

```bash
# Subir tudo
docker compose up -d

# Parar tudo
docker compose down
```

---

## Resumo Visual

```
┌─────────────────────────────────────────────────────────────┐
│                    DOCKER - CONCEITOS                         │
│                                                              │
│  CONSTRUIR         DISTRIBUIR        EXECUTAR                │
│  ─────────         ──────────        ────────                │
│  • Dockerfile      • Registry        • docker run            │
│  • docker build    • docker push     • Containers            │
│  • Layers/Cache    • docker pull     • Volumes               │
│  • Multi-stage     • Tags            • Networks              │
│                                                              │
│  IMAGEM            CONTAINER         COMPOSE                 │
│  ──────            ─────────         ───────                 │
│  • Read-only       • Read-write      • Multi-container       │
│  • Template        • Instância       • docker-compose.yml    │
│  • Compartilhável  • Efêmero         • Orquestração local   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```
