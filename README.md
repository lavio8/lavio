# LAVISH / 大東建託コラム対応サイト

大東建託物件の初期費用比較サービス「LAVISH」のGitHub Pages用サイトです。
トップページは既存の問い合わせ用LPを維持し、`_posts` にMarkdownを追加すると `/column/英数字スラッグ/` の記事を公開できます。

## ディレクトリ構成

```text
.
├── index.html                 # 既存LP。問い合わせ・LINE導線を維持
├── column/
│   └── index.html             # 大東建託コラム・Q&A一覧
├── _posts/                    # Markdown記事。1ファイル追加で公開
├── _layouts/
│   ├── default.html           # 記事・一覧の共通HTMLとSEOメタ
│   └── post.html              # 記事テンプレート
├── _includes/                 # ヘッダー、著者、パンくず、CTA、フッター
├── assets/                    # LPで使用する画像
├── column.css                 # 記事・一覧ページのスマホファーストCSS
├── _config.yml                # URL、著者情報、CTA、Search Console設定
├── sitemap.xml
├── robots.txt
├── 404.html
└── favicon.svg
```

## ローカルでの確認方法

### 1. RubyとBundlerを用意する

GitHub Pagesと同じJekyll環境で確認するため、Rubyをインストールしたあと、初回だけ次を実行します。

```bash
bundle install
```

### 2. サイトを起動する

```bash
bundle exec jekyll serve --baseurl "" --url "http://127.0.0.1:4000"
```

ブラウザで次を開きます。

```text
http://127.0.0.1:4000/
http://127.0.0.1:4000/column/
http://127.0.0.1:4000/column/daito-shokihiyou/
```

これはローカル確認用に `baseurl` を空にしています。GitHub Pagesのプロジェクトサイトでは、設定ファイルの `baseurl: "/lavio"` が公開URLに反映されます。

生成だけ確認する場合は次を実行します。

```bash
bundle exec jekyll build
```

生成物は `_site/` に作られます。`_site/` はGitへコミットしません。

## GitHub Pagesでの公開方法

1. このフォルダの内容をGitHubリポジトリの公開ブランチへpushします。
2. GitHubリポジトリの **Settings → Pages** を開きます。
3. **Build and deployment** のSourceで **Deploy from a branch** を選びます。
4. 公開ブランチとフォルダは、通常は `main` / `/ (root)` を選びます。
5. 保存後、GitHub ActionsまたはPagesのビルド完了を待ちます。
6. 現在のプロジェクトサイトは `https://lavio8.github.io/lavio/` で公開されます。

GitHub PagesはJekyllを自動ビルドするため、`_posts`・`_layouts`・`_includes` をそのまま公開ブランチへ含めてください。`_site` をpushする必要はありません。

## 独自ドメインの設定方法

ドメイン名が決まるまでは、`CNAME` ファイルを作成していません。仮のドメインを入れたまま公開しないためです。

ドメインが決まったら次の手順で設定します。

1. `_config.yml` の次を変更します。

   ```yaml
   url: "https://example.com"
   baseurl: ""
   ```

   `example.com` は取得した実際のドメインに置き換えてください。

2. リポジトリのルートに `CNAME` という名前のファイルを作り、1行だけドメインを書きます。たとえば次のようにします。

   ```text
   example.com
   ```

   `https://`、パス、余分な空白は書きません。`www.example.com` を使う場合は、CNAMEファイルもその名前にします。

3. ドメイン管理会社のDNSで、GitHub Pagesの案内に従ってAレコードまたはCNAMEレコードを設定します。DNSの値はGitHubの公式ドキュメントに表示される最新値を使用してください。
4. GitHubの **Settings → Pages → Custom domain** に同じドメインを入力して保存します。
5. DNS反映後、**Enforce HTTPS** を有効にします。

`_config.yml` の `url` と `baseurl` は、canonical、OGP、構造化データ、sitemap、robots.txtのURLに共通で使われます。独自ドメインへ移行したら、古いGitHub Pages URLがcanonicalに残っていないか確認してください。

## 新しい記事の追加方法

1. `_posts/` に、次の形式のファイルをコピーして作ります。

   ```text
   YYYY-MM-DD-英数字スラッグ.md
   ```

   URLに日本語を使わないため、ファイル名と `permalink` は英数字・ハイフンで作ります。例：

   ```text
   _posts/2026-07-20-daito-chintai-shinsa.md
   ```

