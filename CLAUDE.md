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

This is a minimal, single-page Vite + React 19 app with no router and no external state management or backend. `App` owns the transactions state and composes three presentational/stateful child components:

- `src/main.jsx` — entry point, mounts `<App />` into `#root` inside `StrictMode`.
- `src/App.jsx` — owns the `transactions` state (seeded with hardcoded sample data; in-memory only, resets on reload, no persistence layer) and the static `categories` list. Passes `transactions` down to `Summary` and `TransactionList`, and passes an `onAddTransaction` callback (`handleAddTransaction`) down to `TransactionForm` to append new transactions.
- `src/Summary.jsx` — derives `totalIncome`, `totalExpenses`, and `balance` from the `transactions` prop and renders the three summary cards.
- `src/TransactionForm.jsx` — owns its own form field state (description, amount, type, category) and the add-transaction form UI. On submit, builds the new transaction object and invokes `onAddTransaction`, then resets its local fields.
- `src/TransactionList.jsx` — owns its own filter state (`filterType`, `filterCategory`) and renders the type/category filter selects plus the transactions table, filtering the `transactions` prop it receives.
- `src/App.css` / `src/index.css` — styling.

Because state is split by concern (transactions in `App`, form fields in `TransactionForm`, filters in `TransactionList`), most feature work involves editing the specific component that owns the relevant state rather than `App.jsx` directly — e.g. new form fields go in `TransactionForm.jsx`, new filters go in `TransactionList.jsx`, new derived totals go in `Summary.jsx`.

ESLint config (`eslint.config.js`) uses the flat config format with `react-hooks` and `react-refresh` plugin rules; `no-unused-vars` is configured to ignore uppercase-prefixed identifiers.
