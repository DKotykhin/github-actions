# Copilot Instructions

## Project Overview
Create React App (CRA) project with GitHub Actions CI/CD pipeline. Standard React 19 setup with ESLint, Jest, and Testing Library.

## Key Architecture
- **Standard CRA structure**: `src/` for source code, `public/` for static assets
- **Entry point**: `src/index.js` uses React 19's `createRoot` API (not legacy `ReactDOM.render`)
- **Testing**: Jest + React Testing Library configured via `setupTests.js`
- **Linting**: Custom `.eslintrc.json` with accessibility plugins (jsx-a11y), not just CRA defaults

## Development Workflow

### Local Commands
```bash
npm start          # Dev server on :3000
npm test           # Interactive test watch mode
npm run build      # Production build
npm run lint       # ESLint check (required for CI)
```

### CI/CD Pipeline (`.github/workflows/testing.yml`)
- **Triggers**: Push/PR to `main` branch + manual dispatch
- **Two-job workflow**: `test` → `build` (sequential, build depends on test passing)
- **Node version**: 20 (specify this when suggesting Node upgrades)
- **Cache strategy**: Uses `~/.npm` cache with `package-lock.json` hash key
- **Required checks**: Tests AND linting must pass before build runs

## Testing Conventions
- Import from `@jest/globals` for test utilities: `import { test, expect } from '@jest/globals'`
- Use Testing Library queries: `screen.getByText()`, not direct DOM manipulation
- Test file naming: `*.test.js` alongside source files
- Example pattern in `src/App.test.js`: render component → query → assert

## Code Style & Linting
- **ESLint config**: Uses `eslint:recommended` + React + accessibility plugins
- **No prop-types**: Not configured; prefer TypeScript if type safety needed
- **React version detection**: Auto-detected via settings, don't hardcode version
- **Import plugin**: Catches unresolved imports/exports

## Important Notes
- **Don't eject CRA**: Project uses standard CRA - avoid suggesting eject unless explicitly needed
- **Package manager**: Use `npm ci` in CI, `npm install` locally (project uses npm, not yarn/pnpm)
- **React.StrictMode**: Enabled in production - components render twice in dev mode for safety checks
- **GitHub Actions caching**: When modifying workflows, maintain the npm cache strategy for faster builds

## When Adding Features
1. Create component in `src/` directory
2. Add corresponding `.test.js` file with Testing Library tests
3. Ensure ESLint passes (especially jsx-a11y rules for accessibility)
4. Verify CI pipeline passes before merging to `main`
