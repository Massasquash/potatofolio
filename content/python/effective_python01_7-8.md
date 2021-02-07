---
title: "[Effective Python]07-08 enumerate関数とzip関数"
date: 2021-01-01T01:07
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
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
- 項目5 複雑な式の代わりにヘルパー関数を書く
- 項目6 インデックスではなく複数代入アンパックを使う
- **項目7 rangeではなくenumerateを使う**
- **項目8 イテレータを並列に処理するにはzipを使う**
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ



## 項目7 rangeではなくenumerateを使う
pythonで`for`ループを回すときは連番や１つ飛ばしなどのシーケンス型を簡単に作れる組み込み関数`range()`が便利ですが、`enumerate()`関数を使うことで要素と要素のインデックスを同時に取り出すことができるようになります。  
ループの中でシーケンスのインデックスが必要になるときは、`range()`を使うよりも`enumerate()`を使う方がシンプルに書くことができる、というのがこの項目の内容でした。

## 項目8 イテレータを並列に処理するにはzipを使う
forループにおいて`enumerate()`と同様に便利でよく使われる組み込み関数として`zip()`というのもあります。  
`enumerate()`が　**「１つのイテラブルに対してインデックスと要素を取り出す」**関数と捉えると、`zip()`は　**「２つ以上のイテラブルに対して各要素をそれぞれ取り出す」**関数と捉えられるのかな、と思います。  

`zip()`関数の特徴として、異なる長さのイテレータを扱う場合でもエラーにはならず、短いイテレータに合わせて処理が終了するため、場合によっては予期しない不具合が起きる可能性があります。  
もしこれを防いで一番長いイテレータに合わせて処理したい場合は、`zip()`関数を使う代わりに組み込みモジュール`itertools`の`zip_longest`関数を使えば良い、というのもこの項目で紹介されています。

<!--
## 解説
`enumerate()`関数と`zip()`関数について、整理してきます。  
この中で(前回の項目)[https://massasquash.github.io/potatofolio/python/effective_python01_6/]で見てきたアンパック構文が活用されています。

### （2）enumerate()関数
enumerateとは「列挙する」「数え上げる」のような意味の単語のようです。  
`enumerate()`関数に例えばリストなどのイテラブルを与えてやると、その各要素のインデックスと値をセットで取り出して返してくれる関数です。  

書籍の中で、  
>enumerateは、遅延評価ジェネレータでイテレータをラップします。  
>enumerateは、ループのインデックスとイテレータの次の値の対をyieldします。

というような記述があります。出てくる用語がわかりづらいので、わかりやすくするために逆から整理して見ます。

**イテレータとは**
要素を反復して取り出すことのできるもの。  
内部構造的には、イテレータに当たるものは次の特殊メソッドを持っているクラスのオブジェクトがイテレータに当たります。

| 特殊メソッド | 役割 |
| :--- | :--- |
| __iter__() | イテレータオブジェクトを作るときに働く |
| __next__() | 次の値を取り出すときに働く |

似たような用語に「イテラブル」というものがありますが、こちらは次の特殊メソッドを持っているクラスのオブジェクトのことです。

| 特殊メソッド | 役割 |
| :--- | :--- |
| __iter__() | イテレータオブジェクトを作るときに働く |
| __getitem__() | 値に`[]`でアクセスしたときに働く |


こちらの記事を参考にさせていただいています。  
[Pythonのイテレータとイテラブルとは  |  Hbk project](https://hibiki-press.tech/python/iterable_iterator/1567#toc6)

**ジェネレータとは**

１要素を取り出そうとするたびに処理を行って
「ジェネレータイテレータ」のことを単に「ジェネレータ」と呼ぶこともあるそうです。

`yield`で

**遅延評価とは**


なかなか理解が難しいです。  
次に、`enumerate()`関数を使うとどんな動きをするのか実験してみます。

```python
values = ['a', 'b', 'c']
enum = enumerate(values)

# enumerateという型
print(enum)  # -> <enumerate object at 0x110dda900>

# 組み込み関数list()でインデックスと値のペアのタプルのりすとが作れる
print(list(enum)) # -> [(0, 'a'), (1, 'b'), (2, 'c')]

# forループの中でよく使われる
for i, v in enumerate(values):

```

次に、こんなのを試してみます。

```python
values = ['a', 'b', 'c']
enum = enumerate(values)

print(next(enum)) # -> (0, 'a')
print(next(enum)) # -> (1, 'b')
print(next(enum)) # -> (2, 'c')
print(next(enum)) # -> StopIteration エラー
```

ここで出てくる組み込み関数`next()`で次の値を取り出せるのは **「ジェネレータイテレータ」**というものの性質です。  
ジェネレータは内部では特殊メソッド`__next__()`が実装されていおり、またこのジェネレータを生成する関数には`return`で返り値を返す代わりに`yield`式が使われています。


**enumerate(iterable, start=0)**
[組み込み関数 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/functions.html#enumerate)より引用。
>enumerate オブジェクトを返します。 iterable は、シーケンスか iterator か、あるいはイテレーションをサポートするその他のオブジェクトでなければなりません。 enumerate() によって返されたイテレータの __next__() メソッドは、 (デフォルトでは 0 となる start からの) カウントと、 iterable 上のイテレーションによって得られた値を含むタプルを返します。

ドキュメントに乗っている例で、組み込み関数`enumerate()`関数は次と等価です。
```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```


### （３）zip()関数
**zip(*iterables)**
>それぞれのイテラブルから要素を集めたイテレータを作ります。
>この関数はタプルのイテレータを返し、その i 番目のタプルは引数シーケンスまたはイテラブルそれぞれの i 番目の要素を含みます。このイテレータは、入力イテラブルの中で最短のものが尽きたときに止まります。単一のイテラブル引数が与えられたときは、1 要素のタプルからなるイテレータを返します。引数がなければ、空のイテレータを返します。

### （４）組み込みモジュールitertoolsについて
-->



---
## MEMO
【参考記事】
- [組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html)
- [Pythonのイテレータとイテラブルとは  |  Hbk project](https://hibiki-press.tech/python/iterable_iterator/1567#toc6)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---