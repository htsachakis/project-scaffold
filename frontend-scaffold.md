# Frontend Project Scaffold — React + Vite

## Purpose

Use this document to scaffold a new frontend project or refactor an existing one to match the following opinionated folder structure. Follow every section exactly. Do not invent new top-level folders. Adapt file extensions to the stack in use (`.jsx`/`.tsx`, `.js`/`.ts`).

---

## Target Folder Structure

```
frontend/
├── .docs/                     # Model-facing project intelligence (never shipped)
│   ├── architecture/          # Architecture Decision Records (ADRs)
│   ├── encyclopedia/          # Domain glossary and concept definitions
│   ├── quick-start.md         # Fastest path to running the project
│   ├── scripts.md             # All runnable scripts explained
│   └── model-instructions.md  # How models must read, write, and behave in this repo
├── .plans/                    # AI-generated plans and feature specs (never shipped)
│   ├── features/              # One plan file per planned feature
│   ├── refactors/             # Refactor plans and migration notes
│   └── _template.md           # Blank plan template every model must use
├── docs/                      # Human-facing project documentation (can be shipped/published)
│   ├── getting-started.md
│   ├── api-reference.md
│   └── deployment.md
├── public/                    # Static files served as-is (favicon, robots.txt, etc.)
├── src/
│   ├── assets/                # Images, fonts, icons, and other static assets
│   ├── components/            # Reusable, generic UI components (no business logic)
│   ├── layout/                # Layout shell components (Header, Footer, Sidebar, etc.)
│   ├── pages/                 # Route-level page components (one file per route)
│   ├── features/              # Feature-based modules (self-contained, modular)
│   ├── hooks/                 # Custom React hooks (use* naming convention)
│   ├── context/               # React Context providers for global/shared state
│   ├── redux/                 # Redux store, slices, selectors, and middleware
│   ├── services/              # API clients, external service integrations
│   └── utils/                 # Pure helper functions and utilities
│   ├── App.jsx                # Root component — mounts router and global providers
│   ├── index.css              # Global base styles / CSS reset
│   └── main.jsx               # Entry point — renders <App /> into the DOM
├── AGENTS.md                  # AI agent / CLI instructions (Claude, Cursor, Copilot, etc.)
├── CONTRIBUTING.md            # Human and AI contribution rules
├── .eslintrc.json             # ESLint configuration
├── .gitignore
├── package.json
├── README.md
└── vite.config.js             # Vite build configuration
```

---

## Directory Rules

### `.docs/`
- **Never bundled or deployed.** This folder is for models and developers only.
- Contains the "brain" of the project: decisions, vocabulary, and behavioral instructions.
- Sub-structure:

```
.docs/
├── architecture/
│   └── 001-use-redux-toolkit.md    # One ADR per decision (numbered, immutable)
├── encyclopedia/
│   └── glossary.md                 # Domain terms, abbreviations, concepts
├── quick-start.md                  # Clone → install → run in under 5 steps
├── scripts.md                      # Every package.json script documented with purpose + example
└── model-instructions.md           # The law for AI models working in this repo
```

**`model-instructions.md` must include:**
- Which files a model is allowed to create, edit, and delete
- Naming conventions to enforce
- Patterns that are forbidden (e.g. no default exports, no inline styles)
- How to write a plan before touching code
- How to update `.plans/` and `AGENTS.md` after completing work
- Tone and comment style

### `.plans/`
- **Never bundled or deployed.** All AI-generated and human-written plans live here.
- A plan must exist before a model writes any non-trivial code. No plan = no code.
- Sub-structure:

```
.plans/
├── features/
│   └── 2024-01-auth-feature.md     # Date-prefixed, one file per feature plan
├── refactors/
│   └── 2024-02-migrate-to-redux.md
└── _template.md                    # Every new plan must start from this template
```

**`_template.md` must contain these sections:**
```markdown
# Plan: [Feature / Refactor Name]

## Status
[ ] Draft | [ ] Approved | [ ] In Progress | [ ] Done

## Goal
One paragraph. What problem does this solve?

## Scope
- IN: ...
- OUT: ...

## Steps
1. ...
2. ...

## Files to Create
- path/to/file.jsx — purpose

## Files to Modify
- path/to/existing.js — what changes and why

## Open Questions
- ...

## Definition of Done
- [ ] ...
```

### `docs/`
- **Human-facing documentation** — written for developers, stakeholders, or end users.
- May be published (GitHub Pages, Notion, etc.).
- Keep in sync with the code; outdated docs are worse than no docs.
- Required files:

| File | Purpose |
|------|---------|
| `getting-started.md` | Full local setup guide |
| `api-reference.md` | All endpoints / SDK methods |
| `deployment.md` | How to build and ship to each environment |

### `AGENTS.md`
- **The single source of truth for any AI CLI tool** (Claude Code, Cursor, GitHub Copilot, Aider, etc.).
- Loaded automatically by agents that support `AGENTS.md` / `CLAUDE.md` conventions.
- Must be kept at the project root.

