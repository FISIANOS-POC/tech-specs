# AGENTS.md — AI Coding Assistant Guidelines

## Project Context

This repository (`tech-specs`) is the **root workspace** for the FISIANOS POC project. It contains specifications, documentation, and architecture diagrams. The two application repositories (`tech-frontend` and `tech-backend`) are **cloned inside this directory** as independent Git repositories. They are listed in `.gitignore` so `tech-specs` does NOT track their contents — each repo has its own `.git` history and remote.

> **IMPORTANT**: `tech-frontend/` and `tech-backend/` are **independent Git repos**, NOT subdirectories of `tech-specs`. They each push to their own GitHub remote. Do NOT commit frontend/backend code changes via the `tech-specs` git — use each project's own git context.

## Workspace Structure (Local Filesystem)

```
tech-specs/                          ← THIS repo (GitHub: FISIANOS-POC/tech-specs)
├── .gitignore                       # Ignores tech-frontend/ and tech-backend/
├── AGENTS.md                        # This file — AI assistant rules
├── architecture.md                  # Mermaid.js architecture diagram
├── README.md                        # Repository README
├── specs/
│   └── overview.md                  # High-level project overview
│
├── tech-frontend/                   ← INDEPENDENT repo (GitHub: FISIANOS-POC/tech-frontend)
│   ├── .git/                        # Its own Git history
│   ├── package.json
│   ├── vite.config.ts
│   ├── src/
│   └── ...
│
└── tech-backend/                    ← INDEPENDENT repo (GitHub: FISIANOS-POC/tech-backend)
    ├── .git/                        # Its own Git history
    ├── pom.xml
    ├── src/
    └── ...
```

## Git Remotes

| Directory          | GitHub Repository                              | Branch  |
| ------------------ | ---------------------------------------------- | ------- |
| `tech-specs/`      | `github.com/FISIANOS-POC/tech-specs`           | `main`  |
| `tech-frontend/`   | `github.com/FISIANOS-POC/tech-frontend`        | `main`  |
| `tech-backend/`    | `github.com/FISIANOS-POC/tech-backend`         | `main`  |

## Rules for AI Assistants

### General Rules

1. **Do NOT place frontend or backend code in this repository.** This repo is exclusively for documentation, specifications, and architecture diagrams.
2. **Follow Markdown best practices.** Use proper heading hierarchy, code fences with language identifiers, and consistent formatting.
3. **Keep documentation up to date.** When architectural decisions change, update the relevant spec files accordingly.
4. **Use Mermaid.js for diagrams.** All architecture and flow diagrams should use Mermaid syntax for version-control-friendly visualization.

### Frontend Rules (for `tech-frontend`)

1. **Language**: TypeScript (strict mode). No `any` types unless absolutely necessary.
2. **Framework**: React 18+ with functional components and hooks.
3. **Build Tool**: Vite. Do not introduce Webpack or other bundlers.
4. **Package Manager**: Yarn. Do not use npm or pnpm.
5. **Styling**: Vanilla CSS or CSS Modules. Avoid CSS-in-JS unless explicitly requested.
6. **API Calls**: Use `fetch` or a lightweight HTTP client. Centralize API calls in a dedicated `services/` or `api/` directory.
7. **State Management**: Start with React's built-in state (useState, useContext, useReducer). Only introduce external libraries (Redux, Zustand) when justified.
8. **File Naming**: Use PascalCase for components (`MyComponent.tsx`), camelCase for utilities (`formatDate.ts`).

### Backend Rules (for `tech-backend`)

1. **Language**: Java 17+.
2. **Framework**: Spring Boot 3.x with Spring WebFlux.
3. **Reactive Types**: Use `Mono<T>` for single values and `Flux<T>` for streams. Never block reactive streams with `.block()` in production code.
4. **Build Tool**: Maven. Follow standard Maven project layout.
5. **API Design**: RESTful JSON endpoints. Use proper HTTP status codes and consistent error response structures.
6. **Package Structure**: Organize by feature (e.g., `com.fisianos.techbackend.user`) rather than by layer.
7. **Configuration**: Use `application.yml` over `application.properties` for readability.
8. **CORS**: Configure CORS globally via a `WebFluxConfigurer` bean to allow frontend origins.
9. **Testing**: Use `WebTestClient` for integration tests of reactive endpoints.

### Commit & Branch Conventions

- **Commit Messages**: Use conventional commits format: `type(scope): description`
  - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
  - Example: `feat(api): add user endpoint with reactive CRUD`
- **Branch Strategy**: `main` for stable code. Feature branches named `feature/<short-description>`.

### Security

- Never commit secrets, API keys, or tokens to any repository.
- Use environment variables or Spring Boot's externalized configuration for sensitive values.
- Add sensitive files to `.gitignore`.
