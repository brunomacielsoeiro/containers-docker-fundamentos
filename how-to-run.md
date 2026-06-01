# ▶️ Como Executar este Projeto

## Pré-requisitos

| Ferramenta | Instalação |
|-----------|-----------|
| Docker | [docs.docker.com/get-docker](https://docs.docker.com/get-docker/) |

Verifique a instalação:

```bash
docker --version
# Docker version 24.x.x
```

---

## Passo a Passo

### 1. Clone o repositório

```bash
git clone https://github.com/brunomacielsoeiro/containers-docker-fundamentos.git
cd containers-docker-fundamentos
```

### 2. Construa a imagem

```bash
docker build -t flask-app .
```

Saída esperada:
```
[+] Building 15.2s (8/8) FINISHED
 => [1/4] FROM python:3.9-slim
 => [2/4] WORKDIR /app
 => [3/4] COPY app/ /app
 => [4/4] RUN pip install flask
 => exporting to image
```

### 3. Execute o container

```bash
docker run -d -p 5000:5000 --name minha-app flask-app
```

| Flag | Significado |
|------|-------------|
| `-d` | Roda em background |
| `-p 5000:5000` | Mapeia porta do host para o container |
| `--name minha-app` | Nome do container |

### 4. Acesse a aplicação

Abra no navegador: **http://localhost:5000**

Resposta esperada:
```
Aplicacao rodando dentro de um container Docker!
```

### 5. Verificar logs

```bash
docker logs minha-app
```

### 6. Parar e remover

```bash
# Parar
docker stop minha-app

# Remover container
docker rm minha-app

# Remover imagem (opcional)
docker rmi flask-app
```

---

## Diagrama do Fluxo

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Dockerfile │────►│   Imagem    │────►│  Container  │
│  (receita)  │build│ (template)  │ run │  (rodando)  │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                                │
                                          porta 5000
                                                │
                                                ▼
                                    http://localhost:5000
```

---

## Troubleshooting

| Problema | Solução |
|----------|---------|
| Porta 5000 já em uso | Use outra porta: `-p 8080:5000` |
| Permissão negada | Rode com `sudo` ou adicione user ao grupo docker |
| Imagem não encontrada | Verifique se está no diretório correto ao rodar `build` |
| Container não inicia | Verifique logs: `docker logs minha-app` |
