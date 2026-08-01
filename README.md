# ccar-f-study-skills

CCAR-F（Claude Certified Architect – Foundations）の受験学習を支える、Claude 用のカスタムスキル集。

Anthropic の公式ドキュメントや Anthropic Academy の英文を精読し、その日の学びを NotebookLM に投入できる形で蓄積する、という学習サイクルを自動化することを目的としている。

## これは何か

CCAR-F の学習では、英語の一次資料を読む作業と、読んだ内容を記憶に定着させる作業が繰り返し発生する。毎回同じ指示を会話に貼り直すのは非効率なので、その手順を [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) として切り出したものがこのリポジトリである。

収録している主なスキルは2つ。

| スキル | 版 | 役割 |
|---|---|---|
| `english-parsing` | v8.3 | 英文を10ブロック構成で精読する。ホーンビーの動詞型（VP1〜VP25）による構文分析、米式スペルへの統一、CCAR-F の5ドメインへの対応づけまで行う |
| `learning-asset` | v3.0 | その日の学びを日次ファイル `ccaf-YYYY-MM-DD.md` に構造化する。NotebookLM のソースとして投入し、RAG で検索できる粒度で書き出す |

## ディレクトリ構成

```
.
├── README.md
├── skills/
│   ├── english-parsing/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── hornby-patterns.md          # VP1〜VP25・NP・AP の全型定義
│   │       ├── hornby-tenses.md            # 12時制の定義と相の意味
│   │       ├── hornby-noun-modifiers.md    # §3.81〜§3.84（v3.5 以降は未使用・保持のみ）
│   │       ├── ccar-f-blueprint.md         # 5ドメイン・配点・タスクステートメント
│   │       ├── anthropic-skills-catalog.md # anthropics/skills の公式スキル一覧
│   │       └── changelog-archive.md        # v8.0 以前の変更履歴
│   └── learning-asset/
│       ├── SKILL.md
│       └── references/
│           └── ccar-f-blueprint.md
└── logs/
    ├── ccaf-2026-07-28.md
    └── ccaf-2026-07-31.md
```

`skills/` にスキル本体、`logs/` に日次の学習ログを置く。ログを同じリポジトリに入れるかは運用次第で、学習内容を公開したくない場合は別リポジトリに分けるか `.gitignore` に加える。

## english-parsing の出力構成

英文を渡すと、次のブロック構成で解釈を出力する。

| ブロック | 内容 |
|---|---|
| 【0】ヘッダー | 解釈日時 |
| 【1】ソース区分と概要 | Claude Code Docs / Anthropic Academy 等の判別と、その英文の位置づけ |
| （無番号）原文の提示 | 入力された英文の全文引用 |
| （無番号・条件付き）フォーマット・サンプル | Markdown / JSON / YAML / CSV / XML / JSONL / TOML / HTML やコマンドラインが含まれるときのみ |
| 【2】スラッシュリーディング | チャンク分割と直訳 |
| 【3】英語の型と語法 | 動詞型（VP番号昇順）を軸に、構文分析・修飾関係・カード化案を統合 |
| 【4】語彙・語法メモ | 英式→米式スペルの変換を含む |
| 【5】全訳 | |
| 【6】技術的背景 | Anthropic エコシステムにおける位置づけ |
| 【7】🔗 ソース情報 | **Anthropic Academy のときのみ**。公式ドキュメントへのディープリンク |
| 【8】CCAR-F 試験範囲との対応 | 5ドメイン・タスクステートメント・6シナリオへの対応づけ |
| 【9】関連する公式スキルの推奨 | **Anthropic Academy のときのみ**。`anthropics/skills` から主題に近いスキルを提示 |

【7】と【9】を Academy 限定にしているのは、公式ドキュメント自体が一次資料である場合、そこから別のリンクへ誘導する価値が薄いため。Academy は概念説明なので、実装ドキュメントや動くスキルへの橋渡しに意味がある。

## 設計上の判断

このスキル群は、実際の運用で見つかった問題を繰り返し反映して現在の形になった。主なものを記録として残す。

**動詞型を軸に統合した（v4.0 → v6.0）** — 当初は「構文分析」「カード化案」「副詞的語句」を独立ブロックとして並べていたが、いずれも特定の動詞が作る文の中で働くものなので、動詞型の各項目に統合した。分析対象が分散するより、動詞型の下にまとまっているほうが理解しやすい。

**ソース区分は主題ではなくレジスターで判定する（v3.9）** — 「Claude Code の話だから Claude Code Docs」と判定して誤ったことがある。同じ話題が公式ドキュメントにも Academy 教材にも登場するため、`you'll learn` `In the next lesson` のような学習ナレーションの有無で判定する。

**型番号は推測で書かない** — VP 番号は必ず `references/hornby-patterns.md` から選ぶ。もっともらしい誤った番号は学習の土台を壊す。同じ理由で、章節番号（§）が確定できない場合はその行を出力しない。

**参照ファイルへの分割** — SKILL.md が 500 行を超えると保守しづらくなるため、型定義表・試験ブループリント・公式スキル一覧・古い変更履歴は `references/` に分離した。必要なときだけ読む段階的開示の形になっている。

## 使い方

### Claude Code の場合

個人スキルとして使うなら、ホームディレクトリ配下に配置する。

```bash
mkdir -p ~/.claude/skills
cp -r skills/english-parsing ~/.claude/skills/
cp -r skills/learning-asset ~/.claude/skills/
```

プロジェクト単位で共有するなら `.claude/skills/` に置いてコミットする。ディレクトリ名がそのままコマンド名になるので、`/english-parsing` のように呼び出せる。

### 呼び出し方

英文を貼れば `english-parsing` が起動する。明示的に指定する必要はない。

「今日の分を資産化して」と伝えれば `learning-asset` が起動し、`ccaf-YYYY-MM-DD.md` を書き出す。

## 参考

- [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — スキルの概念と仕様
- [Create plugins](https://code.claude.com/docs/en/plugins) — プラグインとして配布する場合
- [anthropics/skills](https://github.com/anthropics/skills) — 公式スキル集

## 底本

動詞型の分類は A.S. ホーンビー『第2版 英語の型と語法』（伊藤健三 訳注、オックスフォード大学出版局）に依拠している。試験範囲の対応づけは Anthropic 公式の『Claude Certified Architect – Foundations Exam Guide』Version 1.0 に基づく。

## ライセンス

MIT License とするか、私的利用に留めるかは公開範囲に応じて決める。ホーンビーの型番号体系そのものは書籍の分類に依拠しているため、原典の記述を大量に転載する形での公開は避ける。
