# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # start dev server
npm run build        # production build
npm run preview      # preview production build
npm run check        # TypeScript type-check (svelte-check)
npm run check:watch  # type-check in watch mode
npm run lint         # Prettier check + ESLint
npm run format       # auto-format with Prettier
```

There are no automated tests in this project.

## Architecture

This is a SvelteKit single-page todo app with no backend. All application state lives in `src/routes/+page.svelte` and is persisted to `localStorage`.

- **`src/routes/+page.svelte`** — the entire app: todo list state, add/delete/edit logic, and all UI
- **`src/types/todo.ts`** — shared `Todo` type (`{ text: string; done: boolean }`)
- **`src/lib/`** — empty; available for future `$lib` imports
- **`src/app.css`** — Tailwind entry point (`@tailwind base/components/utilities`)

The app uses **Svelte 5 runes** (`$state`, `$effect`) — not the legacy `writable` store pattern. The `$effect` that syncs to `localStorage` is guarded by an `isInitialized` flag to prevent overwriting storage before `onMount` has loaded the saved data.

Deployed via **`@sveltejs/adapter-vercel`**.

## Code Conventions

**Formatting** (enforced by Prettier):
- Single quotes, no trailing commas, 100-char print width, 2-space indent
- `prettier-plugin-svelte` handles `.svelte` files

**TypeScript**: strict mode via `svelte-check`; all `.svelte` scripts use `lang="ts"`

**Size guidelines** (from team style guide):
- Functions ≤ 50 lines, files ≤ 800 lines, nesting ≤ 4 levels deep
- Magic numbers should be extracted as named constants

## Pull Requests

PRs use a Japanese-language template (`.github/PULL_REQUEST_TEMPLATE.md`). Sections: 概要, 背景・目的, 変更内容, スコープ対象外, テスト, 影響範囲, レビュー時の確認ポイント, スクリーンショット/動作ログ, and a self-review checklist.

Code review comments follow a label convention: `must` (blocking), `ask` (clarification), `imo` (optional improvement), `nits` (minor), `good` (praise). Review comments are written in Japanese, phrased as suggestions with reasons.
