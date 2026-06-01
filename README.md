# 🐳 Containers com Docker — Fundamentos

> Projeto de estudos sobre containers utilizando Docker, abordando conceitos de isolamento, criação de imagens, Dockerfile e execução de aplicações containerizadas. Atividade acadêmica para cumprimento de horas complementares.

---

## 📋 Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [O que você vai aprender](#o-que-você-vai-aprender)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Aplicação Prática](#aplicação-prática)
- [Como Executar](#como-executar)
- [Arquitetura](#arquitetura)
- [Conexão com outros projetos](#conexão-com-outros-projetos)
- [Referências](#referências)

---

## Sobre o Projeto

Este repositório demonstra os fundamentos de containers Docker na prática. Inclui uma aplicação Python (Flask) containerizada, documentação de conceitos e referência de comandos.

### Por que Docker é importante?

- Garante que a aplicação roda **igual** em qualquer ambiente
- Elimina o "funciona na minha máquina"
- Base para Kubernetes, CI/CD e microserviços
- Padrão da indústria para deploy de aplicações

---

## O que você vai aprender

| Tema | Arquivo | Descrição |
|------|---------|-----------|
| Conceitos Docker | [docker-conceitos.md](./docker-conceitos.md) | Containers, imagens, registry, volumes |
| Comandos Docker | [docker-comandos.md](./docker-comandos.md) | Referência completa com exemplos |
| Como Executar | [how-to-run.md](./how-to-run.md) | Passo a passo para rodar o projeto |
| Dockerfile | [Dockerfile](./Dockerfile) | Exemplo prático de build |
| Aplicação | [app/app.py](./app/app.py) | App Flask containerizada |

---

## Estrutura do Repositório

```
containers-docker-fundamentos/
├── README.md              ← Este arquivo (visão geral)
├── docker-conceitos.md    ← Fundamentos teóricos
├── docker-comandos.md     ← Referência de comandos
├── how-to-run.md          ← Guia de execução
├── Dockerfile             ← Receita para construir a imagem
└── app/
    └── app.py             ← Aplicação Flask (Python)
```

---

## Aplicação Prática

Este projeto inclui uma aplicação web simples em Python/Flask que roda dentro de um container Docker:

```
┌─────────────────────────────────────────────────┐
│              Docker Container                     │
│                                                  │
│  ┌────────────────────────────────────────────┐ │
│  │  Python 3.9-slim                           │ │
│  │                                            │ │
│  │  ┌──────────────────────────────────────┐  │ │
│  │  │  Flask App (app.py)                  │  │ │
│  │  │  Porta: 5000                         │  │ │
│  │  │  Rota: GET /                         │  │ │
│  │  └──────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────┘ │
│                                                  │
└──────────────────────┬───────────────────────────┘
                       │ -p 5000:5000
                       ▼
              http://localhost:5000
```

---

## Como Executar

```bash
# 1. Construir a imagem
docker build -t flask-app .

# 2. Executar o container
docker run -p 5000:5000 flask-app

# 3. Acessar no navegador
# http://localhost:5000
```

---

## Arquitetura

### Container vs Máquina Virtual

```
CONTAINER                          MÁQUINA VIRTUAL
┌─────────────────────┐            ┌─────────────────────┐
│  App A  │  App B    │            │  App A  │  App B    │
├─────────┼───────────┤            ├─────────┼───────────┤
│  Libs A │  Libs B   │            │  Libs A │  Libs B   │
├─────────┴───────────┤            ├─────────┼───────────┤
│   Container Engine  │            │  OS     │  OS       │
│      (Docker)       │            │ (Linux) │ (Windows) │
├─────────────────────┤            ├─────────┴───────────┤
│   Sistema Operac.   │            │     Hypervisor      │
├─────────────────────┤            ├─────────────────────┤
│     Hardware        │            │     Hardware        │
└─────────────────────┘            └─────────────────────┘

• Leve (MBs)                       • Pesada (GBs)
• Inicia em segundos               • Inicia em minutos
• Compartilha kernel               • Kernel próprio
```

---

## Conexão com outros projetos

Docker é base para:

- **Kubernetes** — Orquestração de containers em escala
- **CI/CD** — Build e deploy automatizado em containers
- **DevOps** — Padronização de ambientes
- **Microserviços** — Cada serviço em seu container
- **Cloud** — ECS, EKS, Fargate (AWS)

---

## Referências

- [Docker Documentation](https://docs.docker.com/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Docker Hub](https://hub.docker.com/)
- [Play with Docker (lab gratuito)](https://labs.play-with-docker.com/)

---

## 📄 Licença

MIT License
