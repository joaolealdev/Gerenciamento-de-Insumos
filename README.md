# 📦 Sistema de Gerenciamento de Insumos

Plataforma web para controle de insumos de uma empresa — cadastro, movimentações de estoque e alertas de estoque mínimo, construída com Fastify, Prisma ORM e Next.js.

---

## 📋 Sumário

- [Sobre o Projeto](#-sobre-o-projeto)
- [Arquitetura](#-arquitetura)
- [Tecnologias](#-tecnologias)
- [Requisitos Funcionais](#-requisitos-funcionais)
- [Requisitos Não Funcionais](#-requisitos-não-funcionais)
- [Estrutura de Pastas](#-estrutura-de-pastas)
- [Como Executar](#-como-executar)
- [Variáveis de Ambiente](#-variáveis-de-ambiente)
- [Endpoints da API](#-endpoints-da-api)

---

## 🎯 Sobre o Projeto

O **Sistema de Gerenciamento de Insumos** é uma aplicação web para centralizar o controle de materiais de uma empresa. Permite cadastrar insumos, registrar entradas e saídas de estoque e visualizar um dashboard com o resumo geral — emitindo alertas quando algum item atinge o estoque mínimo.

---

## 🏗️ Arquitetura

```
┌──────────────────────┐
│   Next.js Dashboard  │
│   • Listagem         │
│   • Formulários      │
│   • Dashboard        │
└──────────┬───────────┘
           │ HTTP REST
┌──────────▼───────────┐
│   Fastify (Node.js)  │
│   • API REST         │
│   • Validações       │
│   • Regras de negócio│
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│     Prisma ORM        │
│   • Insumos          │
│   • Movimentações    │
└──────────────────────┘
```

---

## 🛠️ Tecnologias

| Camada | Tecnologia |
|---|---|
| Frontend | Next.js (App Router, TypeScript) |
| Backend | Fastify (Node.js, TypeScript) |
| Banco de dados | Prisma ORM |
| Linguagem | TypeScript |

---

## ✅ Requisitos Funcionais

### RF01 — Gestão de Insumos

| ID | Requisito | Prioridade |
|---|---|---|
| RF01.1 | O sistema deve permitir o cadastro de insumos com nome, unidade de medida, categoria e quantidade mínima | Alta |
| RF01.2 | O sistema deve permitir a edição e exclusão de insumos cadastrados | Alta |
| RF01.3 | O sistema deve listar todos os insumos com filtro por nome e categoria | Alta |

### RF02 — Controle de Estoque

| ID | Requisito | Prioridade |
|---|---|---|
| RF02.1 | O sistema deve permitir o registro de entradas de estoque informando insumo, quantidade e data | Alta |
| RF02.2 | O sistema deve permitir o registro de saídas de estoque informando insumo, quantidade e motivo | Alta |
| RF02.3 | O sistema deve calcular automaticamente o saldo atual de cada insumo com base nas movimentações | Alta |
| RF02.4 | O sistema deve manter um histórico de todas as movimentações | Média |

### RF03 — Alertas e Dashboard

| ID | Requisito | Prioridade |
|---|---|---|
| RF03.1 | O sistema deve emitir um alerta visual quando o estoque de um insumo atingir ou ficar abaixo do mínimo | Alta |
| RF03.2 | O dashboard deve exibir o total de insumos cadastrados, quantos estão com estoque baixo e as últimas movimentações | Alta |

---

## ⚙️ Requisitos Não Funcionais

| ID | Requisito | Categoria |
|---|---|---|
| RNF01 | Todas as credenciais devem ser configuradas via variáveis de ambiente (.env) | Segurança |
| RNF02 | O projeto deve rodar localmente exigindo apenas Node.js instalado | Portabilidade |
| RNF03 | O código deve ser escrito em TypeScript em todas as camadas | Manutenibilidade |
| RNF05 | O acesso ao banco de dados deve ser feito exclusivamente via Prisma ORM, sem queries SQL manuais | Manutenibilidade |
| RNF04 | O banco de dados deve ser gerenciado por migrations versionadas | Manutenibilidade |

---

## 📁 Estrutura de Pastas

```
insumos-system/
├── .env.example
│
├── backend/
│   ├── package.json
│   └── src/
│       ├── index.ts                 # Entry point do Fastify
│       ├── plugins/
│       │   └── database.ts          # Instância do Prisma Client
│       ├── routes/
│       │   ├── insumos.ts           # CRUD /api/insumos
│       │   ├── movimentacoes.ts     # /api/movimentacoes
│       │   └── dashboard.ts         # GET /api/dashboard/resumo
│       └── prisma/
│           ├── schema.prisma        # Models do banco de dados
│           └── migrations/          # Migrations geradas pelo Prisma
│
└── frontend/
    ├── package.json
    └── app/
        ├── page.tsx                 # Dashboard principal
        ├── insumos/
        │   └── page.tsx             # Listagem e cadastro
        ├── movimentacoes/
        │   └── page.tsx             # Histórico de movimentações
        └── components/
            ├── InsumosTable.tsx
            ├── MovimentacaoForm.tsx
            └── EstoqueBadge.tsx     # Badge de alerta de estoque
```

---

## 🚀 Como Executar

### Pré-requisitos

- [Node.js](https://nodejs.org/) v18 ou superior

### Passos

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/insumos-system.git
cd insumos-system

# 2. Configure as variáveis de ambiente
cp .env.example .env
# Edite o .env com a sua DATABASE_URL do Prisma

# 3. Instale e inicie o backend
cd backend
npm install

# 4. Rode as migrations com o Prisma
npx prisma migrate dev

# 5. Inicie o backend
npm run dev
# API disponível em http://localhost:3001

# 6. Em outro terminal, inicie o frontend
cd ../frontend
npm install
npm run dev
# Dashboard disponível em http://localhost:3000
```

---

## 🔧 Variáveis de Ambiente

| Variável | Descrição | Padrão |
|---|---|---|
| `DATABASE_URL` | URL de conexão do Prisma | `file:./dev.db` (SQLite) ou `postgresql://...` |
| `API_PORT` | Porta do backend | `3001` |
| `NEXT_PUBLIC_API_URL` | URL da API consumida pelo frontend | `http://localhost:3001` |

---

## 🛣️ Endpoints da API

### Insumos

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/insumos` | Lista todos os insumos |
| `POST` | `/api/insumos` | Cadastra um novo insumo |
| `PUT` | `/api/insumos/:id` | Atualiza um insumo |
| `DELETE` | `/api/insumos/:id` | Remove um insumo |

### Movimentações

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/movimentacoes` | Lista o histórico de movimentações |
| `POST` | `/api/movimentacoes/entrada` | Registra uma entrada de estoque |
| `POST` | `/api/movimentacoes/saida` | Registra uma saída de estoque |

### Dashboard

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/api/dashboard/resumo` | Retorna total de insumos, alertas ativos e últimas movimentações |
