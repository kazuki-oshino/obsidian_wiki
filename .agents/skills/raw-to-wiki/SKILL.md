---
name: raw-to-wiki
description: Compile one or more notes under `raw/` into this project's Obsidian wiki. Use when the user gives a `raw/*.md` path and asks to wiki化, compile the wiki, create a source summary, update concept notes, summarize newly added source material, or reflect new raw files into `wiki/`.
---

# Raw To Wiki

このスキルは、この Vault の `raw/` ノートを `wiki/` ノートへ変換・更新するための定型ワークフローです。

## いつ使うか

- ユーザーが `raw/...md` の path を渡して `wiki` に反映したいとき
- 「wiki化して」「compileして」「source summary を作って」「concept note も更新して」のように頼まれたとき
- 新しい一次ソースを `raw/` に追加したあと、Vault の知識ノートへ落とし込みたいとき

## 最初に読むもの

1. `CLAUDE.md`
2. 指定された `raw/*.md`
3. 関連しそうな既存 `wiki/*.md`
4. `wiki/index.md` があれば読む

`CLAUDE.md` の規約が最優先です。特に次を守ります。

- 人間が主に触るのは `raw/` と schema であり、編集対象は主に `wiki/`
- `raw/` は出典保管であり、要約や再構成は `wiki/` 側で行う
- `wiki/` ノートは原則日本語
- `wiki/` ノートには `title`, `type`, `sources`, `created`, `updated`, `tags` を入れる
- `sources` には `[[raw/...]]` を入れる
- 既存ノート更新時は `updated` を更新し、有用な観点やリンクを消さずに改善する

## 入力

- 必須: `raw/` 配下の 1 件以上の Markdown path
- 任意: 「新規作成のみ」「既存 concept も更新」「関連候補だけ提案」などの追加指示

path が曖昧なら、どの `raw` ファイルを対象にするか確認してから進めます。
指定された `raw` path が存在しない、または読めない場合は、同名記事や外部 URL で代替せず、対象ファイルを確認します。`raw/` は根拠そのものなので、実際に読めないソースを読んだ前提で wiki 化しません。

## 出力方針

基本は次の順で処理します。

1. 対象ソースの `source-summary` を `wiki/` に新規作成または更新する
2. 関連する既存 `concept` ノートがあれば更新する
3. 必要条件を満たすときだけ `overview` または `comparison` の作成・更新を検討する
4. `wiki/index.md` を更新し、新規または更新ノートの入口情報を反映する
5. 複数ソースをまたぐ論点が新しく成立した場合だけ、新しい `concept` ノート作成を検討する

不要な大量生成は避け、変更は最小限に保ちます。

## Source Summary の作り方

### ファイル名

- `wiki/` 側のファイル名は短い kebab-case slug にする
- `raw` のファイル名をそのまま写すのではなく、内容を代表する短い名前を優先する
- 既存の対応ノートがあればそれを更新し、重複ノートを作らない

### frontmatter

この形を基本にします。

```markdown
---
title: "<日本語タイトル>"
type: "source-summary"
sources:
  - "[[raw/<raw note name>]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - "wiki"
  - "source-summary"
  - "<topic>"
---
```

### 本文の推奨構成

見出しは固定しなくてよいですが、少なくとも「何のノートか」「重要ポイント」「関連ノート」が分かる構成にします。迷ったら、まずは次の最小構成を使います。

```markdown
# 概要

## 主要ポイント

## 関連ノート
```

`source-summary` はまずソース中心に書きます。次の見出しは、必要なときだけ追加します。

- `## この Vault への当てはめ`
  - そのソースがこの Vault の運用、構造、更新方針に直接つながるときだけ使う
  - 汎用論文や一般論を無理に Vault 向けへ翻訳するための定型見出しにはしない

前提条件、適用範囲、失敗モード、一般化しにくい点は、まず `主要ポイント` に含めます。内容量が多く、独立させた方が読みやすいときだけ、そのノートに合わせた補助見出しを個別に立てます。

### 書き方

- 原則日本語で書く
- 固有名詞や論文名は必要に応じて原文併記
- 単なる要約で終わらず、あとで参照・再利用しやすい観点を抽出する
- `主要ポイント` は、良い点だけでなく、前提、適用範囲、留保、失敗しやすい条件も含めた「持ち帰る論点全体」と考える
- Vault 固有の運用解釈は、ソースとの接続が明確な場合だけ独立セクションにする
- Karpathy のように運用論そのものが主題のソースでは、Vault に効く論点も `主要ポイント` に統合してよい
- 箇条書きは重複なく圧縮し、論点の粒度を揃える
- 断定はソースの根拠に沿って行い、過剰な一般化を避ける

## Concept Note の更新判断

既存 `concept` ノートを優先して育てます。

- 新しい source-summary が既存 concept の論点を補強するなら、その concept に `sources` と本文の観点を追加する
- 既存 concept に自然に入らないが、少なくとも 2 ソース以上を横断する安定した概念がある場合だけ、新しい `concept` を作る
- まだ 1 ソースしかないテーマは無理に concept 化せず、関連候補として残す

`concept` の frontmatter では `type: "concept"` を使います。

## `overview` / `comparison` の作成判断

`source-summary` と `concept` が主軸です。追加ページは、再利用価値が明確なときだけ作ります。

- `overview`
  - 特定テーマの source-summary や concept が増え、入口ページがあった方が読み進めやすいとき
  - ユーザーが「全体像を整理して」「入口ノートを作って」と求めたとき
- `comparison`
  - ユーザーが比較を明示的に依頼したとき
  - 複数ソースに対立点や比較軸があり、後から参照する価値が高いとき
- 既存の `overview` / `comparison` が自然に受け皿になるなら新規作成より更新を優先する
- 再利用価値が弱い一時的な回答は、無理に独立ノート化しない

## raw の扱い

- `raw/` 本文の意味を書き換えない
- `raw/` を編集するのは、frontmatter 補完、明らかな整形崩れ修正、インポート崩れ修正に限定する
- wiki 化の中心作業は必ず `wiki/` 側で行う

## 実行チェックリスト

以下を順に確認します。

- [ ] 対象 `raw` を読み、主題・論点・出典情報を把握した
- [ ] 関連する既存 `wiki` ノートの重複有無を確認した
- [ ] `source-summary` を新規作成または更新した
- [ ] 必要なら既存 `concept` を更新した
- [ ] 必要なら `overview` または `comparison` を作成または更新した
- [ ] `wiki/index.md` を新しい状態に合わせて更新した
- [ ] `sources`, `tags`, `updated` が適切に入っている
- [ ] wikilink が自然につながっている
- [ ] `raw` を不必要に書き換えていない

## ユーザーへの返し方

作業後は次を簡潔に伝えます。

1. 作成または更新した `wiki` ノート
2. 必要に応じて更新した `concept` ノート
3. 必要に応じて更新した `overview` / `comparison`
4. concept 化や成果物化を見送ったが、今後候補になりそうなテーマ
5. `index.md` を更新したかどうか
6. テストや確認をしていない場合はその旨

## 具体例

実例は [examples.md](examples.md) を読むこと。
