# CCAR-F 技術用語・キーワード集

出典：Anthropic 公式『Claude Certified Architect – Foundations Exam Guide』Version 1.0 · §17 Appendix

用途：english-parsing の【8. CCAR-F 試験範囲との対応】で、読んでいる英文に**試験に出る技術用語が含まれているか**を照合するための一覧。用語が一致したら、その用語を明示して対応づける。**推測で用語を作らず、必ずこの表から選ぶこと。**

---

## 1. 技術・概念の一覧（§17 Technologies and Concepts）

公式ガイドが「試験に出る可能性がある」と明示した技術と概念。英文中にこれらが現れたら、試験範囲との接点である。

### Claude Agent SDK（Domain 1 の中核）

`agent definitions` / `agentic loops` / `stop_reason handling` / `hooks (PostToolUse, tool call interception)` / `subagent spawning via Task tool` / `allowedTools configuration`

- **stop_reason** の値は `"tool_use"`（ツール実行が必要＝ループ継続）と `"end_turn"`（完了＝ループ終了）。
- **Task tool** がサブエージェントを spawn する機構。コーディネーターの `allowedTools` に `"Task"` を含める必要がある。

### Model Context Protocol（MCP）（Domain 2 の中核）

`MCP servers` / `MCP tools` / `MCP resources` / `isError flag` / `tool descriptions` / `tool distribution` / `.mcp.json configuration` / `environment variable expansion`

- **MCP resources** は「コンテンツのカタログを見せる」機構（探索的なツール呼び出しを減らす）。**MCP tools** は「行動させる」機構。この対比が出題される。
- **環境変数展開**：`${GITHUB_TOKEN}` のような形で `.mcp.json` に書き、秘密情報をコミットせずに済ませる。

### Claude Code（Domain 3 の中核）

`CLAUDE.md configuration hierarchy (user/project/directory)` / `.claude/rules/ with YAML frontmatter path-scoping` / `.claude/commands/ for slash commands` / `.claude/skills/ with SKILL.md frontmatter (context: fork, allowed-tools, argument-hint)` / `plan mode` / `direct execution` / `/memory command` / `/compact` / `--resume` / `fork_session` / `Explore subagent`

- **SKILL.md の frontmatter オプション**は試験範囲：`context: fork`（隔離実行）、`allowed-tools`（ツール制限）、`argument-hint`（引数の促し）。
- **Explore subagent** は冗長な探索出力を隔離して要約を返すサブエージェント。

### Claude Code CLI（Domain 3 / CI 文脈）

`-p` / `--print`（非対話モード）／`--output-format json` / `--json-schema`（構造化出力の強制）

### Claude API（Domain 4 の中核）

`tool_use with JSON schemas` / `tool_choice options ("auto", "any", forced tool selection)` / `stop_reason values` / `max_tokens` / `system prompts`

- **tool_choice の3種**：`"auto"`（テキストを返してもよい）／`"any"`（必ずツールを呼ぶが選択は自由）／`{"type": "tool", "name": "..."}`（特定ツールを強制）。

### Message Batches API（Domain 4）

`50% cost savings` / `up to 24-hour processing window` / `custom_id for request/response correlation` / `polling for completion` / **`no multi-turn tool calling support`**

- レイテンシ SLA の保証はない。**ブロッキングな処理（マージ前チェック等）には不適**、夜間バッチには最適。

### JSON Schema / Pydantic（Domain 4）

`required vs optional fields` / `enum types` / `nullable fields` / `"other" + detail string patterns` / `strict mode` / `schema validation` / `semantic validation errors` / `validation-retry loops`

- **tool use による strict schema は構文エラーを消すが、意味的な誤り（合計が合わない等）は防げない。**

### 組み込みツール（Domain 2）

`Read` / `Write` / `Edit` / `Bash` / `Grep` / `Glob`

- **Grep**＝ファイル内容の検索、**Glob**＝ファイルパスのパターン一致。この使い分けが出題される。
- **Edit** が一意なテキストを見つけられないときは **Read + Write** で代替する。

### プロンプト技法（Domain 4）

