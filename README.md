# Code Connect

<img width="1519" height="1080" alt="image" src="https://github.com/user-attachments/assets/25ef40ba-3561-47b2-b111-de3a39c3b256" />

Educational full-stack monorepo developed as part of the [AI-Native Software Engineering: orquestre agentes para desenvolver com método, segurança e escala](https://cursos.alura.com.br/formacao-ai-native-software-engineering) formation from [Alura](https://www.alura.com.br/).

This repository follows the course curriculum while incorporating minor personal modifications and experiments on top of what was presented in the lessons.

## About

Code Connect is a learning project focused on **AI-native software engineering**: orchestrating agents to build software with method, safety, and scale. The codebase is organized as a pnpm workspace with a React frontend and a NestJS API backend.

> **Note:** This is an educational project. It is not affiliated with or endorsed by Alura.

## Tech stack

| Layer    | Stack                          |
| -------- | ------------------------------ |
| Frontend | React 19, TypeScript, Vite     |
| Backend  | NestJS 11, TypeScript          |
| Tooling  | pnpm workspaces, ESLint        |

## Project structure

```
code-connect/
├── apps/
│   ├── api/          # NestJS REST API
│   └── web/          # React SPA (Vite)
├── package.json      # Root scripts
└── pnpm-workspace.yaml
```

## Prerequisites

- [Node.js](https://nodejs.org/) (LTS recommended)
- [pnpm](https://pnpm.io/installation)

## Getting started

Install dependencies from the repository root:

```bash
pnpm install
```

### Development

Run both apps in parallel:

```bash
pnpm dev
```

Or run them individually:

```bash
pnpm dev:web   # Frontend — default Vite dev server
pnpm dev:api   # Backend — NestJS watch mode (port 3000)
```

### Build

```bash
pnpm build
```

### Lint

```bash
pnpm lint
```

### Tests (API)

```bash
pnpm test:api
pnpm test:api:e2e
```

## Scripts

| Command            | Description                    |
| ------------------ | ------------------------------ |
| `pnpm dev`         | Start web and API in parallel  |
| `pnpm dev:web`     | Start the frontend dev server  |
| `pnpm dev:api`     | Start the API in watch mode    |
| `pnpm build`       | Build API and web              |
| `pnpm lint`        | Lint both apps                 |
| `pnpm preview:web` | Preview the production web build |
| `pnpm start:api`   | Run the API in production mode |

## Acknowledgments

- [Alura](https://www.alura.com.br/) — course content and learning path
- Formation: [AI-Native Software Engineering](https://cursos.alura.com.br/formacao-ai-native-software-engineering)

## License

This project is licensed under the [MIT License](LICENSE).
