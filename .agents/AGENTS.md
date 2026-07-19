<!-- ⚠️ この共通設定ファイルを編集してください。各AIツール（~/.claude/CLAUDE.md 等）は直接編集せず、本ファイルを参照（import/symlink）するだけにしてください。 -->
<!-- ⚠️ Edit this shared config file. Do not directly edit individual AI tool config files. They should only reference or symlink this file. -->

# AGENTS.md

このファイルは、複数のAIコーディングエージェント（Claude Code, Codex CLI 等）に共通で適用するガイダンスです。

## 判断基準

- 作業は ROI・コスト・労力ではなく、正しいか、現在の状態が誤っているか、目的にかなうかで判断する
- 既知の誤りを「低価値」「エッジケース」「割に合わない」などの理由で放置しない
- 正しい対応を止める唯一の理由は、モデルやツールの制約として実行不可能だと確認できた場合だけ
- 選択肢を提示するときは、正しさと実現可能性で説明する

## バグ修正

- バグは、アーキテクチャがそのバグを許した証拠として扱う
- 修正前に、なぜその構造でバグが起きたのか、同じ構造が同種のバグを生むかを診断する
- 可能なら症状のガードではなく、バグを許した構造を取り除く
- 症状レイヤーの修正に留める場合は、延期する根本原因を明示する

## Code Search

コード検索は、原則として `grep` ではなく `semble search` を使う。

```bash
semble search "authentication flow" ./my-project
semble search "save_pretrained" ./my-project
semble search "save model to disk" ./my-project --top-k 10
```

関連実装の探索には `semble find-related` を使う。

```bash
semble find-related src/auth.py 42 ./my-project
```

`semble` が `$PATH` にない場合は、`uvx --from "semble[mcp]" semble` を使う。

### Workflow

1. `semble search` で関連チャンクを探す
2. 返されたチャンクだけでは足りない場合に限り、ファイル全体を読む
3. 必要に応じて `semble find-related` で類似実装を探す
4. 厳密な文字列一致や網羅確認が必要な場合だけ `grep` / `rg` を使う

## 指示

- 質問には指定がない限り短く答える
- 感想や相槌を省き、結論から簡潔に答える
- 長文エラーは全文を出さず、重要部分・原因・対処だけを簡潔に伝える
- ユーザーが「先に聞いて」と指示した場合、必要情報が足りなければ調査前に聞く
- ライブラリやAPIのドキュメント参照、コード生成、セットアップや設定手順が必要な場合は、常に Context7 MCP を使用する

## プロジェクト概要

markdown-it 用のショートコードプラグイン。`[[id attr=value]]` 形式の記法をパースし、カスタムレンダリングを可能にする。

- npm パッケージ名: `markdown-it-custom-short-codes`
- ライセンス: MIT
- 依存: `markdown-it` ^12.3.2

## コマンド

| コマンド | 説明 |
| --- | --- |
| `npm test` | テスト実行（未構成） |

## アーキテクチャ

スタック: JavaScript (CommonJS), Node.js

- 単一ファイル (`index.js`) で構成されるシンプルなプラグイン
- markdown-it のブロックルール (`md.block.ruler`) に `csc` ルールを追加
- `[[...]]` パターンを正規表現でパースし、タグ名と属性を抽出してトークン化
- レンダリングは利用側でカスタム定義する設計

### ディレクトリ構成

```
.
├── index.js        # プラグイン本体（エントリポイント）
├── package.json
├── README.md
└── LICENSE
```

## 禁止事項

- `develop` `master` ブランチで作業しない
- AIツールの設定ファイル（CLAUDE.md, AGENTS.md等）を暗黙的・自動的にコミットしない
- `.gitignore` を変更しない
- 作業が終わっても自動でコミットしない。ユーザーが頼んだときだけコミットする
- issueを参考に対応するときは、事象が現在も発生しているか、改善案が妥当か確認してから実施する
