# CCAR-F 試験ブループリント（Claude Certified Architect – Foundations）

出典：Anthropic 公式『Claude Certified Architect – Foundations Exam Guide』Version 1.0 · Effective July 2026

このファイルは、英文解釈の各回で「読んでいる英文が試験のどの範囲に当たるか」を対応づけるための参照表である。**推測で対応づけず、必ずこの表から選ぶこと。**

---

## 1. 試験の基本情報（正確な事実）

| 項目 | 内容 |
|---|---|
| **資格名** | Claude Certified Architect – Foundations |
| **試験コード** | **CCAR-F**（※「CCAF」ではない） |
| 出題数 | 60問 |
| 形式 | 多肢選択・複数選択（各問で選ぶ数が明示される） |
| 構成 | **6つのシナリオ群から4つが出題**（ランダム） |
| 制限時間 | 120分 |
| 実施 | 監督付き（オンライン監督／テストセンター） |
| **合格点** | **スケールスコア 720**（100〜1,000 のスケール） |
| 受験料 | 125 USD |
| 有効期間 | 認定日から12か月 |
| 結果 | 合否＋スケールスコア＋ドメイン別正答率 |

**採点方式**：基準準拠評価（criterion-referenced）。他の受験者との比較ではなく、**固定された performance standard に対して測られる**。ドメイン別の正答率は参考情報であり、合否は総合スケールスコアで決まる。

**試験の性格**：`Candidates must demonstrate not only conceptual knowledge but practical judgment about architecture, configuration, and tradeoffs in production deployments.`（概念的知識だけでなく、本番環境における設計・設定・トレードオフについての**実践的判断力**を示さねばならない）——**暗記ではなく判断を問う試験である。**

---

## 2. 出題ドメインと配点比率（最重要）

| ドメイン | 内容領域 | 配点 |
|---|---|---|
| **Domain 1** | **Agentic Architecture & Orchestration**（エージェント設計とオーケストレーション） | **27%** |
| **Domain 2** | Tool Design & MCP Integration（ツール設計と MCP 統合） | 18% |
| **Domain 3** | Claude Code Configuration & Workflows（Claude Code の設定とワークフロー） | 20% |
| **Domain 4** | Prompt Engineering & Structured Output（プロンプト設計と構造化出力） | 20% |
| **Domain 5** | Context Management & Reliability（コンテキスト管理と信頼性） | 15% |

**注意**：Claude Code の設定（Domain 3）は全体の20%にすぎない。**最大配点は Agent SDK を用いたエージェント設計（Domain 1・27%）である。**

---

## 3. ドメイン別タスクステートメント一覧

### Domain 1: Agentic Architecture & Orchestration（27%）

- **1.1** 自律的タスク実行のためのエージェントループの設計と実装
- **1.2** コーディネーター＝サブエージェント方式のマルチエージェントシステムのオーケストレーション
- **1.3** サブエージェントの呼び出し・コンテキスト受け渡し・spawn の設定
- **1.4** 強制（enforcement）と引き継ぎ（handoff）を伴う多段ワークフローの実装
- **1.5** ツール呼び出しの傍受とデータ処理のための Agent SDK フックの適用
- **1.6** 複雑なワークフローのためのタスク分解戦略の設計
- **1.7** セッション状態・再開（resumption）・フォークの管理

### Domain 2: Tool Design & MCP Integration（18%）

- **2.1** 明確な description と境界条件を備えた効果的なツールインターフェースの設計
- **2.2** MCP ツールの構造化エラーレスポンスの実装
- **2.3** エージェント間でのツールの適切な分配とツール設定
- **2.4** MCP サーバーの Claude Code およびエージェントワークフローへの統合
- **2.5** 組み込みツール（Read, Write, Edit, Bash, Grep, Glob）の選択と適用

### Domain 3: Claude Code Configuration & Workflows（20%）

- **3.1** 階層・スコープを踏まえた CLAUDE.md ファイルの設定
- **3.2** カスタムスラッシュコマンドとスキルの作成・設定
- **3.3** 条件付き規約読み込みのためのパス固有ルールの適用
- **3.4** プランモードと直接実行の使い分けの判断
- **3.5** 段階的な改善のための反復的リファインメント技法の適用
- **3.6** Claude Code の CI/CD パイプラインへの統合

### Domain 4: Prompt Engineering & Structured Output（20%）

- **4.1** 精度向上のための明示的基準を備えたプロンプト設計
- **4.2** 出力の一貫性向上のための few-shot プロンプティングの適用
- **4.3** ツール使用と JSON スキーマによる構造化出力の強制
- **4.4** 抽出のための検証・リトライ・フィードバックループの実装
- **4.5** 効率的なバッチ処理戦略の設計
- **4.6** マルチインスタンス・マルチパスのレビューアーキテクチャの設計

### Domain 5: Context Management & Reliability（15%）

- **5.1** 重要情報を保持するための会話コンテキストの管理
- **5.2** 効果的なエスカレーションと曖昧性解決パターンの設計
- **5.3** マルチエージェントシステムにおけるエラー伝播戦略の実装
- **5.4** 大規模コードベース探索におけるコンテキスト管理
- **5.5** ヒューマンレビューのワークフローと確信度較正の設計
- **5.6** マルチソース統合における情報の出所（provenance）保持と不確実性の扱い

---

## 4. 6つの試験シナリオ

本番では、以下6つのうち**4つがランダムに出題**される。

