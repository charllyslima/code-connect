# AGENTS.md

> AI agent instructions for **Code Connect**.

## Summary

pnpm monorepo: **React 19 + Vite + Tailwind** with **Atomic Design** (`apps/web`), and a **RESTful NestJS 11** API (`apps/api`). Every UI component requires a co-located test for essential usage. All git commits use **Conventional Commits**.

## Quick start

```bash
pnpm install
pnpm dev
pnpm build
pnpm lint
pnpm test:api
pnpm test:web    # when Vitest is configured
```

## Repository map

```
code-connect/
├── apps/
│   ├── api/          # REST modules, controllers, services, DTOs
│   └── web/
│       └── src/
│           ├── components/{atoms,molecules,organisms,templates}/
│           └── pages/
├── docs/
│   ├── ARCHITECTURE.md
│   └── CONVENTIONS.md
└── .cursor/rules/
```

## Documentation

| Doc | Purpose |
|-----|---------|
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | System design, atomic layers, REST flow |
| [docs/CONVENTIONS.md](docs/CONVENTIONS.md) | Atomic Design, Tailwind, REST, commits, tests |
| [.cursor/rules/](.cursor/rules/) | Scoped rules including `git-commits.mdc` |

## Working in this repo

### Do

- Place UI in the correct atomic layer; style with **Tailwind** only
- Add `ComponentName.test.tsx` for every new component (essential usage)
- Design API endpoints as **REST resources** with correct HTTP methods and status codes
- Use DTOs + validation on API inputs; keep controllers thin
- Commit with Conventional Commits: `feat(web):`, `fix(api):`, `test(web):`
- Run `pnpm lint` and the relevant test scripts after substantive changes

### Don't

- Ship components without tests
- Use verb-based URLs (`/getUsers`) or wrong status codes on the API
- Add component `.css` for styling (use Tailwind)
- Write vague commit messages (`update`, `wip`)
- Commit secrets or `.env` files

## Stack reference

| Layer    | Path       | Tech |
| -------- | ---------- | ---- |
| Frontend | `apps/web` | React 19, Vite, Tailwind, Atomic Design, Vitest + RTL |
| Backend  | `apps/api` | NestJS 11, REST, Jest |
| Git      | repo root  | Conventional Commits |
