# 想定問題の作り方（【10】用）

出典：Anthropic 公式『Claude Certified Architect – Foundations Exam Guide』Version 1.0 §9 Sample Questions（12問）の分析

用途：english-parsing の【10. 想定問題】で、公式に近い難易度・着眼点の4択問題を作るための指針。

---

## 判断の3軸（必ずいずれかを軸に据える）

| 軸 | 問われ方 | 正解の方向 |
|---|---|---|
| **確実性** | 失敗が許されない場面での実装選択 | プログラム的強制（フック・前提条件ゲート）。プロンプトは確率的遵守なので不可 |
| **根本原因** | 症状が出ている系の、どこを直すか | 上流の設計。下流が正常に動いているなら下流を疑わない |
| **比例性** | 「最初の一手」として何をするか | 低コスト・高レバレッジな施策。過剰設計は誤答 |

---

**誤答の作り方（公式サンプル問題の分析から）**：公式の誤答は「明らかに変な選択肢」ではなく、**一見もっともらしいが軸を外している**もので構成される。次の型を使う。

- **プロンプトで済ませようとする**（確実性が要る場面で）
- **過剰設計**（分類器の訓練、ML 基盤、独自ルーティング層など、簡単な手を試す前に持ち出す）
- **症状に対処**（正常に動いている下流を疑う、根本原因を外す）
- **存在しない機能**（実在しないフラグ・環境変数。公式問題にも `--batch` や `CLAUDE_HEADLESS` として登場する）
- **別の問題を解いている**（問われている論点とずれた、それ自体は妥当な施策）

**出力形式**：

```
## 【10. 想定問題】

**判断軸**: 比例性（対処が問題の規模に見合うか）

**シナリオ**: <1〜3行で本番環境の状況を設定。公式は必ず具体的な数値や症状を含める>

**設問**: <何を選ばせるか。「最も効果的な最初の一手はどれか」「最も可能性の高い根本原因はどれか」など>

A. <選択肢>
B. <選択肢>
C. <選択肢>
D. <選択肢>

**正解**: X。<なぜ正しいかを、軸に沿って説明>
**誤答の理由**: A は…／B は…／D は…（それぞれ、なぜもっともらしく見えて、なぜ外れるか）
```


---

## 公式サンプル問題に見る出題パターン

| # | シナリオ | 軸 | 正解の型 |
|---|---|---|---|
| 1 | 顧客確認を飛ばして返金する | 確実性 | 前提条件ゲートで機械的にブロック |
| 2 | 似たツールを取り違える | 比例性 | まずツール説明文を充実させる（few-shot やルーティング層は過剰） |
| 3 | エスカレーション判断がずれる | 比例性 | 明示的な基準＋few-shot（分類器の訓練は過剰） |
| 4 | コマンドをチーム共有したい | 事実確認 | `.claude/commands/`（project スコープ） |
| 5 | モノリスをマイクロサービス化 | 根本原因 | プランモード（複雑度は既に判明している） |
| 6 | テストファイルが各所に散在 | 根本原因 | `.claude/rules/` の glob パターン |
| 7 | 研究レポートの対象領域が偏る | 根本原因 | コーディネーターの分解が狭い（下流は正常） |
| 8 | サブエージェントがタイムアウト | 確実性 | 構造化エラーで文脈を渡す |
| 9 | 検証のたびに往復が発生 | 比例性 | 85%の単純ケースにスコープ付きツール |
| 10 | CI でジョブがハングする | 事実確認 | `-p` フラグ |
| 11 | バッチ API に全部移すか | 比例性 | ブロッキング処理は同期、夜間はバッチ |
| 12 | 14ファイルのレビューが不安定 | 根本原因 | パス分割（大きなコンテキスト窓では解決しない） |

**観察**：12問中、事実確認は2問だけで、残り10問は判断を問う。**用語を知っているかではなく、状況に対して何を選ぶかが中心**である。

---

## 英語で出題するための定型表現（v10.2）

公式サンプル問題12問から抽出した言い回し。**シナリオ・設問・選択肢はこれらを使って英語で書く。**

### シナリオの書き出し

| 表現 | 用法 |
|---|---|
| `Production data shows that ...` | 本番データで判明した症状を示す |
| `Production logs show the agent frequently ...` | ログから読み取れる頻出の挙動 |
| `Your agent achieves 55% first-contact resolution, well below the 80% target.` | 数値と目標のギャップ |
| `During testing, you observe that ...` | テスト中の観察 |
| `After running the system on ..., you observe that ...` | 実行後の結果 |
| `You are building / You are using / You are integrating ...` | 状況設定 |
| `Your pipeline script runs ... but the job hangs indefinitely.` | 具体的な障害 |

**必ず具体的な数値を入れる**：`in 12% of cases` / `55% first-contact resolution` / `14 files` / `2-3 round trips` / `increases latency by 40%` / `85% of these verifications`。

### 設問の型

| 表現 | 問うている軸 |
|---|---|
| `What change would most effectively address this reliability issue?` | 確実性 |
| `What's the most effective first step to improve ...?` | 比例性（`first step` が鍵） |
| `What is the most likely root cause?` | 根本原因 |
| `What's the most effective approach to reduce overhead while maintaining reliability?` | 比例性（トレードオフ） |
| `Which approach should you take?` | 判断全般 |
| `Where should you create this command file?` | 事実確認 |
| `How should you evaluate this proposal?` | 比例性 |
| `How should you restructure the review?` | 根本原因 |

**`most effectively` / `most likely` / `first step` といった限定語が判断の軸を決める**。特に `first step` は「まず何をするか」を問うので、過剰設計を排除する比例性の問題になる。

### 選択肢の書き方

動詞の原形または動名詞で始め、4つの長さを揃える。

- `Add a programmatic prerequisite that blocks ... until ...`
- `Enhance the system prompt to state that ...`
- `Implement a routing classifier that analyzes ...`
- `Switch both workflows to batch processing with status polling ...`

### 出題文の解説（v10.2）

正解と誤答の理由を書いた後、**シナリオと設問の英文**に対して【スラッシュリーディング】【英語の型と語法】【語彙・語法メモ】を追記する。出題文は試験特有の言い回しの塊なので、それ自体が読解教材になる。選択肢A〜Dは分量が多いため解説対象に含めない（重要な語法があれば語彙メモで触れる）。

---

## 作問時のチェックリスト

- [ ] 軸（確実性／根本原因／比例性）を1つ決めたか
- [ ] シナリオに具体的な数値・症状を入れたか（公式は必ず入れる。例：「12%のケースで」「55%の解決率」「14ファイル」）
- [ ] 誤答3つが、いずれも一見もっともらしいか
- [ ] 正解の根拠が、読んだ英文か参照ファイルにあるか（推測で仕様を作っていないか）
- [ ] 各誤答について「なぜ外れるか」を説明できるか