| # | シナリオ | 主要ドメイン |
|---|---|---|
| **1** | **Customer Support Resolution Agent**（カスタマーサポート解決エージェント）。Agent SDK で構築。返品・請求紛争・アカウント問題など曖昧性の高い要求を扱う。MCP ツール（get_customer, lookup_order, process_refund, escalate_to_human）で backend に接続。目標は初回接触解決率80%以上 | 1, 2, 5 |
| **2** | **Code Generation with Claude Code**（Claude Code によるコード生成）。コード生成・リファクタリング・デバッグ・文書化。カスタムスラッシュコマンド、CLAUDE.md 設定、プランモードと直接実行の使い分け | 3, 5 |
| **3** | **Multi-Agent Research System**（マルチエージェント研究システム）。コーディネーターが専門サブエージェント（Web検索・文書分析・統合・レポート生成）に委譲し、引用付きレポートを生成 | 1, 2, 5 |
| **4** | **Developer Productivity with Claude**（開発者生産性ツール）。未知のコードベース探索、レガシー理解、ボイラープレート生成、反復作業の自動化。組み込みツールと MCP サーバーを利用 | 2, 3, 1 |
| **5** | **Claude Code for Continuous Integration**（CI/CD への統合）。自動コードレビュー、テスト生成、PR フィードバック。実行可能なフィードバックを与え false positive を最小化するプロンプト設計 | 3, 4 |
| **6** | **Structured Data Extraction**（構造化データ抽出）。非構造化文書から情報を抽出し JSON スキーマで検証。エッジケースを適切に処理し下流システムと統合 | 4, 5 |

---

## 5. サンプル問題から読み取れる出題パターン（重要）

公式サンプル問題3問には、**一貫した判断基準**が現れている。

### パターン① 確実性が要る場面では、プロンプトではなくプログラム的強制を選ぶ

> **問**：エージェントが12%のケースで `get_customer` を飛ばして `lookup_order` を呼び、口座誤認と誤返金が起きている。最も効果的な対処は？
> **正解 A**：`get_customer` が検証済み顧客IDを返すまで `lookup_order` と `process_refund` をブロックする**プログラム的な前提条件**を追加する。
> **公式解説**：`When a specific tool sequence is required for critical business logic ..., programmatic enforcement provides deterministic guarantees that prompt-based approaches cannot. Options B and C rely on probabilistic LLM compliance, which is insufficient when errors have financial consequences.`

**＝「指示は希望、フックは保証」そのもの。** システムプロンプトの強化（B）や few-shot 例の追加（C）は、確率的な遵守に頼るため不十分。**金銭的影響がある場面では決定論的保証を選ぶ。**

### パターン② ツール選択の誤りは、まず description の改善で直す

> **問**：類似する2ツールの description が最小限（"Retrieves customer information" / "Retrieves order details"）で、選択を誤る。最も効果的な**第一歩**は？
> **正解 B**：各ツールの description を拡張し、扱う入力形式・クエリ例・エッジケース・**類似ツールとの使い分けの境界**を含める。
> **公式解説**：`Tool descriptions are the primary mechanism LLMs use for tool selection.`

**＝ description が選択を決める、という原理。** few-shot の追加（A）はトークン負荷を増やすだけで根本原因を直さない。ルーティング層（C）は過剰設計。

### パターン③ 過剰設計を避け、原因に比例した最小の対処を選ぶ

> **問**：初回解決率が55%（目標80%）。単純な案件をエスカレーションし、複雑な案件を自律処理しようとしている。最も効果的な改善は？
> **正解 A**：明示的なエスカレーション基準と few-shot 例をシステムプロンプトに追加する。
> **公式解説**：`This is the proportionate first response before adding infrastructure.` LLM の自己申告する確信度は較正が不十分（B）。分類器の訓練は過剰設計（C）。感情分析は別problemを解いている（D）。

**＝「まず原因に比例した対処を。インフラを足すのはその後」。** 5機能を「組み合わせよ、一つに押し込むな」という原則と同じ思想。

### 出題パターンの総括

**選択肢は「どれも一見もっともらしい」。正解を分けるのは次の3軸である。**

1. **確実性が要るか** → 要るならプログラム的強制（フック等）、要らないならプロンプト
2. **根本原因に当たっているか** → 症状ではなく原因を直す選択肢を選ぶ
3. **対処が問題に比例しているか** → 過剰設計（over-engineered）は誤答

---

## 6. 公式が挙げる準備方法（How to Prepare）

- **Agent SDK でエージェントを構築**：ツール呼び出し・エラー処理・セッション管理を含む完全なエージェントループを実装。サブエージェントの spawn とコンテキスト受け渡しを練習
- **実プロジェクトで Claude Code を設定**：CLAUDE.md の階層、`.claude/rules/` のパス固有ルール、frontmatter オプション（`context: fork`、`allowed-tools`）付きのカスタムスキル、MCP サーバー1つ以上の統合
- **MCP ツールの設計とテスト**：類似ツールを明確に区別する description を書く。エラーカテゴリと retryable フラグを持つ構造化エラーレスポンスを実装
- **構造化データ抽出パイプラインの構築**：`tool_use` と JSON スキーマ、検証リトライループ、optional/nullable フィールド、Message Batches API でのバッチ処理
- **プロンプト技法の練習**：曖昧なシナリオ向けの few-shot 例、false positive を減らす明示的レビュー基準、大規模レビュー向けのマルチパス設計
- **コンテキスト管理パターンの学習**：冗長なツール出力からの構造化事実の抽出、長セッション用のスクラッチパッドファイル、コンテキスト制限を管理するサブエージェント委譲
- **エスカレーションと human-in-the-loop の復習**：いつエスカレーションし（ポリシーの隙間・顧客要望・進行不能）、いつ自律解決するか。確信度ベースのルーティング設計

---

## 7. 想定される受験者像

`The ideal candidate for this certification is a solution architect who designs and implements production applications with Claude.`

**Claude API・Agent SDK・Claude Code・MCP を用いた実務経験6か月以上**が目安とされ、本番環境における LLM の能力と限界の両方を理解していることが求められる。
