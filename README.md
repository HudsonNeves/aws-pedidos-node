# 🚀 Mini Plataforma de Pedidos AWS (Node.js + SQS + CloudWatch)

Este projeto implementa uma **mini plataforma de processamento de pedidos** utilizando serviços essenciais da AWS, como **EC2**, **SQS** e **CloudWatch Logs**, com uma API em **Node.js** e um **worker de consumo assíncrono**.

O objetivo é demonstrar **arquitetura distribuída**, boas práticas básicas e provisionamento na nuvem — ideal para estudos de certificação (AWS Cloud Practitioner / Solutions Architect Associate) e para portfólio.

---

## 📌 **Sumário**
- [Arquitetura](#-arquitetura)
- #-tecnologias-utilizadas
- [Como Funciona](#-como-funciona)
- [Executando Localmente](#-executando-localmente)
- [Integração com AWS SQS](#-integração-com-aws-sqs)
- [Deploy em EC2 (Free Tier)](#-deploy-em-ec2-free-tier)
- [Scripts de Infraestrutura](#-scripts-de-infraestrutura)
- #-endpoints-da-api
- [Evoluções Futuras](#-evoluções-futuras)

---

## 🧱 **Arquitetura**

A solução é composta por três partes principais:

### 👇 Detalhes do fluxo:

1. O usuário envia um pedido via endpoint `POST /pedido`.
2. A API publica esse pedido na fila **Amazon SQS**.
3. O **worker** consome mensagens da fila, processa e envia logs para o **Amazon CloudWatch**.
4. Toda a aplicação roda em uma ou mais instâncias **EC2**.

Essa arquitetura pode ser facilmente expandida com:
- **Auto Scaling Group**
- **Elastic Load Balancer**
- Separação em *worker tier* e *web tier*

---

## 🛠️ **Tecnologias Utilizadas**

### 🟦 Linguagens / Frameworks
- Node.js 18+ ou 20+ ou 24 (compatível)
- Express
- AWS SDK v3 (`@aws-sdk/client-sqs`)
- Pino (logs)
- Morgan (HTTP logging)

### 🟧 AWS Free Tier
- Amazon EC2 (t2.micro / t3.micro)
- Amazon SQS
- Amazon CloudWatch Logs
- IAM (Roles & Policies)
- VPC default

---

## ⚙️ **Como Funciona**

### 👉 API (`app.js`)
- Recebe pedidos via HTTP
- Se SQS não estiver configurada, funciona em modo *local*
- Exibe métricas da fila (`queue-metrics`)

### 👉 Worker (`worker.js`)
- Usa *long polling* para consumir a fila SQS
- Processa pedidos (simulação com timeout)
- Deleta mensagens após processar
- Gera logs estruturados para o CloudWatch

---

## 💻 **Executando Localmente**

### 1️⃣ Instalar dependências
```bash
cd app
npm install
