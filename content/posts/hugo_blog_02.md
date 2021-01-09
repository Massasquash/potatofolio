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

### 今回のアジェンダ
x


## GitHub Pagesとは？
GitHubアカウントがあれば、リポジトリを作成することで簡単に静的サイトを公開できるツール。通常自前のサイトを公開するにはレンタルサーバーを借りてファイルをアップロードしたりする必要があるが、Github Pagesだとその手間がなく基本的に無料で公開できる。

## 
Hugo + GitHub Pagesでのサイト構築は、
[Hugoでgh-pagesにブログを作ってみた · var StraySheep](http://straysheep3.github.io/post/hugo-gh-pages-blog-create/)

## GitHubリポジトリの準備
[GitHub](https://github.com/)のページで、ブログ用のリポジトリを制作しておきます。


## 設定ファイルconfig.tomlに公開用設定を記述

**config.toml**
```toml
baseurl = "http://<GitHubに登録している名前>.github.io/" # github-pagesでのURL
publishDir = "docs" # 公開用ディレクトリをdocsに指定
canonifyurls = true # CSSを反映させるために必要
```

アップロードした後に「CSSが反映されていない」ということが起こったら、この設定ファイルを見直すと良さそうです。

- baseurlのパスの最後の`/`が抜けていないか
- cononifyurlsをtrueにしているか



## おわりに
今回はここまで。
今後、Github pagesを使って公開したり、テーマをカスタマイズしていったりします。