`few-shot prompting`（曖昧な場面への的を絞った例示、フォーマットの提示、新規パターンへの汎化）／`prompt chaining`（逐次的なタスク分解）

### コンテキスト管理（Domain 5）

`token budgets` / `progressive summarization` / **`lost-in-the-middle effects`** / `context extraction` / `scratchpad files`

- **lost in the middle**：長い入力の**先頭と末尾は確実に処理されるが、中間部分は欠落しうる**という現象。要約は先頭に置く。

### セッション管理（Domain 1 / 5）

`session resumption` / `fork_session` / `named sessions` / `session context isolation`

### 信頼度スコアリング（Domain 5）

`field-level confidence` / `calibration with labeled validation sets` / `stratified sampling for error rate measurement`

---

## 2. 明示的に試験範囲内の論点（§17 In-Scope Topics）

英文がこれらを扱っていれば、**確実に試験範囲**である。

- エージェントループの実装（stop_reason による制御フロー、ツール結果の扱い、終了条件）
- マルチエージェントのオーケストレーション（コーディネーター＝サブエージェント、タスク分解、並列実行、反復的改善ループ）
- サブエージェントのコンテキスト管理（明示的な受け渡し、状態の永続化、manifest によるクラッシュ復旧）
- ツールインターフェース設計（説明文の書き方、分割と統合、命名による曖昧さの低減）
- MCP のツール／リソース設計（カタログはリソース、行動はツール、採用を左右する説明の質）
- MCP サーバー設定（project と user のスコープ、環境変数展開、複数サーバーの同時利用）
- エラー処理と伝播（構造化エラー、transient / business / permission の区別、局所復旧の優先）
- エスカレーション判断（明示的な基準、顧客の希望の尊重、ポリシーの空白の特定）
- CLAUDE.md 設定（階層、`@import`、`.claude/rules/` の glob パターン）
- カスタムコマンドとスキル（project と user のスコープ、`context: fork`、`allowed-tools`、`argument-hint`）
- プランモード vs 直接実行（複雑度の評価、アーキテクチャ上の判断、単一ファイルの変更）
- 反復的な改善（入出力の例示、テスト駆動の反復、インタビューパターン、逐次 vs 一括の問題解決）
- tool_use による構造化出力（スキーマ設計、tool_choice の設定、nullable による幻覚防止）
- few-shot プロンプティング（曖昧な場面の的確な例示、フォーマットの一貫性、誤検知の削減）
- バッチ処理（Message Batches API の適否、レイテンシ許容度の評価、custom_id による失敗処理）
- コンテキスト窓の最適化（冗長なツール出力の刈り込み、構造化された事実の抽出、位置を意識した入力順）
- 人間レビューのワークフロー（信頼度較正、層化抽出、文書種別・フィールド別の精度分析）
- 情報の出所の保持（claim-source マッピング、時間データの扱い、矛盾の注記、カバレッジの欠落報告）

---

## 3. 明示的に試験範囲外の論点（§17 Out-of-Scope Topics）

**英文がこれらを扱っている場合、【8】では「試験範囲外」と正直に書く。** 無理に対応づけない。

- モデルのファインチューニング／カスタムモデルの訓練
- Claude API の認証・課金・アカウント管理
- 特定のプログラミング言語やフレームワークの詳細な実装
- MCP サーバーのデプロイ・ホスティング（インフラ、ネットワーク、コンテナ）
- Claude の内部アーキテクチャ、訓練過程、モデルの重み
- Constitutional AI、RLHF、安全性訓練の方法論
- 埋め込みモデル、ベクトルデータベースの実装詳細
- Computer use（ブラウザ自動化、デスクトップ操作）
- 画像認識・視覚機能
- ストリーミング API、Server-Sent Events
- レート制限、クォータ、API 料金計算
- OAuth、API キーのローテーション、認証プロトコルの詳細
- 特定クラウド（AWS / GCP / Azure）の設定
- 性能ベンチマーク、モデル比較指標
- プロンプトキャッシュの実装詳細（存在を知る以上のこと）
- トークン数え上げのアルゴリズム、トークナイズの詳細

