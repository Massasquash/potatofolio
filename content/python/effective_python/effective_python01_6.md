---
title: "[Effective Python]06 タプルとアンパック構文"
date: 2021-01-01T01:06
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**１章 Pythonic思考**の項目6について学びました。


## 項目6 インデックスではなく複数代入アンパックを使う
Pythonの組み込み型のタプルは、値の変更不可能なシーケンスを作る型です。このタプルを使った**アンパック代入**という便利な書き方があり、１つの代入式で複数の値に代入することができます。  
この構文を使うと見た目がすっきりとして行数も少なくなり、Pythonicなコードになります。

この構文の便利な例として、例えば値の交換（スワップ）に不要な一時変数を定義する必要がなくシンプルに書くことができる、というのがあります。  
また組み込み関数`enumerate()`関数を活用する際にも便利な構文です（項目7で詳しく見ていきます）。

## 解説

### （１）リストとタプル
どちらも、
- インデックスによる要素のアクセスができる
- スライスによる要素の切り出しができる

という**シーケンス**型なのですが、大きな違いはリストが要素の追加・削除が後からできる（**ミュータブル**）のに対して、タプルはそれができず定義した時点から不変（**イミュータブル**）ということです。  
タプルを使うと最初に定義した値を変更することができないため、変更されたくない値を扱う際により安全に変数を扱うことができます。  

参考までに、公式ドキュメントの表現を以下に引用させていただきます。

**リスト型 (list)**  
[組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#list)
>リストはミュータブルなシーケンスで、一般的に同種の項目の集まりを格納するために使われます (厳密な類似の度合いはアプリケーションによって異なる場合があります)。

**タプル型 (tuple)**  
[組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#tuples)
>タプルはイミュータブルなシーケンスで、一般的に異種のデータの集まり (組み込みの enumerate() で作られた 2-タプルなど) を格納するために使われます。タプルはまた、同種のデータのイミュータブルなシーケンスが必要な場合 (set インスタンスや dict インスタンスに保存できるようにするためなど) にも使われます。


### （２）アンパック構文
アンパック構文は、**「１つの代入式で複数の値に代入できる」**と言う構文です。  
リストもタプルも、このアンパック代入を使うことができます。
```python
# リストのアンパック代入
lst = [1, 2, 3]
a, b, c = lst
print(a, b, c)

# タプルのアンパック代入
tup = (1, 2, 3)
x, y, z = tup
print(x, y, z)

width, height = 210, 297
```

インデックスでリストやタプルにアクセスする場合よりも行数が少なくシンプルに書くことができます。  
例えば、Pythonではこのアンパック代入を使って値のスワップ（交換）が簡単にできる、と言うのが有名なようです。

```python
a = 1
b = 2

a, b = b, a

```

次回以降の項目で出てくる`enumerate()`関数, `zip()`関数でもこのアンパック構文を使うことでシンプルに書けるのが特徴です。

## 感想
タプルを使ったアンパック構文は僕もすごく好きです。おそらく初心者が一番最初に触れる「Pythonic」な書き方なのかな？と思います。積極的に使っていきたい構文です。


---
## MEMO
【参考記事】
- [組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#typesseq)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---