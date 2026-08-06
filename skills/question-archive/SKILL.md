---
name: question-archive
description: 直前の応答で english-parsing の【10. 想定問題】として生成した4択問題を、英文・和訳・解説の全文を含む Markdown ファイルとして書き出し、GitHub リポジトリ ccar-f-study-skills の questions/ フォルダへ commit / push するための手順を用意する。ユーザーが「さきほどの想定問題をcommit/pushして」「想定問題をcommit/pushして」「今の問題をリポジトリに追加して」「想定問題を保存して」と言ったときに使う。ファイル名は question-YYYY-MM-DD-NN.md（同日2問目以降は連番）。生成するのはファイルと PowerShell のコマンドリストであり、チャットに問題文の全文を再掲しない。日次の学習ログ全体を残すのは learning-asset の役割であり、本スキルは想定問題1問だけを切り出して蓄積する。英文を1本読んで解釈するのは english-parsing の役割。判断基準は「直前に生成した想定問題だけをリポジトリへ保存するのか（本スキル）」である。
---

# 想定問題のアーカイブ（question-archive）v1.0

english-parsing の【10. 想定問題】で生成した4択問題を、GitHub リポジトリに蓄積するためのスキル。

## なぜ切り出すのか

想定問題は日次学習ログ（`logs/ccaf-*.md`）の中にも含まれるが、ログは1日分の学習全体を記録するため、**問題だけを探して解き直すことができない**。`questions/` フォルダに1問1ファイルで蓄積すれば、次の使い方ができる。

- **後日まとめて解き直す**。ファイルを開いて選択肢まで読み、正解を見る前に自分で選ぶ。
- **判断軸ごとに検索する**。ファイル内に軸（確実性／根本原因／比例性）を明記してあるため、`grep` や GitHub 検索で絞れる。
- **出題の偏りを確認する**。どのドメインの問題が多いか、どの軸に寄っているかをファイル一覧から把握できる。

## 絶対規則：チャットに問題文を再掲しない

**「想定問題を commit/push して」と言われたら、成果物はファイルとコマンドリストだけである。** 直前の応答で既に問題文は表示されているため、同じ内容をもう一度チャットに書くと会話が長大になる。

- ❌ 問題文・選択肢・解説をチャット本文に書く → しない
- ✅ `create_file` でファイルを作り、`present_files` で提示し、PowerShell コマンドを示す

補足として書いてよいのは、ファイル名、questions/ フォルダの新設有無、蓄積状況（何問目か）、コマンドの注意点のみ。数行に留める。

## 出力手順

1. **`create_file`** で `/mnt/user-data/outputs/question-YYYY-MM-DD-NN.md` を作る。
2. **リポジトリ一式（zip）を再生成**して `questions/` フォルダを含める。
3. **`present_files`** でファイルと zip を提示する。
4. **PowerShell のコマンドリスト**を示す。
5. 短い補足（数行）。

## ファイル名の規則

`question-YYYY-MM-DD-NN.md`

- 日付は **JST**。想定問題を生成した日。
- `NN` は同じ日の連番（`01` から）。1日1問なら `01` で固定。
- 例：`question-2026-08-04-01.md`

## ファイルの構成

直前の応答で生成した【10. 想定問題】の内容を**そのまま全文**収める。要約したり省略したりしない。

```markdown
# 想定問題 YYYY-MM-DD-NN

- **判断軸**: 確実性 / 根本原因 / 比例性 のいずれか
- **対応ドメイン**: Domain N — 名称（配点 N%）／範囲外の場合はその旨
- **出典英文**: どの英文から作ったか（ページ名とソース区分）
- **出典URL**: 確認済みのもののみ。無ければ「要確認」

---

## Scenario

（英語のシナリオ全文）

## Question

（英語の設問全文）

## Options

A.〜D.（英語の選択肢全文）

---

## 解答

<details>
<summary>正解と解説を開く</summary>

**正解**: X

（正解の理由・日本語）

**誤答の理由**

（A〜D それぞれ・日本語）

</details>

---

## 出題文の解説

### 【スラッシュリーディング】
（Scenario・Question・選択肢A〜D すべて）

### 【英語の型と語法】
（動詞型を VP 番号昇順。構文・修飾・カード案を各項目に統合）

### 【語彙・語法メモ】
（試験特有の言い回し、選択肢の語法、米式スペルの確認）

### 【全訳】
（Scenario・Question・選択肢A〜D すべての日本語訳）
```

**`<details>` タグで解答を折りたたむ**。GitHub の Markdown は `<details>` / `<summary>` をサポートしており、これは JavaScript ではなく HTML の標準要素なのでサニタイズされない。解き直すとき、正解を見ずに選択肢まで読める。

## リポジトリの構成

`questions/` フォルダをリポジトリ直下に置く。

```
ccar-f-study-skills/
├── README.md
├── LICENSE
├── skills/
├── prompts/
├── logs/
└── questions/          ← 本スキルが追加する
    ├── README.md       ← 一覧と使い方
    └── question-YYYY-MM-DD-NN.md
```

**`questions/README.md` を必ず用意する。** 蓄積した問題の一覧表（ファイル名・日付・判断軸・ドメイン・出典）を置き、新しい問題を追加するたびに行を足す。フォルダを開いた人が、どの問題があるかを一覧で把握できるようにするため。

## commit / push コマンドの形式

PowerShell を前提とする（学習者の環境が Windows のため）。作業ディレクトリは `C:\Users\kazuy\projects\ccar-f-study-skills`。

```powershell
Expand-Archive -Path "$env:USERPROFILE\Downloads\ccar-f-study-skills.zip" -DestinationPath "$env:TEMP\ccarf" -Force
Copy-Item -Path "$env:TEMP\ccarf\repo\*" -Destination . -Recurse -Force
Remove-Item -Path "$env:TEMP\ccarf" -Recurse -Force
git add .
git commit -m "Add exam question YYYY-MM-DD-NN"
git push
```

コミットメッセージは英語で、何を追加したかが分かる形にする。

## 参照

- 作問の指針・判断3軸・誤答の型は `references/question-design.md`（english-parsing 側）を参照する。本スキルは**既に生成された問題を保存する**役割であり、作問はしない。
- 試験範囲の判定は `references/ccar-f-keywords.md`（english-parsing 側）による。範囲外の英文から作った問題は、その旨をファイル冒頭に明記する。
