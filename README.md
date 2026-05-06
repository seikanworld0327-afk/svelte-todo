# sv

Everything you need to build a Svelte project, powered by [`sv`](https://github.com/sveltejs/cli).

## 機能

- タスクの追加・削除・完了切り替え
- 残件数 / 完了数の表示
- 完了タスクの一括削除
- localStorage への自動保存

## 技術スタック

- [SvelteKit](https://svelte.dev/docs/kit) — フルスタックフレームワーク
- [Svelte 5 Runes](https://svelte.dev/docs/svelte/what-are-runes) — `$state` / `$derived` / `$effect` / `$props` を利用
- [Tailwind CSS](https://tailwindcss.com/) — ユーティリティファーストの CSS
- [TypeScript](https://www.typescriptlang.org/) — 型安全な開発

## 使い方

1. 入力欄にタスク名を入力し `Enter` または **Add** ボタンで追加
2. チェックボックスで完了状態を切り替え
3. **Delete** ボタンで個別削除
4. 一覧下部の「完了したタスクを削除」で完了済みタスクを一括削除

## 開発スクリプト

| コマンド          | 用途                             |
| ----------------- | -------------------------------- |
| `npm run dev`     | 開発サーバを起動                 |
| `npm run build`   | プロダクションビルドを生成       |
| `npm run preview` | プロダクションビルドのプレビュー |
| `npm run check`   | Svelte / TypeScript の型チェック |

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npx sv create

# create a new project in my-app
npx sv create my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.
