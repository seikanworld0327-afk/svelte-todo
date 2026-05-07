# チームレビュー観点 (for Gemini Code Assist)

この文書は Gemini Code Assist がレビュー時に参照するチーム規約です。
人間向けに読めるマークダウンで書けば Gemini が解釈します。

## レビューラベル

各指摘の冒頭に以下のいずれかのラベルバッジを付けてください。

| ラベル | バッジ | 意味 |
|---|---|---|
| must | `![must](https://img.shields.io/badge/review-must-critical.svg)` | マージ前に必ず修正が必要 |
| ask | `![ask](https://img.shields.io/badge/review-ask-9b71ae.svg)` | 意図や背景の確認 |
| imo | `![imo](https://img.shields.io/badge/review-imo-yellow.svg)` | 改善提案 (任意対応) |
| nits | `![nits](https://img.shields.io/badge/review-nits-inactive.svg)` | 些細な指摘 |
| good | `![good](https://img.shields.io/badge/review-good-success.svg)` | 良い実装の称賛 |

## アラート

特に強調したい指摘には GitHub のアラート構文を併用してください。

| 種別 | 用途 |
|---|---|
| `> [!WARNING]` | 問題を回避するために直ちに注意を払う必要がある緊急情報 |
| `> [!CAUTION]` | 特定の行動のリスクやマイナスの結果についてのアドバイス |
| `> [!NOTE]` | 知っておくと有用な補足情報 |
| `> [!TIP]` | より良い書き方のヒント (ライブラリの便利機能、新しい言語機能など、コードベース外の知識共有) |

## 観点とラベルの対応

### セキュリティ → `must` + `> [!CAUTION]`

- ハードコードされた秘密情報 (API キー / パスワード / トークン) がないか
- ユーザー入力のサニタイズ / パラメータ化クエリ
- 認可チェック漏れ

### エラーハンドリング → `must` または `imo`

- 例外を握りつぶしていないか (`must`)
- 境界 (外部 API / ユーザー入力) でのバリデーション (`must`)
- リトライ・タイムアウトの考慮 (`imo`)

### 可読性 → `imo` または `nits`

- 関数 50 行以内 / ファイル 800 行以内 / ネスト 4 段以内 (`imo`)
- マジックナンバーは定数化されているか (`imo`)
- 命名・空白・コメントの細かな指摘 (`nits`)

### 良い実装 → `good`

設計判断が優れている、テストが丁寧、適切な抽象化など、積極的に拾って称賛してください。

### タイポ → `nits`

コード内・ドキュメント問わず、タイポを見つけたら指摘してください。

## コメント例

````markdown
![must](https://img.shields.io/badge/review-must-critical.svg)

> [!CAUTION]
> ここでユーザー入力を直接 SQL に展開しているため SQL インジェクションのリスクがあります。
> プレースホルダを使用する形に修正した方が良いです。
````

````markdown
![imo](https://img.shields.io/badge/review-imo-yellow.svg)

この関数は 80 行を超えているため、〜の責務を別関数に切り出すと読みやすくなりそうです。
````

````markdown
![nits](https://img.shields.io/badge/review-nits-inactive.svg)

> [!TIP]
> PHP 8.1 から Enum が使えるので、こうした定数群は Enum 化するとより安全になります。
````

````markdown
![nits](https://img.shields.io/badge/review-nits-inactive.svg)

`recieve` は `receive` のタイポです。
````

## 指摘のトーン

- **言語: 日本語で記述する**
- 断定ではなく提案ベース ("〜した方が良いかもしれません")
- 必ず理由を添える ("なぜならこのパスは外部から呼ばれるため...")

## 対象外

- テストファイル内のマジックナンバー
- 自動生成コード (`*_pb.go`, `*.gen.ts` など)
