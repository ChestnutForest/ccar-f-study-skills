# anthropics/skills 公式スキル カタログ

出典：https://github.com/anthropics/skills （2026-08 時点。17ディレクトリ）
用途：english-parsing の【9. 関連する公式スキルの推奨】で、読んだ英文の主題に近いスキルを選ぶための一覧。

⚠️ **【9】は、ソース区分が Anthropic Academy のときだけ出力する（v8.0）。** Claude Code Docs・Claude Platform Docs 等では見出しごと省略するため、このカタログも参照しない。理由は、Docs 系はそれ自体が実装リファレンスであり別実装への橋渡し価値が薄いのに対し、Academy は概念・教材なので「概念→動く実装」への橋渡しが学習価値を持つため（【7】🔗ソース情報と同じ非対称性）。

⚠️ **URL はリンクとして機能する形で出力する。** 生 URL をそのまま書くか `[スキル名](URL)` のリンク記法を使い、バッククォートのコード表記にしない（クリックできなくなるため）。

**候補一覧（`anthropics/skills`。2026-08 時点の17ディレクトリ）**：

| スキル | 概要 | 主に効く Domain | URL |
|---|---|---|---|
| mcp-builder | MCP サーバーの設計・実装・評価。ツール一覧確認→読取専用の探索→複雑な質問10件作成→自答で検証という評価手順まで含む | **Domain 2**（18%） | https://github.com/anthropics/skills/tree/main/skills/mcp-builder |
| skill-creator | スキル作成の公式手引き。良い description の書き方、references への分割判断、本文の詳細度 | **Domain 3**（20%） | https://github.com/anthropics/skills/tree/main/skills/skill-creator |
| claude-api | Claude API の使い方。tool_use・構造化出力・メッセージ設計 | **Domain 4**（20%）／Domain 1 | https://github.com/anthropics/skills/tree/main/skills/claude-api |
| webapp-testing | Playwright でローカル Web アプリを検証。生成→実行→確認の検証ループの実例 | **Domain 1**（27%）／Domain 5 | https://github.com/anthropics/skills/tree/main/skills/webapp-testing |
| doc-coauthoring | 文書の共同執筆。gather → refine → reader test の三段階ワークフロー | Domain 4／Domain 5 | https://github.com/anthropics/skills/tree/main/skills/doc-coauthoring |
| frontend-design | フロントエンドの意匠設計とスタイリングの指針 | Domain 4 | https://github.com/anthropics/skills/tree/main/skills/frontend-design |
| web-artifacts-builder | React・Tailwind・shadcn/ui による複雑な HTML アーティファクト構築 | Domain 4 | https://github.com/anthropics/skills/tree/main/skills/web-artifacts-builder |
| canvas-design／algorithmic-art／theme-factory／slack-gif-creator／brand-guidelines／internal-comms | 意匠・作図・テーマ・社内コミュニケーション系 | 試験範囲との距離は遠い | https://github.com/anthropics/skills/tree/main/skills |
| docx／pdf／pptx／xlsx | 製品のドキュメント生成を動かす本番コード。複雑なスキルの参考実装 | Domain 3（参考） | https://github.com/anthropics/skills/tree/main/skills |
| （スキル外）spec | Agent Skills の公式仕様書。英文教材としても手頃 | Domain 3 | https://github.com/anthropics/skills/tree/main/spec |
| （スキル外）template | 最小構成のスキル雛形 | Domain 3 | https://github.com/anthropics/skills/tree/main/template |


## 補足

- **URL を推測で組み立てない。** 上表にないパスに言及する必要がある場合は、検索で実在を確認してから書くか、リポジトリのルート（https://github.com/anthropics/skills）に留める。
- **ライセンス**：`docx` / `pdf` / `pptx` / `xlsx` は source-available であり Apache 2.0 ではない。改変・再配布を伴う使い方を勧める文脈では、その旨を一言添える。他の多くのスキルは Apache 2.0。
- **リポジトリ構成**：`skills/`（スキル実例）、`spec/`（Agent Skills 仕様書）、`template/`（雛形）、`.claude-plugin/`（マーケットプレイス設定）。
- **導入方法（Claude Code）**：`/plugin marketplace add anthropics/skills` の後、`/plugin install example-skills@anthropic-agent-skills` または `/plugin install document-skills@anthropic-agent-skills`。
