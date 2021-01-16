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

## ブログの基本情報の設定
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
|- img
| `- posts
|   |- <画像>.jpg
|   |- <画像>.jpg
|   `- <画像>.jpg
`- page_logo.jp
```

画像を`config.toml`で指定する際は`static/`ディレクトリからの相対表記で書きます。

[Params]ブロックの`description`と、[Params.logo]の`subtitle`の違いが少しわかりづらいですが、   
`description`の方は記述した文字列はWeb上には一見表示されません。HTMLを見るとメタ情報に入っているようです。検索結果にタイトルと一緒に表示される簡単な説明文に当たるのでしょうか。

[meta description（メタディスクリプション）タグの役割と最適化方法](https://bazubu.com/how-to-optimize-meta-description-26891.html)


## デザイン（カラー）の設定
- 区切り線のカラーを設定

```config.toml
# Style
[Params.style.vars]
  highlightColor = "#2d91e2" # ハイライトカラー
```

色によってガラッと全体の雰囲気が変わる気がします。
適当に「#2d91e2」をコピペしてGoogle検索にかけると色のシミュレーターが出てくるので、それを使って好きな色を探すととても便利です。


## メインの表示設定
- メインセクションの設定

ホームページ（トップのページ）に表示したい記事のディレクトリをここで指定します。
ここで指定したディレクトリが、ウィジェットの「最新記事一覧」にも反映されます（ウィジェットについては後述）。

```config.toml
  mainSections = ["posts"]
```

この配列の中に記事の入ってる`content`ディレクトリ以下のディレクトリ名を指定します。  
今回は`content/posts/`の中に記事を作っていこうと思うので、`["posts"]`と指定します。  
参考までに、以下が僕の作っているディレクトリです。

```
content
|- posts
| |- hugo_blog01.md
| |- hugo_blog02.md
| `- hugo_blog03.m
|- about.md
|- auther.md
`- privacy.md
```

この場合、postsディレクトリに入っている記事だけがホームページと最新記事一覧に表示されて、それ以外の記事はここには表示されません。
ただこれ、指定しなくても記事数が多いディレクトリが自動で認識されるようですね。
また、`["posts", "news", "novels"]`のように複数のディレクトリを指定すると、指定したディレクトリ全てが反映されるようです。

現状あまり使い分けがわからないので、記事ディレクトリは分けずに１つだけにします。


## ウィジェットの設定
- ウィジェットの表示設定
- ウィジェットに載せる内容の詳細設定

ウィジェットは、ブログのサイドバーに表示させるものです。
このサイドバーを左に表示するか右に表示するか、また「最新記事一覧」「カテゴリー一覧」など何を表示するのかを指定することができます。

```
[Params.sidebar]
  home = "right"   # トップページのウィジェット設定
  list = "right"   # リストページのウィジェット設定
  single = "right" # シングルページのウィジェット設定
  widgets = ["search", "recent", "categories", "taglist", "social"]

[Params.widgets]
  recent_num = 5 # ウィジェットの"Recent articles"に表示する記事数
```

ウィジェットの設定項目は｀[Params.sidebar]｀ブロックに記述。
`home`はトップページ、`list`は記事一覧ページ、`single`は記事ページに当たるようです。それぞれ`right`, `left`, `false`どれかを指定できます（falseだとそのページにウィジェットを表示させない）

今回はウィジェットの位置をどのページからでも右側に固定したいため、全て`right`を指定しました。

ウィジェットに何を表示させるか、というのはここで指定します。
`widgets = ["search", "recent", "categories", "taglist", "social"]`

- serch:      サーチバー
- recent:     最新記事一覧
- categories: 記事に指定したカテゴリ一覧
- taglist:    記事に指定したタグ一覧
- social:     SNSへのリンク


## ソーシャル情報の設定
上でウィジェットを指定した配列に`"social"`を入れていると、ウィジェットにSNSへのリンクボタンが表示されているはずです。
こちらのリンクをいじっていきます。

[Mainroad公式](https://github.com/vimux/mainroad/#Configuration)からコピーさせてもらったサンプルコードを使います。

```config.toml
[Params.widgets.social]
  # Enable parts of social widget
  facebook = "username"
  twitter = "username"
  instagram = "username"
  linkedin = "username"
  telegram = "username"
  github = "username"
  gitlab = "username"
  bitbucket = "username"
  email = "example@example.com"
```

自分の使っているユーザーネームを指定するだけで、勝手にリンクを作ってくれます。便利ですね。  
使っていないSNSについては、行ごと消してしまいます。

また、これ以外のリンクも追加することもできます。

```
# Custom social links
[[Params.widgets.social.custom]]
  title = "Youtube"
  url = "https://youtube.com/user/username"
  icon = "youtube.svg" # Optional. Path relative to "layouts/partials"

[[Params.widgets.social.custom]]
  title = "My Home Page"
  url = "http://example.com"
```

`[[Params.widgets.social.custom]]`ブロックを増やして行くとリンクも増えていく感じでしょうか。
この`icon`に画像を指定できますが、ここでもやはり`static/`ディレクトリからの相対パスで指定します。なので`static/`ディレクトリに追加したいSNSリンクの画像を入れてやります（なくても問題ありません）。

どこかで見た気がするのですが、仮に`static/`ディレクトリに指定のパスが見つからない場合は、`themes/mainroad/static/`とテーマの中にある`static/`ディレクトリの中を探しに行ってくれている様子です。

[TODO]ソース


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