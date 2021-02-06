---
title: "[Effective Python]30,32 ジェネレータ"
date: 2021-01-01T04:30
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**4章 内包表記とジェネレータ**  
>Pythonには、リスト、辞書、集合の要素を簡単にイテレーションする特別な構文が用意されていて、派生的なデータ構造が作れます。また、列挙可能なストリームから値を取り出し、1つずつ返す関数も作れます。4章では、これらの機能を使い、性能向上、メモリ使用量削減、読みやすさの改善をどのようにして達成するかを述べます。（まえがき xii より引用）

（目次）  
- 項目27 mapやfilterの代わりにリスト内包表記を使う  
- 項目28 内包表記では、3つ以上の式を避ける
- 項目29 代入式を使い内包表記での繰り返し作業をなくす
- **項目30 リストを返さずにジェネレータを返すことを考える**  
- 項目31 引数に対してイテレータを使うときには確実さを優先する  
- **項目32 大きなリスト内包表記にはジェネレータ式を考える**
- 項目33 yield fromで複数のジェネレータを作る  
- 項目34 sendでジェネレータにデータを注入するのは避ける  
- 項目35 ジェネレータでthrowによる状態遷移を起こすのは避ける  
- 項目36 イテレータとジェネレータの作業ではitertoolsを使う  



## 項目30 リストを返さずにジェネレータを返すことを考える
**ジェネレータ**はリストと同じくfor文で利用できる**イテラブル**なオブジェクトですが、次のような違いがあります。

- リスト：全ての要素をメモリ上に保持するため、要素数が増えればメモリ使用量が増える
- ジェネレータ：次の要素が求められた時に次の要素が生成されて返されるため、要素数が増えてもメモリ消費が少なくて済む

この利点を利用して「リストで返す関数を作る時にはジェネレータを返す関数に置き換えてみよう」というのがこの項目の内容でした。
（このようなジェネレータを返す関数のことを**ジェネレータ関数**と呼びます。また、ジェネレータというのはイテレータの一種、と考えて良さそうです）

小規模なプログラムで少量のデータを扱うならリストで問題はないのですが、大規模なプログラムでリストを使うと要素数が大量になった場合にメモリクラッシュを引き起こしかねません。一方でジェネレータを使うと大量のデータを扱ってもメモリを一定しか必要としない、という利点があるようです。  

