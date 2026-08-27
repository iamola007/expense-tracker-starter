# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A basic expense tracker built with React + Vite. This is the starter project for a Claude Code course — it intentionally has a bug, poor UI, and messy code, meant to be fixed incrementally during the course.

## Commands

```bash
npm install      # install dependencies
npm run dev      # start Vite dev server at http://localhost:5173
npm run build    # production build
npm run preview  # preview the production build locally
npm run lint     # run ESLint over the project
```

There is no test suite or test runner configured.

## Architecture

This is a single-component app: essentially all state, logic, and rendering for the entire application lives in `src/App.jsx`. There is no router, no state management library, no backend/API, and no component decomposition — the summary cards, add-transaction form, and filterable transaction table are all inline JSX within one `App` function.

- Transactions are held in-memory via `useState` (seeded with hardcoded sample data) and are lost on page reload — there is no persistence layer.
- `amount` is stored and passed around as a **string** (it comes straight from the text/number input's `value`), not converted to a `Number`. Income/expense totals are computed with `reduce((sum, t) => sum + t.amount, 0)`, which does string concatenation instead of numeric addition — this is the app's known intentional bug, producing garbled totals (e.g. `$0120015080095654515`) once more than one transaction exists.
- Filtering (`filterType`, `filterCategory`) is done by deriving a `filteredTransactions` array on every render rather than via `useMemo` or a reducer.
- Styling is plain CSS in `src/App.css` / `src/index.css`, no CSS framework.