2. 既存記事のFront Matterをコピーし、`title`、`description`、`intro`、`date`、`updated`、`permalink`、`toc`、`faq`、`related` を変更します。
3. Markdown本文はFront Matterの下に書きます。`##` を大見出し、`###` を小見出しとして使い、結論を最初に書きます。
4. 目次の各 `id` と、本文の見出し末尾に書く `{#id}` を一致させます。
5. `faq` の質問と回答を追加すると、本文のFAQ欄とFAQ構造化データの両方に反映されます。
6. `related` に関連記事のタイトルとURLを追加します。
7. pushすると、GitHub Pagesのビルド後に記事一覧と記事ページへ反映されます。

記事Front Matterの最小例です。

```yaml
---
title: 大東建託の〇〇について
description: 検索結果に表示する120文字前後の説明文です。
intro: 結論を2〜3文で先に書きます。読者が最初に答えを理解できる文章にします。
date: 2026-07-20 09:00:00 +0900
updated: 2026-07-20 09:00:00 +0900
permalink: /column/daito-chintai-shinsa/
toc:
  - label: まず結論
    id: conclusion
faq:
  - question: よくある質問ですか？
    answer: 質問への短く明確な回答です。
related:
  - title: 大東建託の初期費用はいくらかかる？
    url: /column/daito-shokihiyou/
---

## まず結論 {#conclusion}

本文を書きます。
```

## 記事タイトルやdescriptionの変更方法

- 記事ページの検索タイトルは、各Markdownの `title` から生成されます。
- 検索結果の説明文、OGP、Article構造化データの説明は、各Markdownの `description` を使います。
- 記事の冒頭に表示する導入文は `intro` です。
- サイト全体の説明文と基本URLは `_config.yml` の `description`、`url`、`baseurl` です。
- 既存LPのタイトルとdescriptionは、トップの `index.html` の `<title>` と `<meta name="description">` を変更します。LPの文言・導線を変更するときは、LINEリンクを壊さないように注意してください。

## CTAリンクの変更方法

記事末尾CTA、記事ヘッダー、記事一覧、フッターの見積りリンクは、原則 `_config.yml` の `cta_url` を参照しています。

```yaml
cta_url: "https://lin.ee/6dwYhc3"
cta_label: AI10秒見積りを試す
```

LINEのリンクを変更するときは `cta_url` だけを変更してください。トップLP内の既存導線は `index.html` に直接書かれているため、必要に応じて同じリンクへ更新します。

## Google Search Console登録方法

1. Google Search Consoleでプロパティを追加します。独自ドメイン後はドメインプロパティを推奨します。
2. HTMLタグ方式で確認する場合、発行されたcontent値を `_config.yml` の `google_site_verification` に入れます。

   ```yaml
   google_site_verification: "Googleから指定されたcontent値"
   ```

3. pushしてGitHub Pagesのビルドが完了すると、トップページと記事ページのheadに確認タグが出力されます。
4. Search Consoleで確認を完了し、`https://公開ドメイン/sitemap.xml` をサイトマップとして送信します。

DNS確認方式を使う場合は、Search Consoleが指定するTXTレコードをドメイン管理会社のDNSへ追加します。その場合はHTMLタグを入れなくても構いません。

## 独自ドメイン設定後のHTTPS確認方法

1. GitHub PagesのCustom domainが正しく保存されていることを確認します。
2. DNSが反映されたあと、Pages設定の **Enforce HTTPS** をオンにします。証明書発行には時間がかかる場合があります。
3. `https://独自ドメイン/`、`https://独自ドメイン/column/`、`https://独自ドメイン/sitemap.xml` をブラウザで開きます。
4. ブラウザの鍵アイコン、リダイレクト、画像・CSSの表示を確認します。
5. 記事ページのソースで、canonical、OGP、Article構造化データ、FAQ構造化データ、パンくず構造化データが独自ドメインになっていることを確認します。

## 運用上の注意

- 記事のURLを公開後に変更すると、検索評価やリンクが途切れる可能性があります。変更が必要な場合は、旧URLからの転送方針を先に決めてください。
- 事実・料金・制度は変更される可能性があるため、`updated` を更新し、本文も見直してください。
- 価格や削減額を記載する記事では、物件・契約条件によって異なることを明記してください。
- 既存LPの問い合わせ導線は事業上重要なため、変更後はLINEボタンと見積り導線を必ず確認してください。
