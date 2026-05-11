# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## コマンド

```bash
npm run dev          # 開発サーバー起動
npm run build        # 本番ビルド
npm run preview      # 本番ビルドのプレビュー
npm run check        # TypeScript 型チェック (svelte-check)
npm run check:watch  # 型チェックのウォッチモード
npm run lint         # Prettier チェック + ESLint
npm run format       # Prettier による自動フォーマット
```

このプロジェクトには自動テストはありません。

## アーキテクチャ

バックエンドを持たない SvelteKit 製のシングルページ Todo アプリ。アプリの状態はすべて `src/routes/+page.svelte` に集約され、`localStorage` に永続化される。

- **`src/routes/+page.svelte`** — アプリ全体: Todo リストの状態管理、追加・削除・編集ロジック、UI
- **`src/types/todo.ts`** — 共通の `Todo` 型 (`{ text: string; done: boolean }`)
- **`src/lib/`** — 現在は空。将来的な `$lib` インポート用
- **`src/app.css`** — Tailwind エントリーポイント (`@tailwind base/components/utilities`)

アプリは **Svelte 5 のルーン** (`$state`, `$effect`) を使用しており、レガシーな `writable` ストアパターンは使わない。`localStorage` に同期する `$effect` は、`onMount` がデータを読み込む前にストレージを上書きしないよう `isInitialized` フラグで保護されている。

デプロイは **`@sveltejs/adapter-vercel`** を使用。

## コード規約

**フォーマット**（Prettier で強制）:
- シングルクォート、末尾カンマなし、1行100文字、インデント2スペース
- `.svelte` ファイルは `prettier-plugin-svelte` で処理

**TypeScript**: `svelte-check` によるストリクトモード。すべての `.svelte` スクリプトは `lang="ts"` を使用

**サイズ指針**（チームスタイルガイドより）:
- 関数は50行以内、ファイルは800行以内、ネストは4段以内
- マジックナンバーは名前付き定数として切り出す

## プルリクエスト

PR には日本語テンプレート (`.github/PULL_REQUEST_TEMPLATE.md`) を使用。セクション構成: 概要、背景・目的、変更内容、スコープ対象外、テスト、影響範囲、レビュー時の確認ポイント、スクリーンショット/動作ログ、セルフレビューチェックリスト。

コードレビューコメントはラベル規約に従う: `must`（マージ前必須修正）、`ask`（意図確認）、`imo`（任意の改善提案）、`nits`（些細な指摘）、`good`（称賛）。レビューコメントは日本語で、理由を添えた提案ベースで記述する。
