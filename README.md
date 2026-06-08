# spec-driven-project-template

Claude Code でスペック駆動開発を行うためのプロジェクトテンプレート。

「何を作るか」を `docs/` に定義し、「今日何をするか」を `.steering/` で管理しながら、Claude Code と協調して実装を進めるワークフローをすぐに始められる。

---

## このテンプレートに含まれるもの

```
.
├── CLAUDE.md                          # プロジェクト概要・規約（要記入）
├── .claude/
│   ├── settings.json                  # スキル許可・フック設定
│   ├── commands/                      # スラッシュコマンド
│   │   ├── setup-project.md           # 初回セットアップ
│   │   ├── add-feature.md             # フィーチャー追加
│   │   ├── update-docs.md             # ドキュメント最新化
│   │   └── checkpoint.md              # 進捗棚卸し
│   ├── hooks/
│   │   ├── lint.sh                    # 自動 Lint（Edit/Write 後）
│   │   └── safety-check.sh            # 安全チェック（Bash 実行前）
│   └── skills/steering/               # ステアリングスキル + テンプレート
├── docs/                              # 永続ドキュメント（/setup-project が生成）
│   ├── product-requirements.md
│   ├── architecture.md
│   ├── development-guidelines.md
│   ├── api-spec.md                    # 外部 I/F がない場合は削除
│   └── data-spec.md                   # データ設計が単純な場合は削除
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md
├── .gitignore
└── .env.example
```

---

## セットアップ

### 1. テンプレートからリポジトリを作成

GitHub の「Use this template」からリポジトリを作成する。

### 2. CLAUDE.md を記入する

`CLAUDE.md` の `{PLACEHOLDER}` を実際の値に書き換える。Claude Code に任せる場合:

```
/setup-project
```

対話形式で `docs/` の永続ドキュメントを作成しながら、CLAUDE.md の内容も埋めていける。

### 3. .gitignore の言語セクションを有効化する

`.gitignore` の言語セクションはコメントアウト済み。使用言語のコメントを外す。

### 4. .env を作成する

```bash
cp .env.example .env
# .env に実際の値を記入する
```

### 5. hooks を有効化する（使用する場合）

`.claude/hooks/lint.sh` の使用言語のコメントを外す。

---

## ワークフロー

```
プロジェクト開始
      │
      ▼
/setup-project          ← docs/ を対話形式で作成
      │
      ▼
/add-feature [機能名]   ← 要件定義 → 設計 → 実装 → テスト
      │
      ├── 実装中に迷ったら /checkpoint  ← .steering/ の進捗確認
      │
      ▼
/update-docs            ← docs/ と CLAUDE.md を最新化
      │
      ▼
PR 作成・マージ
      │
      ▼
次のフィーチャーへ
```

---

## コマンド一覧

| コマンド | タイミング | 概要 |
|---|---|---|
| `/setup-project` | プロジェクト開始時（1回） | `docs/` の永続ドキュメントを対話形式で作成する |
| `/add-feature [名前]` | フィーチャー追加のたびに | 要件定義 → 設計 → 実装 → テストを 7 ステップで進める |
| `/checkpoint` | セッション再開時・進捗確認時 | `.steering/` を棚卸しして完了・進行中・放置中を一覧表示する |
| `/update-docs` | フィーチャー完了後・設計変更後 | `docs/` と CLAUDE.md のズレを解消する |

---

## ドキュメントの二層構造

| 種別 | 場所 | 役割 | 更新頻度 |
|---|---|---|---|
| 永続ドキュメント | `docs/` | 何を作るか・どう作るか | フィーチャー完了ごと |
| ステアリングドキュメント | `.steering/` | 今日何をするか | セッションごと |

`docs/` はプロジェクト全体の設計を記録し、`.steering/` は実装中の詳細な判断を記録する。両者を分けることで、`docs/` を常に「読めば全体像がわかる」状態に保つ。