**Required sections in `AGENTS.md`:**
```markdown
# AGENTS.md — [Project Name]

## Project Overview
Short description of what this project is and what it does.

## Stack
- Runtime: Node 20 / Bun
- Framework: React 18 + Vite
- State: Redux Toolkit + React Context
- Styling: [Tailwind / CSS Modules]
- Testing: Vitest + React Testing Library

## Key Directories
| Path | Purpose |
|------|---------|
| src/features/ | All domain logic lives here |
| src/components/ | Generic UI only |
| .plans/ | Read the relevant plan before writing code |
| .docs/ | Read model-instructions.md before anything else |

## Before You Write Any Code
1. Read `.docs/model-instructions.md`
2. Read or create a plan in `.plans/features/` or `.plans/refactors/`
3. Check CONTRIBUTING.md for branch and commit rules

## Commands
| Command | Use |
|---------|-----|
| `npm run dev` | Start dev server |
| `npm run build` | Production build |
| `npm run test` | Run all tests |
| `npm run lint` | ESLint check |

## Absolute Rules
- Never commit directly to `main`
- Never delete `.plans/` or `.docs/` entries
- Never use `any` in TypeScript
- Always write a test for new utils and hooks
```

### `CONTRIBUTING.md`
- Rules for **humans and AI agents** contributing to the project.
- Must be read before opening any PR.

**Required sections in `CONTRIBUTING.md`:**
```markdown
# Contributing

## Workflow
1. Branch from `main`: `git checkout -b feat/<name>` or `fix/<name>`
2. Write or update the plan in `.plans/` before coding
3. Follow naming conventions in `.docs/model-instructions.md`
4. Open a PR with the provided template

## Branch Naming
| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feat/<slug>` | `feat/user-auth` |
| Bug fix | `fix/<slug>` | `fix/login-redirect` |
| Refactor | `refactor/<slug>` | `refactor/redux-migration` |
| Docs | `docs/<slug>` | `docs/api-reference` |

## Commit Style (Conventional Commits)
```
<type>(<scope>): <short description>

feat(auth): add JWT refresh logic
fix(header): correct mobile nav z-index
docs(plans): add auth feature plan
```

## Pull Request Rules
- Title must follow Conventional Commits format
- PR description must link to the `.plans/` file it implements
- All checks must pass before merge
- At least one human review required for `main`
- AI-generated PRs must include `[AI]` tag in the title

