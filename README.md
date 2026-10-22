# SalonBooking

Um sistema de agendamento para salões de beleza que ajuda a evitar conflitos de horários e organizar os atendimentos.

## Autores

- Caroline Ferreira — [carolferre1ra](https://github.com/carolferre1ra)

## Stack

- **Backend:** NestJS + Prisma + PostgreSQL (conforme [docs/architecture.md](docs/architecture.md))
- **Frontend:** React + Vite (conforme [docs/architecture.md](docs/architecture.md))

## Estado atual

O scaffold do monorepo foi concluído nesta etapa (Atividade 08). A estrutura base das aplicações `apps/api` (NestJS) e `apps/web` (React + Vite) já está criada, com instalação centralizada via **npm workspaces** (lockfile único na raiz). Ainda não foram implementadas funcionalidades de negócio.

## Pré-requisitos

- Node.js (versão compatível com as dependências do projeto)
- npm

## Instalação

As duas aplicações (`apps/api` e `apps/web`) são **workspaces npm** declarados no `package.json` da raiz (`"workspaces": ["apps/*"]`). Por isso a instalação é **centralizada**: um único comando na raiz resolve as dependências das duas aplicações e produz **um único `package-lock.json`**.

```bash
npm install
```

A partir daí, `node_modules/` fica na raiz do monorepo, com o que cada aplicação precisar de versão própria aninhado em `apps/<app>/node_modules/`.

Para reinstalar do zero, respeitando exatamente o lockfile versionado:

```bash
npm ci
```

**Observação sobre o `.npmrc`:** a raiz do monorepo contém um `.npmrc` com `legacy-peer-deps=true`. Isso contorna um bug do **npm 10.x** (`@npmcli/arborist`) que falha com `Cannot read properties of null (reading 'edgesOut')` ao resolver o conjunto de *peers* opcionais do `vitest@4` — dependência da aplicação `apps/api`. **Não é um problema do NestJS** e não decorre das versões declaradas em `package.json`. Com **npm ≥ 11** o bug não se reproduz e o `.npmrc` pode ser removido sem efeito colateral. Não altere as versões já declaradas em `apps/api/package.json` e `apps/web/package.json`.

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

Os testes e2e da API rodam separadamente:

```bash
npm --prefix apps/api run test:e2e
```

## Lint

A partir da raiz:

```bash
npm run lint
```

Ou em cada aplicação:

- API: `cd apps/api && npm run lint`
- Web: `cd apps/web && npm run lint`

## Build

A partir da raiz, o script `build` compila as duas aplicações:

```bash
npm run build
```

Ou em cada aplicação:

- API: `cd apps/api && npm run build`
- Frontend: `cd apps/web && npm run build`

## Documentação

- [docs/prd.md](docs/prd.md) — requisitos, histórias e regras de negócio
- [docs/user-flows.md](docs/user-flows.md) — jornadas dos usuários e pontos de desistência
- [docs/design-tokens.md](docs/design-tokens.md) — paleta, espaçamento, tipografia e protótipo
- [docs/architecture.md](docs/architecture.md) — arquitetura e decisões técnicas
- [docs/checklist.md](docs/checklist.md) — ficha da disciplina e critérios de entrega
