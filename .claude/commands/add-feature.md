# /add-feature コマンド

新機能を仕様駆動で実装する 7 ステップのワークフロー。

**使い方**: `/add-feature [機能名]`  
**例**: `/add-feature historical-data-collection`

---

## 重要ルール

- **各ステップを完了したら即座に次のステップへ進む**（ユーザー確認を求めない）
- 確認が必要な場面（設計の判断・破壊的変更など）のみ一時停止する
- 全タスクが `[x]` になるまで実装ループを継続する

---

## ステップ 1: コンテキスト確立

1. `.steering/[YYYYMMDD]-[機能名]/` ディレクトリを作成する
2. 今日の日付は `currentDate` コンテキストから取得する

---

## ステップ 2: プロジェクト理解

以下を順番に読む（スキップ禁止）:

1. `CLAUDE.md` — プロジェクト全体の制約・規約
2. `docs/` 以下の永続ドキュメント — 関連する仕様・設計
3. 実装対象に関連する既存コードを grep で調査する

```bash
# 例: 過去データ収集の場合
grep -r "ohlcv" Application/ --include="*.py" -l
grep -r "s3_handle" Application/ --include="*.py" -l
```

---

## ステップ 3: 計画フェーズ（Steering スキル: planning モード）

`Skill(steering)` を呼び出し、以下の 3 ファイルを生成する:

1. `.steering/[date]-[feature]/requirements.md` — 要件定義（ユーザー確認後に次へ）
2. `.steering/[date]-[feature]/design.md` — 技術設計（ユーザー確認後に次へ）
3. `.steering/[date]-[feature]/tasklist.md` — タスクリスト（ユーザー確認後に実装開始）

**1 ファイル作成 → ユーザー確認 → 次のファイル、の順番を守る**

---

## ステップ 4: 実装ループ（Steering スキル: implementation モード）

`Skill(steering)` を implementation モードで呼び出し、`tasklist.md` の全タスクを実行する。

- タスク完了 → 即 `tasklist.md` を `[x]` に更新 → 次のタスク
- 全タスク `[x]` になるまでループ継続

---

## ステップ 5: テスト実行

```bash
# pytest 実行
cd /path/to/project && python -m pytest Test/ -v

# 型チェック（mypy が設定されている場合）
# mypy Application/
```

テストが失敗した場合は修正して再実行。全通過まで次のステップに進まない。

---

## ステップ 6: 動作確認

実際にコードを動かして動作を確認する:

```bash
# 例: データ収集スクリプトの場合
python Application/DataCollection/collect.py --exchange bitbank --symbol BTC_JPY --timeframe 1h --dry-run
```

---

## ステップ 7: 振り返り（Steering スキル: retrospective モード）

`Skill(steering)` を retrospective モードで呼び出し、`tasklist.md` の振り返りセクションに記録する。