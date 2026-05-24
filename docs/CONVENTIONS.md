<!-- last-init: 2026-05-23 -->

# Conventions

## General principles

- **Language**: TypeScript in both apps
- **Formatting**: API — Prettier + ESLint; Web — ESLint (semicolon-free app code)
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/) for **all** changes (API, web, root)

### Conventional Commits

```
<type>(<scope>): <description>
```

| Field   | Values |
| ------- | ------ |
| type    | `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`, `build`, `perf` |
| scope   | `api`, `web`, `deps`, or omitted for repo-wide |
| description | Imperative, lowercase, ~72 chars max |

Examples: `feat(web): add Input atom`, `fix(api): return 404 for unknown user`, `test(web): cover Modal close on escape`.

## Naming

| Element        | Convention           | Example |
| -------------- | -------------------- | ------- |
| Nest files     | `kebab-case.role.ts` | `users.controller.ts` |
| React component| `PascalCase` folder  | `atoms/Button/Button.tsx` |
| Component test | `PascalCase.test.tsx`| `Button.test.tsx` |
| API tests      | `*.spec.ts` / `*.e2e-spec.ts` | `users.service.spec.ts` |

## Imports

- Prefer path alias `@/` → `src/` on web when configured (recommended for `components/`, `pages/`)
- API: extensionless imports within `src/`

---

## Frontend

### Atomic Design

| Layer     | Directory     | Examples |
| --------- | ------------- | -------- |
| Atoms     | `components/atoms/` | Button, Input, Label |
| Molecules | `components/molecules/` | SearchBar, FormField |
| Organisms | `components/organisms/` | Header, PostCard |
| Templates | `components/templates/` | DashboardLayout |
| Pages     | `pages/` | HomePage, SettingsPage |

Rules:

- One component per directory with `ComponentName.tsx`, `ComponentName.test.tsx`, `index.ts`
- Composition flows upward only (pages use organisms; atoms do not import organisms)
- Pages orchestrate data fetching and route-level concerns; presentational logic stays in lower layers

### Tailwind CSS

- All component styling via Tailwind utility classes
- Global styles only in `src/index.css` (Tailwind imports, base resets)
- Do not add per-component `.css` files for layout/theme
- Use responsive/state variants (`md:`, `hover:`, `focus-visible:`) instead of custom CSS

### Components & tests

**Every component must have a test** co-located as `ComponentName.test.tsx` that covers **essential usage**:

1. Renders with default/required props
2. Primary interaction (click, type, submit) if the component is interactive
3. Visible outcome users care about (text, role, aria) via Testing Library

Stack: **Vitest** + **React Testing Library** + **user-event**.

Run (when configured): `pnpm test:web` from repository root.

```tsx
// Essential usage — not implementation details
it('submits search query', async () => {
  const onSearch = vi.fn()
  render(<SearchBar onSearch={onSearch} />)
  await userEvent.type(screen.getByRole('searchbox'), 'react')
  await userEvent.click(screen.getByRole('button', { name: /search/i }))
  expect(onSearch).toHaveBeenCalledWith('react')
})
```

---

## Backend — REST

The API is **RESTful**. Controllers expose **resources**, not RPC actions.

### Resource design

- URIs name **resources** with plural nouns: `/users`, `/articles`
- Use **HTTP methods** for actions; paths do not contain verbs (`/createUser` is invalid)
- Use **nouns** in lowercase kebab-case for multi-word resources: `/blog-posts`
- Nest: `@Controller('users')` — base path without leading/trailing slash noise

### HTTP semantics

| Method | Intent        | Typical success |
| ------ | ------------- | --------------- |
| GET    | Read          | 200, 204        |
| POST   | Create        | 201             |
| PUT    | Replace       | 200, 204        |
| PATCH  | Partial update| 200             |
| DELETE | Remove        | 204             |

Use accurate status codes: `400`/`422` validation, `401`/`403` auth, `404` not found, `409` conflict, `500` only for unexpected server faults.

### Request/response

- JSON bodies; validate with DTOs + `ValidationPipe`
- POST create returns `201` and preferably `Location` header pointing to the new resource
- Consistent error JSON; no stack traces in production responses
- **Stateless**: no server-side session for API; authentication via tokens when introduced

### Layering (NestJS)

| Layer      | Responsibility |
| ---------- | -------------- |
| Controller | HTTP mapping, status, headers, param/DTO binding |
| Service    | Business rules, orchestration |
| Repository | Persistence (when added) |

Controllers stay thin; services own domain logic.

### Testing REST

- Unit tests mock services; assert controller delegates correctly
- E2E tests use supertest: method + URL + status + body shape for each endpoint

---

## Testing summary

| App | Framework | Location | Run |
| --- | --------- | -------- | --- |
| API | Jest | `src/**/*.spec.ts`, `test/**/*.e2e-spec.ts` | `pnpm test:api` |
| Web | Vitest + RTL | beside each component | `pnpm test:web` |

---

## Do

- Follow Atomic Design folders and Tailwind on every new UI piece
- Add component tests before considering UI work done
- Design API changes as resources with correct verbs and status codes
- Write Conventional Commit messages for every commit

## Don't

- Commit without Conventional Commit format
- Add React components without `.test.tsx`
- Use RPC-style routes or verbs in URLs on the API
- Use raw CSS files for component styling on the web
- Return wrong HTTP status (e.g. `200` for validation errors)
