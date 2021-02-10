---
title: "[Effective Python]27-28 リスト内包表記について"
date: 2021-01-01T04:27
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**3章 関数**の項目27-28について学びました。


## 項目27 mapやfilterの代わりにリスト内包表記を使う
Pythonの**リスト内包表記**を使うと、他のシーケンスやイテラブルオブジェクトから簡単な記法でリストを作ることができます。 
内包表記を使うと通常の`for`文よりも実行速度が速い、というメリットもあります。  
リストだけでなく辞書、集合も内包表記で作ることができます。似たような表記で**ジェネレータ式**という記法もあります。

組み込みの`map()`関数を使うと同じような操作ができますが、ラムダ関数を直接引数に入れると少し回りくどい書き方になってしまいます。内包表記の方が短く明確な書き方になります。また、同じく組み込みの`filter()`関数を使って要素の抽出ができますが、内包表記だと同じような要素の抽出も簡単にできてしまいます。


## 項目28 内包表記では、3つ以上の式を避ける
内包表記は`for`や`if`を繰り返して多重ループや複数の条件を使うこともできます。便利な反面、あまり多用しすぎると読むのが難しく可読性が下がってしまい、内包表記の「短く書ける」という長所も生かされなくなってしまいます。  
目安として**内包表記では３つ以上の式を使うことを避ける**、つまり
- ２つの条件
- ２つのループ
- １つの条件と１つのループ
までにして、それよりも複雑になる場合は通常の`if`文や`for`文を使って書くのが良い、とのことです。


## 解説
### （１）内包表記
内包表記の基本的な使い方を整理します。  
リストを作るカッコ`[]`中で`for`, `if`を使い、繰り返しや抽出条件を表現することでアルゴリズム的にリストを作ることができます。  
数学の集合の表記（例：`集合A = {x | xは10以下の自然数}`）と感覚的に似たような書き方をするので、このような表記に慣れている人なら書き方をイメージしやすいかもしれません。

```python
numbers = [i for i in range(10)]

doubles = [i*2 for i in numbers]
evens = [i for i in numbers if i % 2 == 0]

print(numbers)
# -> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(doubles)
# -> [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
print(evens)
# -> [0, 2, 4, 6, 8]
```

通常のfor文と比較すると、リストに追加する際の`append()`をする必要がない分、動作が軽くなります。

```python
# リスト内包表記
numbers = [i for i in range(10)]

# 通常のfor文
numbers = []
for i in range(10):
  numbers.append(i)
```

リスト内包表記の他に、辞書内包表記やセット内包表記もあります。
```python
# 辞書内包表記
numbers_dict = {str(i): i for i in range(10)}

# セット内包表記
numbers_set = {i for i in range(10)}
```


### （２）map関数とfilter関数
組み込み関数の`map()`と`filter()`についてもここで整理しておきます。

まず基本的な使い方として、「リストの各要素を２倍した新しいリストを作りたい」「リストから偶数のみ抽出した新しいリストを作りたい」という想定で、`map()`と`filter()`を使って結果を見てみます。

```python
numbers = list(range(10))

doubles = map(lambda x: x*2, numbers)
evens = filter(lambda x: x % 2 == 0, numbers)

print(type(numbers), numbers)
# -> <class 'list'> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(type(doubles), list(doubles))
# -> <class 'map'> [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
print(type(evens), list(evens))
# -> <class 'filter'> [0, 2, 4, 6, 8]
```

`type()`関数の結果でわかる通り、これらの組み込み関数で返されるオブジェクトはリストではなくあくまで**イテラブルオブジェクト**なので、リストを作りたい場合は組み込み関数`list()`でリスト型に変換してあげる必要があります。

関数の詳細は以下のように整理できます。

| 関数 | 返り値 | 解説 |  
| :---: | :---: | :---: |  
| map(func, *iterables) | map object<br>（イテレータ） | イテラブルの全要素に対して関数を適用する | 
| filter(func or None, iterable)  | filter object<br>（イテレータ） | イテラブルの各要素に対して関数を適用して条件に一致する要素のみを抽出する |


`map()`関数は、
- イテラブルの全要素に対して同じ関数を適用し、その結果が入った**mapオブジェクト**というイテレータにして返す
- 一番シンプルな形は、第二引数に１つのイテラブル、第一引数の関数は１つの値を返す関数
- 第二引数以降に複数のイテラブルを渡すこともできて、その場合は第一引数の関数には同じ数の引数を受け取る関数を渡す

`filter()`関数は、
- イテラブルの各要素に関数を適用して、条件に当てはまる（`True`を返す）要素のみを絞り込んで**filterオブジェクト**というイテレータにして返す
- 第二引数にはイテラブルを１つだけ渡し、第一引数の関数には引数を１つだけとる関数を渡す
- 第一引数に`None`を渡すと、要素自体の真理値評価の結果を使ってフィルタリングを行ってくれる
（例えば数値型なら`0`が`False`、それ以外なら`True`）



## 感想
個人的に内包表記は大好きです。ちょっとしたものが簡単にかけてしまう記法なのでワンライナーで書けるのが良いのですが、面白いからと多用すると可読性が落ちてしまうという副作用があるので、気をつけたいところです。
関係する用語として **「イテレータ」「ジェネレータ」** についてはわかりづらく今後整理が必要そうです。


---
## MEMO
【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
- Python実践入門
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---