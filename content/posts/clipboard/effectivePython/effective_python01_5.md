---
title: "[Effective Python]05 条件分岐(論理式・三項式)とヘルパー関数の活用"
date: 2021-01-01T01:05
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**１章 Pythonic思考**の項目5について学びました。


## 項目5 複雑な式の代わりにヘルパー関数を書く
Pythonの構文はシンプルで簡潔に書けるがために、かえって複雑で読みにくい１行の式が書けてしまいます。  
例えば論理演算子`or`や`and`を使うとコードは短くなりますがわかりづらいことが多くなります。そんな時に、`if/else`条件式を使った方が長くなるものの読みやすい式が書けます。  
さらに、繰り返し出てくる処理はそれを **ヘルパー関数**として別の関数に切り分けておくと便利です。一般的に、再利用しやすいように便利な機能を切り出した関数をヘルパー関数と呼ぶようです。

プログラミングには **DRYの原則** というものがあります。 **"Don't Repeat Yourself"「同じコードを繰り返すな」** と言われています。  
繰り返し似たようなコードを書いてしまうとメンテナンス性が落ちるので、繰り返し使う処理はまとめて関数を切り分けるというのが、可読性・保守性を上げるための１つのテクニックとして紹介されています。


## 解説
この項目は以下のような内容が主題かなと思います。

1. 条件式のあるロジックをどう書くと一番わかりやすく書けるのか？
2. 繰り返し使われるロジックを「ヘルパー関数」として別に切り分けるテクニック

読解するために少し引っかかりそうな構文について整理していきます。


### （１）dict.getメソッドについて
このメソッドは、一言で言うと`KeyError`対策。つまり **「辞書に存在しないキーを参照した場合にもエラーにならず何らかの値を返してくれるようにする」** というものです。

まず、dict（辞書）型について。リテラルは`values = {'red': 5, 'blue': 0, 'green': 0 }`のように波括弧を使って書き、キーとバリュー（値）をセットで扱うデータ型です（他の言語では「ハッシュ」「連想配列」などと呼ばれたりもします）。  
辞書型ではキーをインデックスとして利用することで、格納された値を取り出すことができます。

ここで辞書型に対する基本的な操作としては、

- キーによる要素の追加 `values['yellow'] = 1 `
- キーによる要素の参照 `values['red']`
- キーによる要素の削除　`del values['blue']`

という操作があります。このうち「要素の参照」の役に立つのが`get`メソッドです。

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

似たようなメソッドで`dict.setdefault()`というのがありますが、後の項目で出てくるのでその際に整理します。

### （２）論理式を使った条件分岐について
論理式を使って条件分岐を行う構文がさりげなく使われていますので、こちらも整理しておきます。  

論理演算子を使った式、例えば`x and y`や`x or y`はブール値`True/False`どちらかを返す、と言うのは基礎を学んだら直感的にわかりやすい使い方です。  
ところが、論理式を使う場合でも返り値がブール値でないこともあり、プログラミングに慣れていないと混乱してしまうポイントかなと思います。  
例えば`x`, `y` が数値だったり文字列だったりした場合に、`x and y`や`x or y`の値はブール値ではなく`x`か`y`のどちらかの値が返ります。特に、前に来る`x`が空文字列や`0`などの「`False`扱いされる値」の場合に、返ってくる値が色々と変わってくるのがポイントです。  

```python
# 例1
x = 'Bob'  # True扱いの文字列
y = 25
print(x and y)  # 25（後にあるyが出力）
print(x or y)  # Bob（前にあるxが出力）

# 例2
x = ''  # False扱いの空文字列
y = 25
print(x and y)  # （前にあるx（空文字列）が出力）
print(x or y)  # 25（後にあるyが出力）
```

というように、出力結果が`x`の値によって変わります。  
[公式ドキュメント](https://docs.python.org/ja/3/reference/expressions.html#boolean-operations)を見ると、

> 式 x and y は、まず x を評価します; x が偽なら x の値を返します; それ以外の場合には、 y の値を評価し、その結果を返します。  
> 式 x or y は、まず x を評価します; x が真なら x の値を返します; それ以外の場合には、 y の値を評価し、その結果を返します。  

式は左から順に処理をしていくので、左側の`x`を先に評価することで結論が出てしまい、右側の`y`が評価されない、と言うことが起こっています（**ショートサーキット（短絡評価）**）。  

この項目ではこの論理式の性質を活用して、ちょっとした条件分岐を作るテクニックがさりげなく使われています。  
結論としてはこの書き方はわかりづらい例として書かれています。


### （３）論理式とif/else条件式どれがわかりやすいか？
最後にこの項目の主題に関係する部分。  
「条件式のあるロジックをどう書くのが一番わかりやすいか？」という視点で、この章では３つの書き方が登場しています。  
[公式のサンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_05.py)でも確認することができます。  

1. 論理式で書く
2. ifを使った三項式で書く
3. 複数行のif/else文（基本形）で書く

結論としては、この3つ目の`if/else`文の基本形を使うパターンが、何を意味しているのかがわかりやすいコードになっています。  
この部分だけを見ると行数が長く感じてしまいますが、この処理をヘルパー関数として別の関数に切り分けることで、コードの長さは問題にならなくなりかえって可読性・保守性の上がるコードにすることができます。


## 感想
このケースでは　**基本的な書き方を守るのが一番可読性が高い**という結論になりました。  
基本を守ってわかりやすい式を書く・関数を切り分けると言うテクニックは、プログラミングの上でとても重要に思います。  
論理式を使った簡単な条件分岐は今回の例ではわかりにくい例として紹介されてしまっていましたが、本当にちょっとしたコードなら簡単に使えてワンライナーでかける便利なテクニックだな、と思も思いました  

関数を作る際には **「１つの関数に１つの機能」** というのを学んだのも思い出しました。このようなヘルパー関数の使い方をマスターしておきたいところです。


---
## MEMO
【参考ページ】
- [Pythonの辞書のgetメソッドでキーから値を取得（存在しないキーでもOK） | note.nkmk.me](https://note.nkmk.me/python-dict-get/)
- [Pythonの論理演算子and, or, not（論理積、論理和、否定） | note.nkmk.me](https://note.nkmk.me/python-boolean-operation/)

【ドキュメント】
- [6. 式 (expression) — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/reference/expressions.html#boolean-operations)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---