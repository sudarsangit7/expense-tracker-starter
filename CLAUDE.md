# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Starter project for a Claude Code course (codewithmosh.com). A small expense/income tracker built with React + Vite. The app **intentionally** ships with a bug, poor UI, and messy code — these are meant to be found and fixed incrementally, so don't assume rough edges in `src/App.jsx` are accidental unless asked to investigate them.

## Commands

```bash
npm install      # install dependencies
npm run dev      # start Vite dev server at http://localhost:5173
npm run build    # production build (outputs to dist/)
npm run preview  # preview the production build locally
npm run lint     # run ESLint over the project
```

There is no test suite configured in this repo.

## Architecture

This is a minimal, single-page Vite + React 19 app with no router and no external state management or backend — everything lives in one component:

- `src/main.jsx` — entry point, mounts `<App />` into `#root` inside `StrictMode`.
- `src/App.jsx` — the entire application. All state (transactions list and form/filter inputs), derived totals (income/expenses/balance), filtering logic, and the add-transaction form/table UI live in this single component via `useState` and inline computations. Transaction data is in-memory only (seeded with hardcoded sample transactions) and resets on reload — there is no persistence layer.
- `src/App.css` / `src/index.css` — styling.

Because everything is colocated in one component, most changes (bug fixes, new fields, filters, persistence, splitting into subcomponents) will involve editing `src/App.jsx` directly rather than tracing logic across multiple files.

ESLint config (`eslint.config.js`) uses the flat config format with `react-hooks` and `react-refresh` plugin rules; `no-unused-vars` is configured to ignore uppercase-prefixed identifiers.
