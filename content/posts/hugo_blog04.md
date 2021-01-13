---
title: "[Hugoブログ04]テーマのカスタマイズその１"
date: 2021-01-11T10:15:30+09:00
lead: "最低限のカスタマイズでブログを整える"
categories:
  - "Hugoブログ"
---

## はじめに
Hugoの構造を理解しながらカスタマイズを行っていきます。
僕の使っているテーマは「[Mainroad](https://github.com/vimux/mainroad/)」なので、こちらでカスタマイズしていきます。

基本的には、

- 記事や載せたいページは`/content`以下ディレクトリに追加
- 設定は`config.toml`をいじっていく
- 記事やページそれぞれにもフロントマターを記述することで細かい設定ができる

というイメージです。

こちらのページで詳しく書かれていて、とてもわかりやすいです。
[Hugo によるブログ作成と mainroad テーマのカスタマイズ - terashim.com](https://terashim.com/posts/create-hugo-blog-and-customize-mainroad-theme/)

## 概要
こちらから飛べるデモサイトのような作りにしていきたいです。
[Mainroad | Hugo Themes](https://themes.gohugo.io/mainroad/)

[Mainroadの公式GitHubページ](https://github.com/vimux/mainroad/#Configuration)の`REAME`に`config.toml`のサンプルが載っているので、そちらを活用していきます。


この設定を丸々コピペしてくるだけだと、僕の環境だとどうもエラーが発生して解消しませんでした。  
必要な部分だけ理解しながらコピペしてきて、追加したい機能が出てきたら後で追加する、というやり方が今後のためにも良いかなと思います。

また、Hugoを完全に理解したり、HTML, CSSやGolangを読み解く…というところまではここでは目標としていません。
今回は、必要に応じて表示させてみて試しながら、調べたことや[Hugoのドキュメント](https://gohugo.io/documentation/)と照らし合わせてカスタマイズしていきます。

## ブログの基本情報
- タイトル
- 日本語設定
- サブタイトル、ロゴ
- フッターのクレジット表記

基本情報を先に設定します。`config.toml`をいじっていきます。  
タイトルやテーマ、github公開用の設定などはすでに記述済みかと思うので、それ以外の部分を追記します。

```toml
title = "Potatofolio" # ページタイトル
languageCode = "ja" # 言語指定
theme = "mainroad"

# github公開用
baseurl = "https://massasquash.github.io/potatofolio/"
publishDir = "docs"
canonifyurls = true

[Params]
  description = "This Is My Development Note" # サイトの詳細
  copyright = "Massa" # フッターに表示されるコピーライトの名前

[Params.logo]
  subtitle = "Agrifeel-LABO - My Learning And Development Note" # ロゴサブタイトル。最上部に表示
  image = "img/page_logo.jpg" # ロゴイメージ。"static"からの相対パス。
```

`title`に指定したタイトルは、ページ最上部に表示されるのと、ブラウザで開いたときのタブに表示させるタイトルにもなります。

ページ最上部のタイトルの下に表示させるサブタイトル・ロゴは`[Params.logo]`のブロックに指定します。
イメージファイルをロゴに表示させたい場合は、`static/`ディレクトリ以下に突っ込むのが良さそうです。例えば僕の場合はタイトルロゴは`static/img/`直下、記事に関係するものは`static/img/posts/`ディレクトリを作って入れています（以下のような構造）
```
static
`- img
  `- posts
    |- <画像>.jpg
    |- <画像>.jpg
    `- <画像>.jpg
  `- page_logo.jp
```

画像を`config.toml`で指定する際は`static/`ディレクトリからの相対表記で書きます。

[Params]ブロックの`description`と、[Params.logo]の`subtitle`の違いが少しわかりづらいですが、   
`description`の方は記述した文字列はWeb上には一見表示されません。HTMLを見るとメタ情報に入っているようです。検索結果にタイトルと一緒に表示される簡単な説明文に当たるのでしょうか。

[meta description（メタディスクリプション）タグの役割と最適化方法](https://bazubu.com/how-to-optimize-meta-description-26891.html)


## デザイン
- 区切り線のカラーを設定

```config.toml
# Style
[Params.style.vars]
  highlightColor = "#2d91e2" # ハイライトカラー
```

色によってガラッと全体の雰囲気が変わる気がします。
適当に「#2d91e2」をコピペしてGoogle検索にかけると色のシミュレーターが出てくるので、それを使って好きな色を探すととても便利です。


## メインの表示
- メインセクションの設定

```config.toml
  mainSections = ["posts"] # ホームページ、"Recent articles"ウィジェットに表示させる内容
```

記事の入ってる`content`ディレクトリ以下のディレクトリ名を指定します。ここでは`content/posts/`の中に記事を作っていく設定になっています。  
ただこれ、指定しなくても記事数が多いディレクトリが自動で認識されるようですね。


## ウィジェット
- ウィジェットの表示設定
- ウィジェットに載せる内容の詳細設定

```
[Params.sidebar]
  home = "right"   # トップページのウィジェット設定
  list = "right"   # リストページのウィジェット設定
  single = "right" # シングルページのウィジェット設定
  # Enable widgets in given order
  widgets = ["search", "recent", "categories", "taglist", "social"]

[Params.widgets]
  recent_num = 5 # ウィジェットの"Recent articles"に表示する記事数
```

だいたい[Mainloadの公式ページのConfigration](https://github.com/vimux/mainroad/#Configuration)のサンプルコードから持ってきています。

ウィジェットの設定項目は[Params.sidebar]ブロックに記述。
`home`はトップページ、`list`は記事一覧ページ、`single`は記事ページに当たるようです。それぞれ`right`, `left`, `false`どれかを指定できます（falseだとそのページにウィジェットを表示させない）
今回はウィジェットの位置を右側に固定したいため全て`right`を指定しました。

ウィジェットに何を表示させるか、というのはここで指定します。
`widgets = ["search", "recent", "categories", "taglist", "social"]`

## ウィジェットのソーシャル情報
[TODO]説明追加
上でウィジェットに`"social"`と入れると、ウィジェットの最後にSNSへのリンクボタンが表示されているはずです。
こちらをいじっていきます。


```config.toml
Params.widgets.social]
  # Enable parts of social widget
  twitter = "massasquash"
  github = "Massasquash"

# Custom social links
[[Params.widgets.social.custom]]
  title = "Note"
  url = "https://note.com/agrifeel_labo"
  #icon = "youtube.svg"

[[Params.widgets.social.custom]]
  title = "Youtube"
  url = "https://www.youtube.com/channel/UCsu1mENsBiVFsdc-yq0a4Aw"
  #icon = "youtube.svg"
```


## おわりに
次はメニューバーから飛べる固定ページを作ったり、記事に関する詳細な設定を追加していきます。

---
## MEMO
- Hugo公式
  - [Hugo Documentation | Hugo](https://gohugo.io/documentation/)

- Mainroadテーマに関して
  - [Mainroad | Hugo Themes](https://themes.gohugo.io/mainroad/)
  - [GitHub - Vimux/Mainroad: Responsive, simple, clean and content-focused Hugo theme based on the MH Magazine lite WordPress theme](https://github.com/vimux/mainroad/)
---