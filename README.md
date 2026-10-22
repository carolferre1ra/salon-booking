# SalonBooking

Um sistema de agendamento para salões de beleza que ajuda a evitar conflitos de horários e organizar os atendimentos.

## Autores

- Caroline Ferreira — [carolferre1ra](https://github.com/carolferre1ra)

## Stack

- **Backend:** NestJS + Prisma + PostgreSQL (conforme [docs/architecture.md](docs/architecture.md))
- **Frontend:** React + Vite (conforme [docs/architecture.md](docs/architecture.md))

## Estado atual

O scaffold do monorepo foi concluído nesta etapa (Atividade 08). A estrutura base das aplicações `apps/api` (NestJS) e `apps/web` (React + Vite) já está criada. Ainda não foram implementadas funcionalidades de negócio.

## Pré-requisitos

- Node.js (versão compatível com as dependências do projeto)
- npm

## Instalação

### Aplicação API (`apps/api`)

```bash
cd apps/api
npm install --legacy-peer-deps
```

**Observação:** No ambiente testado (Node 22.20.0 e npm 10.9.3), foi necessário utilizar `--legacy-peer-deps` devido ao erro `"Cannot read properties of null (reading 'edgesOut')"` durante a instalação do NestJS. Esse procedimento foi o que funcionou neste ambiente; não é garantido que seja necessário em todos os ambientes. Não altere as versões já declaradas em `package.json`.

### Aplicação Web (`apps/web`)

```bash
cd apps/web
npm install
```

## Execução

A partir da raiz do monorepo:

- **API (desenvolvimento):** `npm run api` (equivale a `npm --prefix apps/api run start:dev`)
- **Frontend (desenvolvimento):** `npm run web` (equivale a `npm --prefix apps/web run dev`)

Ou diretamente em cada aplicação:

- API: `cd apps/api && npm run start:dev`
- Web: `cd apps/web && npm run dev`

## Testes

A partir da raiz:

```bash
npm run test
```

Ou em cada aplicação:

- API: `cd apps/api && npm run test`
- Web: `cd apps/web && npm run test`

## Lint

A partir da raiz:

```bash
npm run lint
```

Ou em cada aplicação:

- API: `cd apps/api && npm run lint`
- Web: `cd apps/web && npm run lint`

## Build (frontend)

```bash
cd apps/web && npm run build
```

## Documentação

- [docs/prd.md](docs/prd.md) — requisitos, histórias e regras de negócio
- [docs/user-flows.md](docs/user-flows.md) — jornadas dos usuários e pontos de desistência
- [docs/design-tokens.md](docs/design-tokens.md) — paleta, espaçamento, tipografia e protótipo
- [docs/architecture.md](docs/architecture.md) — arquitetura e decisões técnicas
- [docs/checklist.md](docs/checklist.md) — ficha da disciplina e critérios de entrega
