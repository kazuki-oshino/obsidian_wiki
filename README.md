# Obsidian Wiki Starter

このリポジトリは、`raw/` に集めた一次ソースをもとに、LLM が `wiki/` を継続的に育てるための Obsidian Vault テンプレートです。

## 基本の使い方

1. 記事やページを `raw/` に追加する

   例: [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf?hl=ja) などのツールで、Web 記事を Markdown として保存します。

2. `raw/` の記事を `wiki/` に反映する

   ```text
   /raw-to-wiki
   ```

   `raw/` の一次ソースを読み、`wiki/` 側に source-summary や関連ノートを作成・更新します。

3. 質問や調査結果を `wiki/` に残す

   ```text
   /query-to-wiki
   ```

   既存の `wiki/` をもとに回答し、再利用価値が高い内容だけを `wiki/` に還元します。

## ディレクトリ

- `raw/`: Web 記事、論文、メモなどの一次ソース置き場
- `wiki/`: 要約、概念整理、比較、入口ページなどの知識ノート
- `attachments/`: 画像や添付ファイル
- `.agents/`: この Vault 向けのエージェント用スキル

## 入口

`wiki/index.md` が `wiki/` 全体の目次です。新しいノートを追加したら、必要に応じてここも更新します。
