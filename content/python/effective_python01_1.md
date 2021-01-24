---
title: "[Effective Python]項目1 使用するPythonのバージョンを知っておく"
date: 2021-01-19
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**１章 Pythonic思考**  
>Pythonコミュニティでは、特定のスタイルに沿ったコードを表すのにPythonicという形容詞を使います。Pythonのイディオムは、この言語を使い仲間と作業する経験から時間をかけて発展してきました。１章では、Pythonで最も中心的な共通的に使われる最良の方法を扱います。（まえがき xii より引用）

（目次）
- **項目1 使用するPythonのバージョンを知っておく**
- 項目2 PEP 8スタイルガイドに従う
- 項目3 bytesとstrの違いを知っておく
- 項目4 Cスタイルフォーマット文字列とstr.formatは使わずf 文字列で埋め込む
- 項目5 複雑な式の代わりにヘルパー関数を書く
- 項目6 インデックスではなく複数代入アンパックを使う
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ


## 項目1 使用するPythonのバージョンを知っておく
Python3.8が2019年10月にリリースされてから、多くの構文が導入されました。
またPython2は2020年1月にサポートがなくなり、さらに2020年4月に使用期限が終わり公式な保守が行われなくなりました。
自己責任で使うことはできますが、Python2の利用は避け、Python3をプロジェクトに使っていきましょう。

## 解説
使用しているPythonのバージョンを調べる方法は
- （１）コマンドラインで調べる方法
- （２）Python実行時（Jupyter Notebookのセル上で）調べる方法


### （１）コマンドラインでバージョンを調べる方法
まずはターミナルで確認する方法。自分のインストールしているpythonのバージョンを調べられます。
`python`コマンドと`python3`コマンドでは結果が違い、どのPython言語をインストールしたかによって変わってきます。
（例えばPython3をインストールしていない場合は、Python3コマンドは動きません）。
```bash
$ python --version  #-> Python 2.7.10
$ python3 --version #-> Python 3.8.0
```

### （２）Python実行時にバージョンを調べる方法
次にPythonスクリプト上でバージョンを確認します。組み込み関数`sys`を使います。
Jupyter Notebook上などで確認する場合は、このようなやり方で確認できます。
```python
import sys
print(sys.version_info)
print(sys.version)
```

## 感想
プログラミング言語自体も、その実は誰かがプログラミングで作ったものです。
Pythonはオープンソースなので裏側でコミュニティの皆さんが保守管理してくれているため、僕らは安心してPythonでプログラミングを書くことができています。
より良くなるようにバグの改善・セキュリティ向上など改善を続けているので、しっかりと「サポートが受けられているバージョン」をプロジェクトに使っていくのが良さそうです。

---
## MEMO
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---

