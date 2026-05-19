# AGENTS.md

この Vault は、`raw/` に集めた一次ソースをもとに、Claude が `wiki/` を継続的に構築・更新していく個人用 LLM ナレッジベースです。狙いは、質問のたびに `raw/` から知識を再発見することではなく、LLM が保守する永続的な wiki を複利的に育てることにあります。

## 基本方針

- この Vault は `raw sources / wiki / schema` の 3 層で運用する。
- `raw/` は不変に近い出典、`wiki/` は Claude が編集する成果物、schema は `CLAUDE.md` と `.claude/skills/` に置く運用規約群とする。
- 人間が主に触るのは `raw/` と schema であり、Claude の主な編集対象は `wiki/` である。
- 人間はソース収集、問いの設定、強調点の判断を担い、Claude は要約、相互参照、横断更新、index 保守、整合性点検を担う。
- ノートは原則として日本語で作成する。固有名詞や論文名は必要に応じて原文を併記する。
- wiki は persistent artifact であり、毎回作り直すものではなく、増分的に育てるものとして扱う。

## ディレクトリの責務

- `raw/`: 一次ソースの取り込み先。Web Clipper、論文、記事、スレッド、メモなどを置く。
- `wiki/`: Claude が生成・更新する知識ノート本体。`index.md` を入口にし、要約、概念整理、比較、全体像の整理を type 別サブディレクトリに蓄積する。
- `attachments/`: 画像や添付ファイルの共通置き場。
- `.claude/`, `.agents/`: Agent 向けの project instructions。schema の一部として、skill や将来の command / rule を置く。

## `raw/` の取り込みルール

- 新しいソースは必ず `raw/` に入れる。`Clippings/` は使わない。
- `raw/` は要約置き場ではなく、出典の保管場所として扱う。
- 既存の frontmatter パターンを維持する。
- 最低限、以下のフィールドを優先して残す。
  - `title`
  - `source`
  - `author`
  - `published`
  - `created`
  - `description`
  - `tags`
- `raw/` の本文は出典に近い形を保ち、要約や再構成は `wiki/` 側で行う。
- `raw/` を編集する場合は、インポート崩れの修正、frontmatter の補完、明らかな整形修正に限定する。

## `wiki/` の作成ルール

- `wiki/` のファイル名は短い kebab-case の slug にする。
- `wiki/index.md` 以外のノートは type ごとのサブディレクトリに置く。
  - `source-summary`: `wiki/source-summaries/<slug>.md`
  - `concept`: `wiki/concepts/<slug>.md`
  - `comparison`: `wiki/comparisons/<slug>.md`
  - `overview`: `wiki/overviews/<slug>.md`
- 各ノートは Obsidian の wikilink を前提に相互リンクする。
- 各ノートの frontmatter には、少なくとも以下を入れる。
  - `title`
  - `type`
  - `sources`
  - `created`
  - `updated`
  - `tags`
- `sources` には、根拠となる `raw/` ノート、または query 還元で直接参照した既存 `wiki/` ノートを Obsidian の wikilink で列挙する。フォルダをまたぐ wiki ノートを `sources` に入れる場合は、`[[wiki/source-summaries/...]]` や `[[wiki/concepts/...]]` のように現在の保存先を明示する。
- `type` は `source-summary` または `concept` を主軸とし、必要なときだけ `overview` と `comparison` を使う。
- `overview` は、特定テーマの concept や source-summary が増え、横断的な入口ページがあった方が探索しやすいときに作る。
- `comparison` は、ユーザーが比較を明示的に求めたとき、または複数ソースに対立点や比較軸があり、後から参照する価値が高いときに作る。
- 既存ノートを更新するときは `updated` を更新し、すでに有用なリンクや観点を消さずに改善する。

## `wiki/` のノート構成

- `source-summary`: 1つのソースを要約し、この Vault にとって重要な論点、前提、留保、適用範囲を抽出する。
- `concept`: 複数ソースを横断して概念、運用フロー、判断基準を整理する。
- `overview`: あるテーマの入口として、主要ノート、全体像、読み進め方を示す。
- `comparison`: 複数の主張、手法、ソース、判断軸を並べて比較する。
- 見出しは厳密に固定しないが、少なくとも「何のノートか」「重要ポイント」「関連ノート」が分かる構成にする。

## 特殊ファイル

- `wiki/index.md` は内容中心の目次であり、wiki 全体の入口として扱う。
- Claude は ingest 時と大きな構造変更時に `wiki/index.md` を更新し、各ページの短い説明と分類を保つ。`index.md` 自体は `overview` 型だが、wiki 全体の入口なので `wiki/overviews/` へ移動しない。
- `wiki/log.md` は将来、ノート数や運用イベントが増えたら導入を検討する。現時点では必須にしない。

## `attachments/` の扱い

- 画像や添付は `attachments/` 配下に保存する。
- あるソース専用の添付が複数ある場合は `attachments/<source-slug>/` にまとめる。
- ノートから添付を参照するときは、相対パスより Obsidian の wikilink や埋め込みを優先する。

## 運用サイクル

### Ingest

1. 新しいソースを `raw/` に取り込む。
2. Claude が対象 `raw/` と関連する既存 `wiki/` ノートを読む。
3. `source-summary` を新規作成または更新する。
4. 必要に応じて既存 `concept`、`overview`、`comparison` を更新し、重複ノートは増やさない。
5. `wiki/index.md` を更新し、新しい入口情報を保つ。

### Query

- Claude はまず `wiki/index.md` と関連ノートをたどって回答を組み立てる。
- まず回答品質を優先し、その後に wiki へ還元する価値があるかを判断する。
- 回答、比較、調査メモ、図解方針などのうち再利用価値が明確に高いものは、まず既存ノートへの追記を優先し、必要なら新しい `concept`、`comparison`、`overview` として wiki に還元する。
- 一度得た整理結果を chat に閉じ込めず、後から再利用できる artifact に変えることを優先する。
- 定型の query 還元フローは `.claude/skills/query-to-wiki/` を参照する。

### Lint

- Claude は定期的に wiki を health check し、矛盾、古い主張、孤立ノート、欠けたリンク、欠けた概念、重複した説明、出典不足を点検する。
- lint は、ユーザーが点検を依頼したとき、複数回の ingest でノートが増えたとき、または大きな構造変更のあとに優先して行う。
- lint の結果は、必要なら既存ノートの局所更新、リンク追加、関連候補の提案として反映する。
- 小規模 Vault では、専用ツールよりも `index.md` と既存ノートの読み直しで十分なことが多い。

## 更新原則

- wiki は毎回 `raw/` から再生成するのではなく、既存ノートに対する delta update を基本とする。
- grow-and-refine を優先し、まず必要な観点を追加し、その後に重複除去や整理を行う。
- 短さだけを最適化しない。brevity bias や context collapse を避け、後から使える具体性を残す。
- ノート全体の全面書き換えより、局所的な追記、修正、重複除去を優先する。
- 新しいソースが過去の主張を弱める、更新する、否定する場合は、その差分を既存ノート側に明示する。
- Q&A や調査で得た新しい観点は、再利用価値があるときだけ wiki に戻し、会話ログだけに閉じ込めない。

## 拡張方針

- 定型ワークフローが固まったら `.claude/commands/` に slash command を追加する。
- ルールが長くなったら `.claude/rules/` に分割する。
- `wiki/` は `source-summaries/`, `concepts/`, `comparisons/`, `overviews/` に分けて運用する。新しい type を増やす必要が出たときだけ追加サブフォルダを検討する。
- ローカル検索 CLI や MCP は、`index.md` だけでは探索しづらくなってから追加を検討する。