## What Models Must NOT Do
- Modify `.gitignore`, `package.json` scripts, or CI config without a human-approved plan
- Merge their own PRs
- Delete or rewrite history in `.plans/` or `.docs/`
- Add dependencies without noting them in the plan
```

---

### `public/`
- Files here are **not processed by the bundler** — they are copied verbatim to the build output.
- Use for: `favicon.ico`, `robots.txt`, open-graph images, and third-party scripts that must remain unmodified.

### `src/assets/`
- Everything imported via JavaScript (images, SVGs, fonts).
- Organise into subfolders as needed: `assets/icons/`, `assets/images/`, `assets/fonts/`.

### `src/components/`
- **Generic, reusable UI pieces** with no knowledge of business domain.
- Examples: `Button`, `Modal`, `Input`, `Card`, `Spinner`.
- Each component lives in its own folder with co-located styles and tests:
  ```
  components/
  └── Button/
      ├── Button.jsx
      ├── Button.test.jsx
      └── Button.module.css   (or Button.styles.ts)
  ```

### `src/layout/`
- Components that define the **page shell**: `Header`, `Footer`, `Sidebar`, `MainLayout`.
- Imported once at the router / app level — not scattered across pages.

### `src/pages/`
- One file per route.
- Pages are **thin** — they import features/components, pass data down, and define layout composition.
- Naming should mirror the route: `HomePage.jsx`, `DashboardPage.jsx`, `NotFoundPage.jsx`.

### `src/features/`
- **The core of the app.** Each feature is a self-contained module.
- A feature owns its own components, hooks, services, and state that are NOT shared.
- Structure:
  ```
  features/
  └── auth/
      ├── components/
      ├── hooks/
      ├── authSlice.js        (if using Redux)
      ├── authService.js
      └── index.js            (public API — export only what other features need)
  ```
- Prefer importing from a feature's `index.js` to keep boundaries clean.

### `src/hooks/`
- **Shared** custom hooks used across multiple features or pages.
- Feature-specific hooks stay inside `features/<name>/hooks/`.
- Always prefix with `use`: `useDebounce`, `useWindowSize`, `useLocalStorage`.

### `src/context/`
- React Context providers for **global** state that does not belong in Redux (theme, locale, auth session).
- Each context is a standalone file: `ThemeContext.jsx`, `AuthContext.jsx`.
- Export both the context and its provider component.

### `src/redux/`
- Redux Toolkit setup.
- Structure:
  ```
  redux/
  ├── store.js                # configureStore — imports all slice reducers
  └── (feature slices live in features/<name>/authSlice.js)
  ```
- Do not put slice files directly here; keep them co-located inside `features/`.

### `src/services/`
- **Shared** API clients and third-party integrations (Axios instance, Supabase client, analytics).
- Feature-specific API calls stay inside `features/<name>/`.

### `src/utils/`
- Pure functions with zero side effects.
- Examples: `formatDate`, `slugify`, `clamp`, `cn` (classname helper).
- Each util should have a unit test.

---

## Conventions

| Rule | Detail |
|------|--------|
| Component files | PascalCase (`UserCard.jsx`) |
| Hook files | camelCase with `use` prefix (`useAuth.js`) |
| Utility files | camelCase (`formatDate.js`) |
| Page files | PascalCase + `Page` suffix (`DashboardPage.jsx`) |
| Slice files | camelCase + `Slice` suffix (`authSlice.js`) |
| Service files | camelCase + `Service` suffix (`userService.js`) |
| Index barrel files | Allowed **only** at feature boundaries, not everywhere |
| CSS approach | Module CSS or utility-first (Tailwind) — pick one, stay consistent |

---

## Scaffold Instructions for AI Models

When scaffolding or refactoring, follow these steps in order:

**Phase 0 — Project Intelligence (do this first, always)**
1. **Generate `.docs/model-instructions.md`** — fill in naming rules, forbidden patterns, and model behavior for this specific project.
2. **Generate `.docs/quick-start.md`** — clone, install, run.
3. **Generate `.docs/scripts.md`** — document every `package.json` script.
4. **Generate `.docs/encyclopedia/glossary.md`** — seed with any domain terms from the project brief.
5. **Generate `.plans/_template.md`** — use the template shown in the `.plans/` section above verbatim.
6. **Generate `AGENTS.md`** — fill in project name, stack, key directories, and commands from the project brief.
7. **Generate `CONTRIBUTING.md`** — use the template shown in the `CONTRIBUTING.md` section above.

**Phase 1 — Source Scaffold**
8. **Create the `src/` directory tree** exactly as shown. Do not add extra folders.
9. **Generate `main.jsx`** — mounts `<App />`, wraps with Redux `<Provider>` and any Context providers.
10. **Generate `App.jsx`** — sets up the router (React Router v6 `<Routes>`), imports `Layout`, maps routes to pages.
11. **Generate `layout/MainLayout.jsx`** — renders `<Header />`, `<Outlet />`, `<Footer />`.
12. **Generate stub page files** for each route specified in the project brief.
13. **Generate `redux/store.js`** with an empty root reducer (add slices as features are built).
14. **Generate `vite.config.js`** with path aliases: `@` → `src/`, `@components` → `src/components/`, etc.
15. **Generate `.eslintrc.json`** with React + hooks rules enabled.

**Phase 2 — Features**
16. For each feature requested: write a plan in `.plans/features/<date>-<name>.md` first, then create `src/features/<name>/`.
17. **Do not generate placeholder lorem-ipsum content.** Use realistic but minimal stub data.

---

## Vite Path Alias Reference

```js
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@components': path.resolve(__dirname, 'src/components'),
      '@features': path.resolve(__dirname, 'src/features'),
      '@pages': path.resolve(__dirname, 'src/pages'),
      '@hooks': path.resolve(__dirname, 'src/hooks'),
      '@utils': path.resolve(__dirname, 'src/utils'),
      '@services': path.resolve(__dirname, 'src/services'),
      '@assets': path.resolve(__dirname, 'src/assets'),
    },
  },
})
```

---

## Refactor Checklist

When applied to an existing project, verify each item:

**Project Intelligence**
- [ ] `.docs/model-instructions.md` exists and reflects current project rules
- [ ] `.docs/quick-start.md` is accurate and runnable
- [ ] `.docs/scripts.md` documents all `package.json` scripts
- [ ] `AGENTS.md` is at the root and reflects current stack + commands
- [ ] `CONTRIBUTING.md` is at the root with branch, commit, and PR rules
- [ ] `.plans/_template.md` exists for future plans
- [ ] A plan was written in `.plans/` before any refactor code was touched

**Source Structure**
- [ ] All reusable UI moved to `components/` — no business logic inside
- [ ] All route components moved to `pages/` — one file per route
- [ ] All domain logic extracted into `features/<name>/`
- [ ] Shared hooks extracted to `hooks/`
- [ ] API calls isolated in `services/` or `features/<name>/<name>Service.js`
- [ ] Global state split: Context for UI state, Redux for server/domain state
- [ ] No cross-feature imports except through a feature's `index.js`
- [ ] Path aliases configured and used consistently (`@components/...` not `../../components/...`)
- [ ] `public/` contains only unprocessed static assets
- [ ] `assets/` contains only imported static assets

---

*Generated from the frontend architecture diagram. Apply uniformly across all new and refactored projects.*
