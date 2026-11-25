# AGENTS.md — Stockin (Stock Market Dashboard)

## 1. Project Overview

Stockin (working placeholder name until the owner picks a final one) is a stock market tracking dashboard web app. It lets the user browse stocks, view detailed charts and stats, keep a watchlist/favorites, and manage a simulated portfolio and wallet.

- Personal portfolio project, built as real work, from scratch in a new repo.
- The UI is based on the Figma community file "Trading Stock Market Dashboard Platform UI Kit" by Indra Maulana (CC BY 4.0, attribution required): https://www.figma.com/community/file/1359230717010633204/trading-stock-market-dashboard-platform-ui-kit
- Desktop web first; responsive/mobile layout comes in a later phase.
- The app is dynamic: stock data comes from a real market-data API (provider to be chosen — see API Design).

## 2. Tech Stack

- React + TypeScript
- Tailwind CSS
- Axios for HTTP, always behind the dedicated API layer
- Vite (proposed build tooling — confirm at repo setup)
- No backend and no state-management library in v1 (React state/hooks only)
- Hosting: Vercel, with serverless functions used to hide the API key in production

## 3. Architecture

- Frontend-only SPA in v1: no backend, no database.
- Watchlist, favorites, portfolio, and wallet state live client-side (browser storage).
- Feature-based (domain) modules: each feature owns its components, hooks, and services.
- A single shared API layer isolates the market-data provider from the rest of the app, so the provider can be swapped without touching feature code.
- Later phases: backend + database for persistence, then the responsive/mobile pass.

## 4. Folder Structure

```
src/
  features/
    auth/
    dashboard/
    stocks/
    stock-detail/
    favorites/
    wallet/
    community/
    profile/
    contact/
  components/       # shared UI primitives only
  services/api/     # Axios instance + endpoint functions (the API layer)
  hooks/            # shared hooks
  types/            # shared types
  lib/              # utilities
  assets/
```

Each feature folder contains its own `components/`, `hooks/`, `services/` ,`pages/` (feature-level calls that go through the API layer), and `types.ts`.

## 5. Coding Conventions

Proposed by the assistant, pending the owner's review:

- TypeScript everywhere.
- camelCase for variables and functions; PascalCase for components and types.
- ESLint + Prettier; format before committing.
- Conventional Commits for messages (feat:, fix:, chore:, ...).
- No comments inside code (see AI Instructions).

## 6. API Design

- The market-data provider is not chosen yet. The API layer must hide it behind internal functions (e.g. `getQuote(symbol)`, `getCandles(symbol, range)`), so swapping providers touches one folder only.
- All HTTP goes through one configured Axios instance (base URL, headers, key injection) in `services/api`. Components and pages never import axios directly.
- Errors are normalized inside the API layer into one consistent shape before reaching the UI.
- The API key is injected from environment variables only, never hardcoded (see Security Rules).

## 7. Business Logic

- Simulated trading only. The wallet starts with a virtual balance ($10,000 default) and buy/sell adjusts the virtual balance and portfolio locally. No real money, no payment processing.
- The add-card screen in the design is UI only: no real card data is collected or stored.
- Watchlist and favorites are client-side only in v1.
- Community screens show the news/articles layout from the design; no user-generated content in v1.

## 8. Security Rules

- Secrets and API keys never enter the repo: `.env` is gitignored, and no keys appear in code, config, or commit history.
- Development: the key lives in the local `.env`.
- Production on Vercel: the key lives in serverless functions; the frontend calls the function, never the provider directly with the key.
- No real personal or payment data anywhere in the app.

## 9. AI Instructions

- Do nothing unless the owner explicitly asks: no commits, no pushes, no dependency installs, no refactors on your own initiative.
- Never write comments inside code.
- Never write secrets, tokens, or keys in code.
- Do not invent features, screens, endpoints, or business logic that are not in the Figma design or explicitly requested.
- Follow the Figma design exactly: nothing added, nothing removed.
- Ask before changing the folder structure, adding a library, or deviating from anything in this file.
