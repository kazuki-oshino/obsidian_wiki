# Obsidian Wiki Starter

このリポジトリは、`raw/` に集めた一次ソースをもとに、LLM が `wiki/` を継続的に育てるための Obsidian Vault テンプレートです。

## 基本の使い方

1. 記事やページを `raw/` に追加する

   例: [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf?hl=ja) などのツールで、Web 記事を Markdown として保存します。

2. `raw/` の記事を `wiki/` に反映する

   ```text
   /raw-to-wiki
   ```

   `raw/` の一次ソースを読み、`wiki/source-summaries/` 側に source-summary を作成・更新します。必要に応じて concept 判定も行います。

3. 質問や調査結果を `wiki/` に残す

   ```text
   /query-to-wiki
   ```

   既存の `wiki/` をもとに回答し、再利用価値が高い内容だけを `wiki/` に還元します。

4. 溜まった source-summary から concept を棚卸しする

   ```text
   /concept-harvest
   ```

   `wiki/source-summaries/` を横断して、`wiki/concepts/` にまとめるべき概念や保留候補を整理します。

## ディレクトリ

- `raw/`: Web 記事、論文、メモなどの一次ソース置き場
- `wiki/index.md`: wiki 全体の入口
- `wiki/source-summaries/`: 1 ソースごとの要約ノート
- `wiki/concepts/`: 複数ソースを横断する概念ノート
- `wiki/comparisons/`: 比較ノート
- `wiki/overviews/`: テーマ別の入口ページ
- `attachments/`: 画像や添付ファイル
- `.agents/`: この Vault 向けのエージェント用スキル

## 入口

`wiki/index.md` が `wiki/` 全体の目次です。新しいノートを追加したら、必要に応じてここも更新します。
