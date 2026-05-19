---
name: concept-harvest
description: Use when this Vault's `wiki/` has accumulated source-summary notes and the user wants to find, create, update, promote, or consolidate cross-source `concept` notes.
---

# Concept Harvest

このスキルは、既存 `wiki/source-summaries/` に溜まった `source-summary` を横断的に見直し、必要な `concept` ノートを `wiki/concepts/` に作成・更新・昇格・統合するための定期棚卸しフローです。

## いつ使うか

- 「concept 化できるものを整理して」「summary から横断統合して」「concept が足りないか見て」と頼まれたとき
- `raw-to-wiki` を何度か実行したあと、source-summary が増えたのに concept が増えていないとき
- 実質 concept のように複数 `raw` を抱える `source-summary` が出てきたとき
- 既存 `concept` が広くなりすぎ、テーマ別の入口に分けた方がよさそうなとき

`raw/*.md` を新しく wiki 化する作業は `raw-to-wiki` を使います。このスキルは、すでにある `wiki/` を棚卸しする後段フェーズです。

## 最初に読むもの

1. `CLAUDE.md`
2. `wiki/index.md`
3. 既存の `wiki/concepts/` ノート
4. 関連しそうな `wiki/source-summaries/` ノート
5. 必要なら source-summary の `sources` から辿れる `raw/*.md`

`CLAUDE.md` の規約が最優先です。特に、`concept` は複数ソースを横断して概念、運用フロー、判断基準を整理するノートとして扱います。

## 基本フロー

1. `wiki/index.md` と既存 `wiki/concepts/` を読み、現在の入口構造を把握する
2. `source-summary` を tags、関連ノート、`sources`、本文の論点でクラスタリングする
3. 各クラスタについて `既存 concept 更新` / `新規 concept 作成` / `source-summary 昇格` / `保留` を判定する
4. 必要な `concept` だけを `wiki/concepts/` に作成または更新する
5. 新規 concept や役割変更がある場合は `wiki/index.md` を更新する

## 判定基準

### 既存 concept 更新

次の条件なら、既存 `concept` に局所追記します。

- すでに自然な受け皿がある
- 新しい source-summary が既存 concept の判断基準、失敗モード、適用範囲を補強する
- 新規ノートを作ると、入口が細かくなりすぎる

### 新規 concept 作成

次の条件をすべて満たす場合だけ、新規 `concept` を作ります。

- 少なくとも 2 件以上の source-summary または既存 wiki を横断する
- 既存 concept への追記だけでは主題が埋もれる
- 後から探索するときの入口として使う可能性が高い
- 単なる話題タグではなく、概念、運用フロー、判断基準、失敗モードのいずれかを整理できる

同じ `raw` 由来の複数 wiki ノートは、concept 作成条件の「2 件以上」として重複カウントしません。

### source-summary 昇格

次の条件なら、既存 source-summary を `concept` へ昇格するか、別 concept を作って summary 側をリンク元に戻します。

- frontmatter の `sources` が複数 `raw` を持ち、本文も単一ソース要約を超えている
- 関連ノートから実質的な入口として参照されている
- タイトルや本文が特定記事ではなく横断テーマを扱っている

原則として source-summary は残し、横断部分を別 `concept` に抽出します。昇格を選ぶ場合も、元 raw ごとの単一ソース記録が失われないよう、必要なら source-summary を維持または分割します。

### 保留

次の条件なら、無理に concept を作りません。

- まだ 1 ソースしかない
- 既存ノートにリンクを足せば十分
- 横断テーマがタグ程度で、判断基準や運用フローまで育っていない
- 概念名は思いつくが、後から入口として使う確度が低い

保留する場合も、候補名、関連 source-summary、次に揃うと concept 化できる材料をユーザーへ返します。

## Concept ノートの作り方

frontmatter はこの形を基本にします。

```markdown
---
title: "<日本語タイトル>"
type: "concept"
sources:
  - "[[wiki/source-summaries/<source-summary>]]"
  - "[[raw/<必要なら raw note>]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - "wiki"
  - "concept"
  - "<topic>"
---
```

根拠にした source-summary は必ず `sources` に現在の保存先で入れます。既存 concept を根拠にした場合は `[[wiki/concepts/<concept>]]` を入れます。`raw` を直接読み直した場合は、その `raw` も追加します。

本文は固定しませんが、迷ったら次を使います。

```markdown
# 概要

## 基本構造

## 判断基準

## 失敗モード

## 関連ノート
```

## 避けること

- source-summary の品質を落としてまで概念化すること
- 1 ソースだけのテーマを急いで concept 化すること
- 既存 concept に自然に追記できる内容で新規ノートを量産すること
- `sources` が辿れない抽象論を書くこと
- `wiki/index.md` に入口として不要な細部まで載せること

## 実行チェックリスト

- [ ] `CLAUDE.md`、`wiki/index.md`、既存 `wiki/concepts/` を読んだ
- [ ] 関連する `wiki/source-summaries/` をクラスタリングした
- [ ] 実質 concept 化している source-summary の有無を確認した
- [ ] 各候補を `既存 concept 更新` / `新規 concept 作成` / `source-summary 昇格` / `保留` に分類した
- [ ] 必要な `concept` だけを `wiki/concepts/` に作成または更新した
- [ ] 新規入口や役割変更がある場合だけ `wiki/index.md` を更新した
- [ ] `sources`, `tags`, `updated` が適切に入っている
- [ ] 保留候補と理由をユーザーに伝えた

## ユーザーへの返し方

作業後は次を簡潔に伝えます。

1. 作成または更新した `concept` ノート
2. 昇格した、または昇格を見送った source-summary
3. 保留した concept 候補と理由
4. `wiki/index.md` を更新したかどうか
5. テストや確認をしていない場合はその旨
