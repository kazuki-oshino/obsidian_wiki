# Examples

## 典型プロンプト

### 1. 1件の raw を wiki 化する

入力例:

```text
raw/Thread by @karpathy.md を wiki にコンパイルして
```

期待される動き:

- `CLAUDE.md` を読む
- `raw/Thread by @karpathy.md` を読む
- `wiki/index.md` があれば読む
- 重複防止のため既存 `wiki` を確認する
- `wiki/source-summaries/karpathy-llm-knowledge-bases.md` のような `source-summary` を作成または更新する
- 関連する `wiki/concepts/llm-knowledge-base-workflow.md` のような `concept` があれば更新する
- `source-summary` 完了後に Concept 判定を行い、既存 concept 更新・新規 concept 作成・候補保留のどれかを明示する
- 必要なら `overview` / `comparison` の既存ノートを更新する
- `wiki/index.md` を更新する

### 2. 複数 raw をまとめて反映する

入力例:

```text
次の raw をまとめて wiki に反映して:
- raw/Thread by @karpathy.md
- raw/Fundamentals of Building Autonomous LLM Agents This paper is based on a seminar technical report from the course Trends in Autonomous Agents Advances in Architecture and Practice offered at TUM..md
```

期待される動き:

- 各ソースの `source-summary` を個別に作成または更新する
- 該当する既存 `concept` があれば更新し、なければ新規作成または保留を判定する
- 共通テーマが未整理で、2 ソース以上をまたぐ安定した概念がある場合は新規 `concept` を作成する
- 必要なら比較軸を `comparison` として残す
- `wiki/index.md` を更新する

### 3. source-summary だけに限定する

入力例:

```text
raw/Thread by @karpathy.md から source-summary だけ作って。concept はまだ更新しないで
```

期待される動き:

- 対応する `source-summary` のみを作成または更新する
- `wiki/index.md` を更新する
- Concept 判定は行うが、ファイル編集はせず候補提案だけに留める

### 4. 溜まった source-summary から concept を棚卸しする

入力例:

```text
最近増えた source-summary から concept 化できるものを整理して
```

期待される動き:

- `raw-to-wiki` ではなく `concept-harvest` を使う
- `wiki/index.md` と既存 `wiki/concepts/` / `wiki/source-summaries/` を読む
- 実質 concept 化している source-summary、既存 concept に吸収されすぎているテーマ、未整理の横断テーマを見つける
- 必要なら新規 `concept` 作成、既存 `concept` 更新、source-summary から concept への昇格を行う
- 新規入口が増えた場合は `wiki/index.md` を更新する

## この Vault での実例

`raw/Thread by @karpathy.md` からは、少なくとも次のような論点を抽出できる。

- `raw/` を一次ソース保管に使い、`wiki/` は LLM が継続編集する
- `source-summary` と `concept` を分けて育てる
- 必要時のみ `overview` や `comparison` を追加する
- Q&A や可視化の出力も再び wiki に戻して資産化する
- health check によって知識ベース全体を継続的に整える

`source-summary` の見出しは固定しない。通常は `概要 / 主要ポイント / 関連ノート` を基本にし、前提条件や適用範囲、留保はまず `主要ポイント` に含める。

特に `raw/Thread by @karpathy.md` のように Vault 運用そのものが主題のソースでは、Vault に効く論点を `主要ポイント` にそのまま含めてよい。

書き方のイメージ:

- 分けない方が自然な例:
  - 「詳細な playbook を保てる」
  - 「ただし短い指示だけで十分なタスクにはそのまま一般化しにくい」
- 独立見出しを検討してよい例:
  - 適用範囲や前提だけで長い箇条書きになり、`主要ポイント` の流れを壊す場合

この場合の出力候補:

- `wiki/source-summaries/karpathy-llm-knowledge-bases.md`
- `wiki/concepts/llm-knowledge-base-workflow.md`

## 避けること

- `raw` の本文を大幅に要約・再構成してしまうこと
- `sources` の wikilink を省略すること
- 既存 `concept` があるのに、似た内容の新規ノートを量産すること
- 英語ソースをそのまま英語中心で wiki 化して、日本語ノート方針を崩すこと
