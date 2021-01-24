---
title: "[Effective Python]複雑な式の代わりにヘルパー関数を書く"
date: 2021-01-23
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**１章 Pythonic思考**  
>Pythonコミュニティでは、特定のスタイルに沿ったコードを表すのにPythonicという形容詞を使います。Pythonのイディオムは、この言語を使い仲間と作業する経験から時間をかけて発展してきました。１章では、Pythonで最も中心的な共通的に使われる最良の方法を扱います。（まえがき xii より引用）

（目次）
- 項目1 使用するPythonのバージョンを知っておく
- 項目2 PEP8スタイルガイドに従う
- 項目3 bytesとstrの違いを知っておく
- 項目4 Cスタイルフォーマット文字列とstr.formatは使わずf 文字列で埋め込む
- **項目5 複雑な式の代わりにヘルパー関数を書く**
- 項目6 インデックスではなく複数代入アンパックを使う
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ



## 複雑な式の代わりにヘルパー関数を書く
Pythonの構文はシンプルで簡潔に書けるがために、かえって複雑で読みにくい１行の式が書けてしまいます。  
例えば論理演算子`or`や`and`を使うとコードは短くなりますがわかりづらいことが多くなります。
そんな時に、`if/else`条件式を使った方が長くなるものの読みやすい式が書けます。  
さらに、繰り返し出てくる処理はそれを **ヘルパー関数** として別の関数に切り分けておくと便利です。

プログラミングにはDRYの原則というものがあります。 "Don't Repeat Yourself"
繰り返しコードを書かないように、関数を切り分けるというテクニックです。


## 解説
この章の本題は、

1. 「条件式のあるロジックをどう書くか？」
  - 簡単なロジックなら **論理式** を使うと短く書ける
  - `if`を使った**三項式（条件式）** を使うとややわかりやすくなる
  - 複数行の`if/else`文を使うと、長くなるが明確に書ける
2. 「繰り返し使われるロジックを、 **ヘルパー関数** として別に切り分ける」

というところです。
本題に入る前に、出てくる構文について２つほどチェックします。

### (1)dict.getメソッドについて
本題に入る前に、この章でさらっと出てくるdict型の`get`メソッドについてまず整理します。
このメソッドは、一言で言うと`KeyError`対策。つまり **「辞書に存在しないキーを参照した場合にもエラーにならず値を返してくれる」** と言うものです。

まず、dict型とはリテラルは`values = {'red': 5, 'blue': 0, 'green': 0 }`のように波括弧を使って書き、キーとバリュー（値）をセットで扱うデータ型です（他の言語では「ハッシュ」「連想配列」などと呼ばれたりもします）。  
dict型ではキーをインデックスとして利用することで、格納された値を取り出すことができます。

ここでdict型に対する基本的な操作としては、

- キーによる要素の追加（・削除） `values['yellow'] = 1 `
- キーによる要素の参照 `values['red']`

があります。このうち「要素の参照」の役に立つのが`get`メソッドです。

まず、キーが存在する要素を参照する場合と、キーが存在しない要素を参照しようとした場合は以下のようになります。

```python
values = {'red': 5, 'blue': 0, 'green': 0 }

print(values['red'])     # -> 5
print(values['opacity'])  # -> KeyError: 'opacity'
```

このように、キーが存在しない要素を参照しようとすると`KeyError`が発生してしまいます。
そこで、`get`メソッドを使うと、以下のようになります。

```python
print(values.get('red'))     # -> 5
print(values.get('red', 0))  # -> 5

print(values.get('opacity'))     # -> None
print(values.get('opacity', 0))  # -> 0
```

このように、キーが存在している場合は参照した値を返し、キーが存在していない場合はエラーにならず別の値を返してくれるようになります。
第一引数にキーの名前、第二引数にはキーがない場合に返したい値を入れることができます。

### (2)論理式について
もう１つ、論理式を使って条件分岐を行うと言うテクニックがさりげなく使われていますので、こちらも整理しておきます。
ここで使われているのは「**ショートサーキット（短絡評価）**」と言うテクニックです。

`x and y`や`x or y`がブール値`True/False`をどちらかを返す、と言うのは自然な使い方ですが、この返り値がブール値にならないケースがありいます。

[TODO]
https://note.nkmk.me/python-boolean-operation/
```python

```


式は左から順に処理をしていくので、左側の`x`を先に評価することで右側の`y`が評価されない、と言うことが起こります（これをショートサーキット（短絡評価）と呼んでいます）。
`and`, `or`を使った論理式のどちらか一方を返すという結果になります。

これを活用すると、ちょっとした値を分岐して返したい場合にささっと短く書くことができます。


### （３）論理式とif/else条件式どれが良いか？
次に「条件式のあるロジックをどう書くか？」と言う部分で、３つの書き方を並べてみました。
[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_05.py)を参照させてもらいます。

```python
from urllib.parse import parse_qs

my_values = parse_qs('red=5&blue=0&green=',
                     keep_blank_values=True)
print(repr(my_values))  # -> {'red': ['5'], 'blue': ['0'], 'green': ['']}


# 1.論理式で書く
opacity = my_values.get('opacity', [''])[0] or 0
print(f'Opacity: {opacity!r}')  # -> Opacity: 0

# 2.ifを使った三項式で書く
opacity_str = my_values.get('opacity', [''])
opacity = int(opacity_str[0]) if opacity_str[0] else 0
print(f'Opacity: {opacity!r}')  # -> Opacity: 0

# 3.複数行のif/else文で書く
opacity_str = my_values.get('opacity', [''])
if opacity_str[0]:
    opacity = int(opacity_str[0])
else:
    opacity = 0
print(f'Opacity: {opacity!r}')  # -> Opacity: 0
```

結論としては、この3つ目の複数行で書くパターンが一番長いものの、何を意味しているのかがわかりやすいコードになっています。
最後にこの関数を


## 感想
このケースでは「基本的な書き方を守るのが一番可読性が高い」と言うことになりました。
基本を守ってわかりやすい式を書く・関数を切り分けると言うテクニックは、プログラミングの上でとても大事な考え方だなと思いました。


---
## MEMO
【参考記事】
- [Pythonの辞書のgetメソッドでキーから値を取得（存在しないキーでもOK） | note.nkmk.me](https://note.nkmk.me/python-dict-get/)
- [Pythonの論理演算子and, or, not（論理積、論理和、否定） | note.nkmk.me](https://note.nkmk.me/python-boolean-operation/)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---