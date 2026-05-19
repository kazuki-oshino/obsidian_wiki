# Examples

## 典型プロンプト

### 1. 回答だけ返して wiki 更新を見送る

入力例:

```text
`llm-knowledge-base-workflow` の Query はどういう意味か、短く教えて。まだ wiki には反映しなくていい。
```

期待される動き:

- `CLAUDE.md`、`wiki/index.md`、`wiki/concepts/llm-knowledge-base-workflow.md` を読む
- 質問に短く答える
- 既存ノートにすでに同種の整理があり、新しい durable artifact は不要だと判断する
- ユーザーが wiki 更新を明示していなくても、言い換えだけなら更新しない
- `wiki/` は更新しない

### 2. 既存 `concept` に追記する

入力例:

```text
この Vault の Query 運用では「まず回答品質を優先し、その後に wiki 還元を判断する」点も重要だから、既存の workflow ノートに反映して。
```

期待される動き:

- 関連する既存ノートを読み、どこへ追記するのが自然かを判断する
- 新規ノートを作らず、`wiki/concepts/llm-knowledge-base-workflow.md` のような既存 `concept` へ追記する
- `updated` を更新する
- 新しい入口が不要なら `wiki/index.md` は更新しない

### 3. 比較結果を `comparison` に昇格する

入力例:

```text
`karpathy-llm-knowledge-bases` と `karpathy-llm-wiki-pattern` の違いを比較して、後から見返せる comparison ノートとして残して。
```

期待される動き:

- 2 つの source-summary と関連ノートを読む
- 比較軸が安定しているかを確認する
- 再利用価値が十分なら、新しい `comparison` ノートを作成するか既存ノートを更新する
- 新しい入口が必要なら `wiki/index.md` を更新する

## 判断の目安

- 見送り:
  - 既存ノートの内容をほぼ言い換えるだけ
  - 今回だけの一時的な質問
- 追記:
  - 既存 `concept` や `overview` に 1 つ新しい観点を足すだけで十分
- 新規作成:
  - 既存ノートでは主題が埋もれ、あとで再訪する価値が高い

## 避けること

- `raw` を渡されていないのに `source-summary` を量産すること
- 比較軸が曖昧なまま `comparison` を作ること
- 入口ページが不要なのに `overview` を増やすこと