## 項目32 大きなリスト内包表記にはジェネレータ式を考える
項目30では「リストの代わりにジェネレータを使うと、大量の入力に対してもメモリ不足の心配がない」というお話でした。  
ジェネレータを作るためにジェネレータ関数を使っていましたが、もう１つの作り方として**ジェネレータ式**があります。  
これは[項目27](https://massasquash.github.io/potatofolio/python/effective_python04_27-28/)のリスト内包表記を使ってリストを作るのと似た構文で、内包表記を使ってジェネレータを作る構文です。  

また、ジェネレータ式から得られたジェネレータを他のジェネレータ式に渡すこともできて、そのように連鎖したジェネレータは非常に高速に動作する、とのことです。


## 解説
この項目では「英文の文字列中の単語の１文字目の位置のインデックスを求める関数」を作る例が題材になっています。  
結果をシーケンスで返す関数の実装として、インデックスが格納されたリストを返す関数と、同じくインデックスが格納されたジェネレータを返す関数のコードをそれぞれ比較してみてから、ジェネレータの仕組みについて整理してみます。

### （１）リストを返す関数とジェネレータを返す関数の比較
コードを並べて比較してみます。  
結論を言ってしまうと、ジェネレータを使う方が短く書くことができます。確かにリストを使う場合は`append()`メソッドを呼び出したりする手間が多いように見えます。  

[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_30.py)より引用させていただきます。

```python
# リストを返す関数
# Example 1
def index_words(text):
    result = []
    if text:
        result.append(0)
    for index, letter in enumerate(text):
        if letter == ' ':
            result.append(index + 1)
    return result

# Example 2
address = 'Four score and seven years ago...'
result = index_words(address)
print(result[:10])
```

結果
> [0, 5, 11, 15, 21, 27]

```python
# ジェネレータを返す関数
# Example 3
def index_words_iter(text):
    if text:
        yield 0
    for index, letter in enumerate(text):
        if letter == ' ':
            yield index + 1

# Example 4
address = 'Four score and seven years ago...'
it = index_words_iter(address)
print(next(it))
print(next(it))
```

結果
> 0
> 5

## （２）ジェネレータ
ここで出てくる「ジェネレータ」について整理してみます。  

**ジェネレータ**はリストと同じくfor文で利用できる**イテラブル**なオブジェクトです。  
リストとの違いは次の要素があらかじめ格納されているわけではなく、組み込み関数`next()`で呼び出された時に初めて要素が取り出されます。  
内部的には返り値の部分で`yield`式が使われていて、yieldは「生み出す」「算出する」のような意味を持ちます。

ジェネレータは要素の数に関係なく一定の大きさの箱（メモリ）と次の要素を取り出す仕組みだけが用意されていて、要素を１個１個取り出す時にその仕組みに従って要素を取り出す、というイメージかなと思います。  
数学がわかる方は、この仕組みは数列を作るときの「漸化式」のようにイメージできるかもしれません。  

例えばリストでは作ることのできない、無限に要素を持つリスト...のようなものもジェネレータを活用すると作ることができます。  
（これをリストで作ろうとすると、要素を入れるための箱（メモリ）が無限に必要になってしまいます）

ジェネレータを作るには「ジェネレータ関数」を使って作る方法と、「ジェネレータ式」を使って作る方法があります。

## （３）ジェネレータ関数とジェネレータ式
**ジェネレータ関数**
ジェネレータ関数とは内部で`yield`式を使う関数のことで、呼び出された際に実際の処理を行わずに**イテレータ**を返します。  
組み込み関数`next()`が呼ばれるごとに、イテレータはジェネレータを次の`yield`式に１つ進めます。ジェネレータによりyieldに渡される値は、それぞれイテレータによって呼び出し元に返されます。

実際のジェネレータ関数のコードの例を見てみます。  
組み込み関数`enumerate()`で返されるenumerateオブジェクトはジェネレータに当たるようなので、それを題材にしてみます。  
[ドキュメント](https://docs.python.org/ja/3/library/functions.html#enumerate)に内部的なコードが書かれているので、そちらを引用してみます。

```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```

`sequence`にリストのようなイテラブル（シーケンスかイテレータ）が与えられると、処理をしていって`yield`の値をイテレータ（ジェネレータイテレータ）として返します。  
通常値を返す関数では`return`で終わるところを、ループの処理の途中で`yield`で返しているのがポイントかな、と思います。  

実際にジェネレータを生成して、組み込み関数`next()`で要素を１つずつ取り出している様子です。

```python
gen = enumerate(range(10))
next(gen)  # -> (0, 'A')
next(gen)  # -> (1, 'B')
next(gen)  # -> (2, 'C')
next(gen)  # -> エラー「StopIteration」が発生して実行が止まる
```


**ジェネレータ式**
もう１つのジェネレータの作り方はこのジェネレータ式という内包表記を使う方法です。[リスト内包表記](https://massasquash.github.io/potatofolio/python/effective_python04_27-28/#%EF%BC%91%E5%86%85%E5%8C%85%E8%A1%A8%E8%A8%98)と似た書き方でジェネレータを生成できます。

```python
# 1〜10をそれぞれ2乗した値を生成するジェネレータ式
gen = (i**2 for i in range(10))
```

この式から作られたジェネレータも関数で生成された時と同様で、組み込み関数`next()`で次の値を取り出すことができます。

```python
gen = (i**2 for i in range(10))
next(gen)  # -> 0
next(gen)  # -> 1
next(gen)  # -> 4
```

試しにこのジェネレータ式と等価のジェネレータを作成するジェネレータ関数も書いてみました。

```python
def squares(max):
  i = 0
  for i in range(max):
    yield i**2
    i += 1
```



<!--
## （３）用語の整理
シーケンス、イテレータ・イテラブル、ジェネレータ

-->
## 感想
ジェネレータの特徴を理解すると、案外難しいものではなくすごく身近に感じられるものだなと思いました。実際に使ってみたくなります。

書籍でPythonの文法を体系的に学習していると「ジェネレータ」という謎のものが突然出てきて仕組みの解説が出てくる…という感じで、使い道がよくわからないなーという印象でした。  
この項目でなぞったようにまずリストには「課題」があってそれを解決するための「手段」としてジェネレータがある、と考えると、すごく腑に落ちました。  
ストーリーを学んでから再び仕組みを学ぶと理解が深まります。新しい概念を学ぶ時には、このようにいくつかの視点を行き来しながら学ぶことが重要なんだろうなー、と思いました。

---
## MEMO
【参考書籍】
- [Python実践入門](https://www.amazon.co.jp/Python%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80-%E8%A8%80%E8%AA%9E%E3%81%AE%E5%8A%9B%E3%82%92%E5%BC%95%E3%81%8D%E5%87%BA%E3%81%97%E3%80%81%E9%96%8B%E7%99%BA%E5%8A%B9%E7%8E%87%E3%82%92%E9%AB%98%E3%82%81%E3%82%8B-WEB-PRESS-plus-ebook/dp/B0842JDVBZ)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
  
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---