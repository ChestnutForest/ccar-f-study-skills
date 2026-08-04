# ccar-f-study-skills

CCAR-F（Claude Certified Architect – Foundations）の受験学習を支える、Claude 用のカスタムスキル集。

Anthropic の公式ドキュメントや Anthropic Academy の英文を精読し、その日の学びを NotebookLM に投入できる形で蓄積する、という学習サイクルを自動化することを目的としている。

## これは何か

CCAR-F の学習では、英語の一次資料を読む作業と、読んだ内容を記憶に定着させる作業が繰り返し発生する。毎回同じ指示を会話に貼り直すのは非効率なので、その手順を [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) として切り出したものがこのリポジトリである。

収録している主なスキルは2つ。

| スキル | 版 | 役割 |
|---|---|---|
| `english-parsing` | v10.3 | 英文を10ブロック構成で精読する。ホーンビーの動詞型（VP1〜VP25）による構文分析、米式スペルへの統一、CCAR-F の5ドメインへの対応づけまで行う |
| `learning-asset` | v3.1 | その日の学びを日次ファイル `ccaf-YYYY-MM-DD.md` に構造化する。NotebookLM のソースとして投入し、RAG で検索できる粒度で書き出す |

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
│   │       ├── ccar-f-keywords.md          # 技術用語・In/Out-of-Scope・アンチパターン
│   │       ├── question-design.md          # 想定問題の作り方・公式12問の分析
│   │       ├── anthropic-skills-catalog.md # anthropics/skills の公式スキル一覧
│   │       └── changelog-archive.md        # v8.0 以前の変更履歴
│   └── learning-asset/
│       ├── SKILL.md
│       └── references/
│           └── ccar-f-blueprint.md
├── prompts/
│   └── notebooklm-slide-prompt.md      # 学習ログからスライドを生成するプロンプト
└── logs/
    ├── ccaf-2026-07-28.md
    ├── ccaf-2026-07-31.md
    └── ccaf-2026-08-01.md
```

`skills/` にスキル本体、`prompts/` に NotebookLM へ渡すプロンプト、`logs/` に日次の学習ログを置く。ログを同じリポジトリに入れるかは運用次第で、学習内容を公開したくない場合は別リポジトリに分けるか `.gitignore` に加える。

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
| 【10】想定問題 | **試験範囲内のときのみ**。CCAR-F 形式の4択を1問。公式の判断3軸のいずれかを軸に据える |

【7】と【9】を Academy 限定にしているのは、公式ドキュメント自体が一次資料である場合、そこから別のリンクへ誘導する価値が薄いため。Academy は概念説明なので、実装ドキュメントや動くスキルへの橋渡しに意味がある。

## 設計上の判断

このスキル群は、実際の運用で見つかった問題を繰り返し反映して現在の形になった。主なものを記録として残す。

**動詞型を軸に統合した（v4.0 → v6.0）** — 当初は「構文分析」「カード化案」「副詞的語句」を独立ブロックとして並べていたが、いずれも特定の動詞が作る文の中で働くものなので、動詞型の各項目に統合した。分析対象が分散するより、動詞型の下にまとまっているほうが理解しやすい。

**ソース区分の判定は二度作り直した（v3.9 → v10.1）** — 最初は「Claude Code の話だから Claude Code Docs」と主題で判定して誤り、レジスター（語り口）で判定する方式に改めた（v3.9）。しかしそこで挙げた「二人称ナレーション」というサインが今度は粗すぎた。公式ドキュメントの Quickstart や How-to も `This quickstart walks you through ...` のように二人称で書かれるため、Docs を Academy と誤判定する事故が起きた。v10.1 では、**`lesson` `course` といった学習単位への言及があるときだけ Academy と判定し、二人称は判定材料から外した**。迷ったら Docs 側に倒す（誤って Academy とすると余計なブロックが出るが、逆は省略されるだけで実害が小さい）。

**型番号は推測で書かない** — VP 番号は必ず `references/hornby-patterns.md` から選ぶ。もっともらしい誤った番号は学習の土台を壊す。同じ理由で、章節番号（§）が確定できない場合はその行を出力しない。

**参照ファイルへの分割** — SKILL.md が 500 行を超えると保守しづらくなるため、型定義表・試験ブループリント・技術用語集・公式スキル一覧・古い変更履歴は `references/` に分離した。必要なときだけ読む段階的開示の形になっている。

**読解を演習に変換する（v10.0）** — CCAR-F は暗記ではなく判断を問う試験だが、読んで理解するだけでは「選択肢の中から選ぶ」という本番の行為を一度も練習できない。そこで【10】で毎回1問だけ4択を作る。自作問題が公式とずれないよう、必ず公式サンプル問題の判断3軸（確実性・根本原因・比例性）のいずれかを軸に据えることを必須制約にした。誤答も「一見もっともらしいが軸を外している」型を使う。

**試験範囲の判定を公式ガイドに接地させた（v9.0）** — 公式 Exam Guide の Appendix には、出題対象の技術用語（`stop_reason`、`tool_choice`、`Task tool`、lost-in-the-middle など）、明示的な In-Scope / Out-of-Scope の一覧、そして公式が名指しするアンチパターンが載っている。これを `ccar-f-keywords.md` に整理し、読んだ英文が試験にどう効くか（あるいは範囲外か）を推測ではなく一次資料に基づいて判定できるようにした。

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

## 底本と第三者の著作物について

**動詞型の分類体系**は A.S. ホーンビー『第2版 英語の型と語法 Guide to Patterns and Usage in English』（伊藤健三 訳注、オックスフォード大学出版局）に依拠している。VP1〜VP25、NP1〜NP3、AP1〜AP3 という番号体系と、各型が指す文構造は同書のものである。

`references/hornby-patterns.md` および `hornby-tenses.md` に収録しているのは、**型番号と、その型が指す文構造の要約、および型ごとに1〜2文の短い例文**である。例文は `The moon rose.` `This is a book.` のように、英文法書に広く共通して現れる汎用的な短文に限っている。原典の解説文や、まとまった分量の記述は転載していない。

とはいえ、**分類体系そのものは同書に由来する**。このリポジトリのライセンスは筆者が書いた部分（スキルの設計、判断基準、出力フォーマットの定義、日本語による説明）に対するものであり、ホーンビーの分類体系や原典の記述に対して何らかの権利を主張するものではない。体系を体系として学びたい方は原典を参照されたい。

**試験範囲の対応づけ**は Anthropic 公式の『Claude Certified Architect – Foundations Exam Guide』Version 1.0 に基づく。`references/ccar-f-blueprint.md` `ccar-f-keywords.md` `question-design.md` は、同ガイドの記述を要約・再構成したものである。試験問題そのものは含まない（公式サンプル問題の分析は、正解の判断軸を抽出した要約に留めている）。

## ライセンス

[MIT License](LICENSE) — Copyright (c) 2026 Kazuyuki Kuribayashi

上記「底本と第三者の著作物について」のとおり、このライセンスが及ぶのは筆者が書いた部分（スキルの設計、判断基準、出力フォーマットの定義、日本語による説明）に限られる。
