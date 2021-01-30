---
title: "[Hugoブログ06]ブログにipynbファイルをアップする"
date: 2021-01-30
lead: "Jupyter Notebookで作ったipynbファイルをそのままブログ記事にする"
categories:
  - "Hugoブログ"
draft: true
---

# はじめに
Pythonの学習のために、Jupyter Notebookをそのままページにしたかったので、調べながらやってみました。

## 概要
ターミナル操作
- （１）HomebrewでGoをインストールする
- （２）Jupyter Notebook Handlerをインストールする

ipynbファイルの設定
- （３）frontmatterメタデータを設定する
- （４）Hugoのコンテンツディレクトリにipynbファイルを保存する

- （５）Hugoプロジェクトディレクトリにhugo.goを作成
- （６）hugo.goを実行

# 実装
## （１）HomebrewでGoをインストールする
こちら[MacにGoをインストールする - Qiita](https://qiita.com/sunnyG/items/cabc700e6d9a28219cc8)を参照しながら、Go言語をインストールします。  
インストールには時間がかかるので、しばらく待ってからバージョンを表示させてインストールできているか確認します。

```bash
brew install go

go version
# -> go version go1.15.6 darwin/amd64
```

## （２）Jupyter Notebook Handlerをインストールする
こちら[Jupyter NotebookをHugoのコンテンツとして使う方法 | kuune.org](https://kuune.org/text/2017/07/27/how-to-use-jupyter-notebook-as-hugo-content/)を参照しながら、「Jupyter Notebook Handler for Hugo」をインストールします。  

```bash
go get -u -v github.com/naoina/hugo-jupyter-handler
```


## （３）frontmatterメタデータを設定する
Jupyter Notebookを立ち上げてブログにアップしたいipynbファイルを開き、以下のメタデータを設定します。  
「編集」＞「ノートブックのメタデータを編集」より編集します。  
ここで設定したものが、そのままHugoのフロントマターとして使われるようです。  


以下にもコードなどは残しますが、公式[GitHub - naoina/hugo-jupyter-handler: Jupyter Notebook Handler for Hugo (Deprecated)](https://github.com/naoina/hugo-jupyter-handler#usage)に従ってやるのが良いと思います。  

すでに色々書かれているのはいじらないようにして、最初の`{`の直後に、以下をコピペします。  
最初と最後の`{}`が対になっているのでそれを触らないようにして、次の` "kernelspec": { ... }`以下と同列の構造になるようにします。  

```json
  "frontmatter": {
    "title": "[退屈Python]7章 正規表現によるパターンマッチング　演習",
    "date": "2021-01-30T21:00",
    "lead": "「退屈なことはPythonにやらせよう」の学習備忘録",
    "categories": [
      "退屈Python"
    ]
   },
```

JSON形式なので、ipynbをテキストエディタで開いて直接打ち込んでも可能です。  
僕の環境だとVSCodeで開くと勝手にJupyter Notebook形式で開いてしまうので、フロントマター編集の仕方がわかりませんでした。要チェック。


## （４）Hugoのコンテンツディレクトリにipynbファイルを保存する
ブログにアップしたいipynbファイルに、ファイルをHugoのコンテンツディレクトリに保存します。  
「ファイル」＞「名前を付けてダウンロード」＞「Notebook(ipynb)」で、ディレクトリを指定して保存します。  
ディレクトリは、僕の環境だとHugoプロジェクトの`content`以下になります（通常はこのディレクトリのはずです）。

## （５）Hugoプロジェクトディレクトリにhugo.goを作成
Hugoのプロジェクトディレクトリに`hugo.go`という名前のテキストファイルを作成し、以下をコピペします（これも公式のGithubのページより）。  

```go
package main

import (
	"os"
	"runtime"

	"github.com/gohugoio/hugo/commands"
	jww "github.com/spf13/jwalterweatherman"

	_ "github.com/naoina/hugo-jupyter-handler"
)

func main() {
	runtime.GOMAXPROCS(runtime.NumCPU())
	commands.Execute()
	if jww.LogCountForLevelsGreaterThanorEqualTo(jww.LevelError) > 0 {
		os.Exit(-1)
	}

	if commands.Hugo != nil {
		if commands.Hugo.Log.LogCountForLevelsGreaterThanorEqualTo(jww.LevelError) > 0 {
			os.Exit(-1)
		}
	}
}
```

これは、参考ページによると「Hugo CLIにJupyter Notebook Handlerをコンパイルタイムプラグインとして組み込む」ためのもののようです。  
あまり意味がわからないままやっています。


## （６）hugo.goを実行
下記のコマンドを実行し、`hugo.go`ファイルのスクリプトを実行します。  
今後、普段記事をアップするときに`hugo`コマンドの代わりに以下を実行したのち、必要なコマンドを実行していけばOKなのかなと思います

```bash
go run hugo.go server -w
```

[TODO]現状うまくアップできていないので、要チェック
このコマンド実行時にエラーを確認しました。こんな感じでズラーっと並んでいます。

```
../../go/src/github.com/gohugoio/hugo/deploy/cloudfront.go:22:2: cannot find package "github.com/aws/aws-sdk-go/aws" in any of:
        /usr/local/Cellar/go/1.15.6/libexec/src/github.com/aws/aws-sdk-go/aws (from $GOROOT)
        /Users/m.ohsaki/go/src/github.com/aws/aws-sdk-go/aws (from $GOPATH)
../../go/src/github.com/gohugoio/hugo/deploy/cloudfront.go:23:2: cannot find package "github.com/aws/aws-sdk-go/aws/session" in any of:
        /usr/local/Cellar/go/1.15.6/libexec/src/github.com/aws/aws-sdk-go/aws/session (from $GOROOT)
        /Users/m.ohsaki/go/src/github.com/aws/aws-sdk-go/aws/session (from $GOPATH)
```

どうやら、うまくgithubからパッケージを見つけられていないようです。

もしかしたらこのライブラリ自体が3年前の更新で止まっているので、今は使えないのかもしれません。。。


# おわりに
<!-- これでipynbファイルがhugoブログに記事として投稿できるはずです。   -->
<!-- 僕がやったときは最初うまく更新されていなかったので、フロントマターを見直すとミスがありました（`]`の後にカンマがついていた）。コピペミスには注意です。 -->

---
## MEMO
【参考記事】
- [MacにGoをインストールする - Qiita](https://qiita.com/sunnyG/items/cabc700e6d9a28219cc8)
- [Jupyter NotebookをHugoのコンテンツとして使う方法 | kuune.org](https://kuune.org/text/2017/07/27/how-to-use-jupyter-notebook-as-hugo-content/)

【Jupyter Notebook Handler for Hugo 公式】
- [GitHub - naoina/hugo-jupyter-handler: Jupyter Notebook Handler for Hugo (Deprecated)](https://github.com/naoina/hugo-jupyter-handler#usage)
---