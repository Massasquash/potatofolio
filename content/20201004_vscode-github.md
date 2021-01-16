title: 新規作成を始める際のGithub・ローカルフォルダ作成の覚え書き
Slug: vscode-github
Date: 2020-10-04 13:19
Category: Article
Tags: github
Summary: Github立ち上げ手順、ローカルフォルダ名の付け方ルールなど

## はじめに
新しくプロジェクトを作成するときの手順の備忘録
完全自分用
VSCode, Githubを利用

## 前提：VSCodeの活用
ローカルの`Documents`フォルダをワークスペースに突っ込んでおく。

## （１）個人利用目的のツール制作を始める際の手順

### Githubリポジトリ
こちらに追加していく形で制作する
[GitHub - Massasquash/mytools: 自作のツール置き場[Python]](https://github.com/Massasquash/mytools)

### ローカルディレクトリ制作
`[DEV]my-tools`に新しいフォルダを作成し、その中で制作していく。
名前は`20200925_pdf_to_pic`のように、

- 着手年月日 8桁
- プロジェクト名

をつけるようにする。

以降、制作を進めて適宜コミット・プッシュする

## （２）新規プロジェクトを始める際の手順

### Githubリポジトリの作成
Github公式より、新規リポジトリを作成する。
名前の付け方ルールはおいおい整備。


### ローカルにクローンし、フォルダ名を変更

- Githubで作成したリポジトリのhttpsのURLをコピー
- ターミナル操作をしてクローンを作成。
- フォルダ名を`[DEV]`から始まるプロジェクト名に変更しておく

```
cd Documents
git clone <https://github***/***.git>
```

### git初期化とプッシュ
- 必要に応じて`.gitignore`を用意

以下はクローンした際に必要かどうかわからないけど、初期化
```
git init
git add .
git commit -m "First Commit"
```

- リポジトリへのpush
```
git push origin master
```


## おまけ：gitignoreの書き方
gitignoreを書くときは

- 特定の拡張子を除きたい場合
`*.jpg`
`*.ipynb`

- 特定の名前から始まるファイルを除きたい場合
正規表現が使える
`arcive*`

## 気になること：
- ファイルパスをどうするか、githubにあげない方が良さそうだが、どう管理する？


## 参考
[VSCodeとGitHubの連携方法【Mac】 - Qiita](https://qiita.com/ynack/items/b6daaa6aedc285fddf6f)