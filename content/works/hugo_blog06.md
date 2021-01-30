---
title: "[Hugoブログ06]ブログにipynbファイルをアップする"
date: 2021-01-15
lead: "Jupyter Notebookで作ったipynbファイルをそのままブログ記事にする"
categories:
  - "Hugoブログ"
---

# はじめに


## 概要
- ターミナル操作
  - （１）HomebrewでGoをインストールする
  - （２）Jupyter Notebook Handlerをインストールする

- ipynbファイルの設定
  - （３）frontmatterメタデータを設定する
  - （４）Hugoのコンテンツディレクトリにipynbファイルを保存する

- （５）Hugoプロジェクトディレクトリにhugo.goを作成

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

# おわりに


---
## MEMO
- [MacにGoをインストールする - Qiita](https://qiita.com/sunnyG/items/cabc700e6d9a28219cc8)
---