**注意**：ZDR（データ保持）やエンタープライズ管理設定は、この Out-of-Scope 一覧に「API 認証・課金・アカウント管理」として部分的に触れられているが、`managed settings` による**ツール・コマンドの制御**は Domain 3 の設定階層に関わる。データ保持ポリシーそのものは範囲外、設定による強制は範囲内、と切り分ける。

---

## 4. 公式が名指しするアンチパターン（出題されやすい「誤答」）

公式ガイドが `Avoiding anti-patterns such as ...` `Why ... are both anti-patterns` と明示したもの。**選択肢として現れたら誤答である可能性が高い。**

| アンチパターン | 正しい対処 | 出典 |
|---|---|---|
| 自然言語のシグナルを解析してループ終了を判定する | `stop_reason` で判定する | 1.1 |
| 恣意的な反復回数の上限を主たる停止機構にする | `stop_reason` で判定する | 1.1 |
| アシスタントのテキスト有無を完了の指標にする | `stop_reason` で判定する | 1.1 |
| プロンプトだけで確実な遵守を期待する（金銭が絡む場面で） | フック・前提条件ゲートで機械的に強制 | 1.4 / 1.5 |
| 一律の汎用エラー（"Operation failed"）を返す | 構造化エラー（category / isRetryable / 説明） | 2.2 |
| エラーを黙って握りつぶす（空の結果を成功として返す） | 構造化エラーで伝える | 5.3 |
| 単一の失敗でワークフロー全体を終了する | 局所復旧し、部分結果とともに伝播 | 5.3 |
| エージェントに多すぎるツールを与える（18個など） | 役割に必要な 4〜5 個に絞る | 2.3 |
| モデルの自己申告 confidence でエスカレーションを判断 | 明示的な基準＋few-shot | 5.2 |
| 感情分析（sentiment）でエスカレーションを判断 | 感情は複雑さと相関しない | 5.2 |
| 「保守的に」「高確信のものだけ報告して」と指示する | 具体的・カテゴリ的な基準を書く | 4.1 |
| 大きなコンテキスト窓のモデルに変えて注意力の問題を解決しようとする | パスを分割する（per-file ＋ cross-file） | 4.6 |
| 複数回実行して2回以上出た指摘だけ採用する | 実際のバグの検出を抑制してしまう | 4.6 |
| 同じセッションで生成したコードを自己レビューさせる | 独立したインスタンスでレビューする | 4.6 |

---

## 5. 判断の3軸（サンプル問題12問から抽出）

公式サンプル問題の正解・不正解の理由づけは、次の3つの軸に集約される。**【8】で「出題される場合の問われ方」を書くときは、この軸を使う。**

1. **確実性が要るなら機構で強制する** — 金銭・本人確認など失敗が許されない場面では、プロンプト（確率的遵守）ではなくフック・前提条件ゲート（決定論的保証）を選ぶ。
2. **根本原因に当たる** — 症状ではなく原因を直す。下流のエージェントが正常に動いているなら、原因は上流の分解や設計にある。
3. **問題の規模に比例した対処を選ぶ** — 過剰設計は誤答。「まず何をすべきか」を問われたら、低コストで効果の高い施策（説明文の改善など）を選ぶ。分類器の訓練や大規模な基盤構築は、簡単な手を試す前には選ばない。

---

## 6. 6つの試験シナリオ（再掲・対応づけ用）

| # | シナリオ | 主要ドメイン |
|---|---|---|
| 1 | Customer Support Resolution Agent（顧客サポートの解決エージェント） | 1, 2, 5 |
| 2 | Code Generation with Claude Code（Claude Code によるコード生成） | 3, 5 |
| 3 | Multi-Agent Research System（マルチエージェント研究システム） | 1, 2, 5 |
| 4 | Developer Productivity with Claude（開発者生産性ツール） | 2, 3, 1 |
| 5 | Claude Code for Continuous Integration（CI への統合） | 3, 4 |
| 6 | Structured Data Extraction（構造化データ抽出） | 4, 5 |
