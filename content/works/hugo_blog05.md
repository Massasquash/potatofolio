---
title: "[Hugoブログ05]テーマのカスタマイズその２"
date: 2021-01-15
lead: "固定ページや記事に関する詳細を設定していく"
categories:
  - "Works"
tags:
  - "Hugoブログ"
---

# はじめに
前回、ブログ全体のタイトルなどの設定や、ウィジェットの設定などをやってきました。
今回はメニュやフッターに固定ページを追加したり、記事に関する設定をしていきます。

## 概要
まずブログのどのページからでもみられるグローバルメニューを作り、そこにトップページや固定ページへのリンクを追加します。
- 「HOME」 トップページへのリンク
- 「AUTHER」 プロフィールページ（固定ページ）
- 「ABOUT」 サイトの紹介ページ（固定ページ）

フッターにも固定ページ「プライバシーポリシー」ページを追加します。

次に、記事に関する設定をします。
- 一覧ページの設定：「ReadMore」ボタンの設置や一覧表示する際の文字数を指定
- 個別ページの設定：目次、著者情報、ページャー（次の記事/前の記事）の追加


# 実装
## メニューを表示させて「HOME」へのリンクを追加
まずは、メニューの設定をします。
初期状態のままだとメニューは何もない状態です。この状態で`config.toml`に以下を追加することで「HOME」メニューを追加します。
```toml
[[Menus.main]]
  name = "Home"
  url = "/"
  weight = 1
```

`url`はトップページへのリンクにしたいので、相対パス`/`を指定します。
`weigth`に数値を指定することで、複数のメニューを作る場合に順番を変えれられるようです。この数値を指定しない場合は順番は`name`の昇順になるようです。  
右にいくにつれて大きな値になるように指定すれば良さそうですなのですが、あまりこの書き方がよくわかっておりません。公式ページ[Menus | Hugo](https://gohugo.io/content-management/menus/#readout)を見ると`weight`はマイナス指定しているので、そのほうが良いのでしょうか…？

## メニューに「Auther」「About」の固定ページを追加する
続いて、メニューに固定ページを追加していきます。

メニューにページを追加する際にやることは、
- `content/`ディレクトリに記事ページをmdファイルで作る
- `config.toml`にリンクを設定する。

という流れになります。  
`content/`以下に記事ファイルを作ることで、Hugoビルド時にファイル名から自動でurlを生成してくれます。
例えば記事ページを`auther.md`, `about.md`という風に作成すると、それぞれ`/auther/`, `/about/`という`content/`からの相対パスでurlが出来上がります。

`config.toml`には
```toml
[[Menus.main]]
  name = "auther"
  url = "/auther/"
  weight = 2

[[Menus.main]]
  name = "about"
  url = "/about/"
  weight = 3
```

という風に記述してやれば、固定ページが完成です。

固定ページのmdファイルには好きな情報を書きましょう。  
ちなみにこの固定ページにも一般記事と同じようにフロントマターを記述します。最上部には以下を追加しておきます（詳細は後述）
```
---
title: このブログについて # 固定ページ名
toc: false # 目次を表示させない
pager: false # ページャーを表示させない
---
```

## フッターに「プライバシーポリシー」ページを追加
メニューに固定ページを追加した時と同様の手順で追加できます。
記事ファイルを作り上記と同じようにフロントマターを記述し、`config.toml`に

```
[[Menus.footer]]
  name = "プライバシーポリシー"
  url = "/privacy/"
```

というブロックを追加してやります。


## 記事一覧ページを整える
一般的なブログでよく見かける「Read More」ボタンを作っていきたいと思います。
記事一覧ページに記事の最初の数十文字以外が省略されて表示され、ボタンをクリックすると記事詳細ページに飛べるやつです。

記事の一覧ページは「List Page Template」と呼ばれるテンプレートとして定義されているようです。  
(Hugo公式 [Lists of Content in Hugo | Hugo](https://gohugo.io/templates/lists/#readout))

トップページに表示されている記事一覧、またカテゴリやタグを一覧表示した際にもこのテンプレートが使われているようです。

この設定もやはり`config.toml`に記述していきます。  

```toml
languageCode = "ja"
.
.
paginate = "10" # 記事一覧の１ページに表示する記事数
hasCJKLanguage = true # 日本語の単語カウントを有効にする
summaryLength = 20 # サマリーの長さ
```

最初に`languageCode = "ja"`と、言語を日本語を指定していることを確認。  
そして`hasCJKLanguage = true`を指定することで、日本語で単語カウントをするように設定できます。  
（この行がないと日本語で思い通りの長さになりません）  
`summaryLength`が文字数なので、好きな数をここで指定してやります。

これだけでもいいのですが、「ReadMore」ボタンを追加してみます。
こちらは`[Params]`ブロックに記述します。

```toml
[Params]
  .
  .
  readmore = true
```

## 記事詳細ページを整える
記事一覧ページの設定ができたら、次は個別の記事詳細ページの設定もここでしておきます。

記事の個別ページは「Single Page Template」と呼ばれるテンプレートとして定義されているようです。  
(Hugo公式 [Single Page Templates | Hugo](https://gohugo.io/templates/single-page-templates/))

まずは全体に適用するものを、こちらも`config.toml`の`[Params]`に記述。  
先ほど書いた`readmore = true`の下に続ける形で大丈夫です。

```toml
[Params]
  .
  .
  readmore = true
  authorbox = false
  pager = true
  toc = true # Enable Table of Contents for specific page
```

`authorbox`は、記事を開いたときに一番下に表示される、著者情報です。  
僕の場合はfalseにして使っていませんが、これを`true`にして使用したい場合は、著者名やプロフィール、アイコンを`Author`ブロックを作ってこちらに書けばOKです。  
（`avatar`でアイコン画像を指定。画像を指定するやり方はタイトル部のロゴ表示と同じ考え方でいけるのかなと思います。[Hugoブログ04テーマのカスタマイズその１ - Potatofolio](https://massasquash.github.io/potatofolio/posts/hugo_blog04/#%E3%83%96%E3%83%AD%E3%82%B0%E3%81%AE%E5%9F%BA%E6%9C%AC%E6%83%85%E5%A0%B1)）

```toml
[Author]
  name = "Massa"
  bio = ""
  avatar = "img/avatar.png"
```

`pager`は各記事に「次へ」「前へ」リンクをつけて、記事間を移動できるようなる機能。便利なのでtrueにしておきます。  
`toc`は記事の頭に目次を表示するかどうか。Markdownの見出し（#, ##, ###, ...）に対応しています。これも便利なのでtrueにしました。

ここで`config.toml`に指定しておくことで、全ての記事に適用されます。  
これらは、各記事のフロントマターに設定することで記事ごとに個別に設定もできます（上で個別ページを制作したときに指定したのがそれでした）。


# おわりに
さて、これで一通りブログの設定ができたかなと思うので、導入としては一旦ここまでとします。  
今後、Google Analyticsとの連携や、Disqusを使ったコメント機能の追加、またサーチバーをもっと使いやすいようにカスタマイズしていきたいと思っています  

この辺りはまだ手付かずですので、今後の課題とします。

---
## MEMO
- Hugo公式ページ
  - [Menus | Hugo](https://gohugo.io/content-management/menus/#readout)
  - [Lists of Content in Hugo | Hugo](https://gohugo.io/templates/lists/#readout))
  - [Single Page Templates | Hugo](https://gohugo.io/templates/single-page-templates/))
---