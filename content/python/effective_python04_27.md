---
title: "[Effective Python]項目27 mapやfilterの代わりにリスト内包表記を使う"
date: 2021-01-26
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**4章 内包表記とジェネレータ**  
>Pythonには、リスト、辞書、集合の要素を簡単にイテレーションする特別な構文が用意されていて、派生的なデータ構造が作れます。また、列挙可能なストリームから値を取り出し、1つずつ返す関数も作れます。4章では、これらの機能を使い、性能向上、メモリ使用量削減、読みやすさの改善をどのようにして達成するかを述べます。（まえがき xii より引用）

（目次）  
- **項目27 mapやfilterの代わりにリスト内包表記を使う**
- 項目28 内包表記では、3つ以上の式を避ける  
- 項目29 代入式を使い内包表記での繰り返し作業をなくす  
- 項目30 リストを返さずにジェネレータを返すことを考える  
- 項目31 引数に対してイテレータを使うときには確実さを優先する  
- 項目32 大きなリスト内包表記にはジェネレータ式を考える  
- 項目33 yield fromで複数のジェネレータを作る  
- 項目34 sendでジェネレータにデータを注入するのは避ける  
- 項目35 ジェネレータでthrowによる状態遷移を起こすのは避ける  
- 項目36 イテレータとジェネレータの作業ではitertoolsを使う  



## 項目27 mapやfilterの代わりにリスト内包表記を使う
Pythonの**リスト内包表記**を使うと、他のシーケンスやイテラブルオブジェクトから簡単な記法でリストを作ることができます。 
内包表記を使うと通常の`for`文よりも実行速度が速い、というメリットもあります。  
リストだけでなく辞書、集合も内包表記で作ることができます。似たような表記で**ジェネレータ式**という記法もあります。

組み込みの`map()`関数を使うと同じような操作ができますが、ラムダ関数を直接引数に入れると少し回りくどい書き方になってしまいます。内包表記の方が短く明確な書き方になります。また、同じく組み込みの`filter()`関数を使って要素の抽出ができますが、内包表記だと同じような要素の抽出も簡単にできてしまいます。

## 解説
### （１）内包表記
内包表記の基本的な使い方を整理します。  
リストを作るカッコ`[]`中で`for`, `if`を使い、繰り返しや抽出条件を表現することでアルゴリズム的にリストを作ることができます。  
数学の集合の表記（例：`集合A = {x | xは10以下の自然数}`）と感覚的に似たような書き方をするので、数式に慣れていると書き方がすんなり入ってくる印象です。

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

通常のfor文と比較すると、リストに追加する際の`append()`をする必要がない分、動作が軽くなるようです。

```python
# リスト内包表記
numbers = [i for i in range(10)]

# 通常のfor文
numbers = []
for i in range(10):
  numbers.append(i)
```


### （２）map関数とfilter関数
組み込み関数の`map()`と`filter()`についてもここで整理しておきます。

基本的な使い方として、「リストの各要素を２倍した新しいリストを作りたい」「リストから偶数のみ抽出した新しいリストを作りたい」という想定で、`map()`と`filter()`を使って結果を見てみます。

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
関係する用語として **「イテレータ」 「ジェネレータ」**についてはわかりづらく理解が必要なところなので、今後整理していきます。


---
## MEMO
【参考書籍】
- [Python実践入門](https://www.amazon.co.jp/Python%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80-%E8%A8%80%E8%AA%9E%E3%81%AE%E5%8A%9B%E3%82%92%E5%BC%95%E3%81%8D%E5%87%BA%E3%81%97%E3%80%81%E9%96%8B%E7%99%BA%E5%8A%B9%E7%8E%87%E3%82%92%E9%AB%98%E3%82%81%E3%82%8B-WEB-PRESS-plus-ebook/dp/B0842JDVBZ)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---