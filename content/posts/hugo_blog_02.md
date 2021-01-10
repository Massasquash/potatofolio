---
title: "[Hugoブログ02]GitHub Pagesで公開する"
date: 2021-01-08
description: ""
categories:
  - "Hugoブログ"
---

## はじめに
前回、以下のコマンドでローカルサーバーにアクセスしてブラウザ表示させるところまでできました。

```
hugo server -D
```

現状これだとブログはローカルでしか見られない状態（自分のPCからのみアクセスすることができる状態）なので、一般公開できる状態にしていきます。

今回`GitHub Pages`を利用しました。


## GitHub Pagesとは？
GitHubアカウントがあれば、リポジトリを作成することで簡単に静的サイトを公開できるツール。通常自前のサイトを公開するにはレンタルサーバーを借りてファイルをアップロードしたりする必要があるが、Github Pagesだとその手間がなく基本的に無料で公開できる。

Hugo + GitHub Pagesでのサイト構築は、この辺りを参考にさせていただいています。
[Hugo + GitHub Pages（独自ドメイン適応）でサイトを作成・公開する - Qiita](https://qiita.com/ysdyt/items/a581277dd1312a0e83c3)

こちらが公式。
[Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

## GitHubリポジトリの準備
まずは事前準備として、[GitHub](https://github.com/)のページで、ブログ用のリポジトリを制作しておきます。publicリポジトリで作らないと公開できなさそうです。

ここで制作したリポジトリ名が、公開した時のサイトURLになります。
`https://<GitHubユーザー名>.github.io/<リポジトリ名>/`

こんなURLになります。  
後から設定すれば独自ドメインを利用することもできるようですね。

また、GitHub Pagesを使って公開するやり方には現在２種類あり、

- Hugoで作ったファイル全てをmainブランチに公開する方法
- 公開するフォルダのみをGithubに公開するgh-pagesブランチを使う方法

がありますが、簡単なため前者のやり方で実装します。

※いろんな記事を参照するときは、GitHubのブランチ名が「master」「main」で混在しているので、更新日に注意。2020年10月からGitHubのデフォルトブランチ名は「main」になっています。


## 設定ファイルconfig.tomlに公開用設定を記述
設定ファイル`config.toml`に、ブログ公開のための設定を記述しておきます。

```toml
baseurl = "`https://<GitHubユーザー名>.github.io/<リポジトリ名>/`" # github-pagesで公開するURL
publishDir = "docs" # 公開用ディレクトリをdocsに指定
canonifyurls = true # CSSを反映させるために必要
```

後ほどでアップロードした後に「CSSが反映されていない」ということが起こることがあるのですが、その際にはこの設定ファイルを見直すと良さそうです。

- baseurlのパスの最後の`/`が抜けていないか
- cononifyurlsをtrue（絶対パス指定）にしているか

パスの指定が重要、ということでしょうか。

参考
[Hugoでgh-pagesにブログを作ってみた · var StraySheep](http://straysheep3.github.io/post/hugo-gh-pages-blog-create/)


## gitの初期設定とリモートリポジトリとの紐付け
ここからはコマンドラインで操作します。  
（gitのインストールは済んでいる前提になります）

まずは、gitの初期化をします。`config.toml`のあるディレクトリにいることを確認してから

```bash
git init
```

これでディレクトリに隠しファイル`.git`が作成されて、gitでのバージョン管理ができる状態になりました。次に、ステージングとコミット。

```bash
git add .
git commit -m "First commit"
```

次に、ローカルファイルをGitHubリポジトリにプッシュして両者を紐付けします。GitHubでリポジトリを作った時に出てきたページの、真ん中らへんにある「push an existing repository from the command line」の部分をコピペして実行出来ます。

```bash
git remote add origin https://github.com/<GitHubユーザー名>/<リポジトリ名>.git
git branch -M main
git push -u origin main
```

## GitHubリポジトリでPagesで公開するための設定
[TODO]詳細記述

「Settings」タブに移動して、そのまま下へスクロールしていくと下の方に「GitHub Pages」という項目があります。そこで色々設定。

ちょっと省略。

参考までに、僕の状態です。

![aaa][1]
[1]: posts/20210110_ghp.png

## ビルドを行い、リモートリポジトリへプッシュする
再びコマンドライン操作に戻ります。`hugo`コマンドを打ち込むだけで、ビルドが自動で行われます（ビルドについては後述）。  
この`hugo`コマンドは今後も使いますが、`config.toml`のあるディレクトリにいないと出来ないので、注意です。

```
hugo
```

これで、手元にあるHugoで作成したファイル群（元となるソースコード）からWebに公開するために必要なファイル群（HTMLやCSSなど）が生成された、と考えれば良さそうです。
`docs`というディレクトリに、公開するファイルが出来ているはず。

最後にステージング、コミット、プッシュの一連の

```bash
git add .
git commit -m "First Build"
git push origin main
```

これで、OKのはずです。
反映されるまで少し時間がかかると思いますが、
`https://<GitHubユーザー名>.github.io/<リポジトリ名>/`


## （コラム）ビルドの仕組み
[TODO]ビルドについて調べる

Hugoのビルドの仕組みがわかりやすそうだったので抜粋してみました。
[Hugo+Github Pagesで新しい個人ウェブサイトを作った - DEV Community](~https://dev.to/mshr_h/hugo-github-pages-35me~)

> hugoでビルドしたページはpublic/ディレクトリに生成される。このディレクトリをusername.github.ioにプッシュすることで、GitHub Pagesが更新される。
> hugoで作成したblogディレクトリをリモートのblog/リポジトリに紐付け、public/ディレクトリをusername.github.ioリポジトリに紐付ける。

上記にはpublicディレクトリとありますが、おそらく新しい情報だとこれがdocsディレクトリである必要がありそうです。


## おわりに
途中[TODO]として端折っている項目は、今後修正していきたいところです。  
今後は記事の更新についてと、テーマのカスタマイズについて。

---
MEMO
- git関連の理解
[何も考えずにGitでaddしてcommitしてpushしてるだけのあなたへ | Simple is Best](https://oldbigbuddha.dev/posts/for-git-beginners)

- ビルドの理解
[ビルド (build)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12775.html)