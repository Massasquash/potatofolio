---
title: "[Effective Python]06 タプルとアンパック構文"
date: 2021-01-01T01:06
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
- 項目5 複雑な式の代わりにヘルパー関数を書く
- **項目6 インデックスではなく複数代入アンパックを使う**
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ



## 項目6 インデックスではなく複数代入アンパックを使う
Pythonの組み込み型のタプルは、値の変更不可能なシーケンスを作る型です。このタプルを使った**アンパック構文**という便利な書き方があり、１つの代入式で複数の値に代入することができます。  
この構文を使うと見た目がすっきりとして行数も少なくなり、Pythonicなコードになります。

この構文の便利な例として、例えば値の交換（スワップ）に不要な一時変数を定義する必要がなくシンプルに書くことができる、というのがあります。  
また組み込み関数`enumerate()`関数を活用する際にも便利な構文です（項目7で詳しく見ていきます）。

## 解説
Pythonの「タプル」という組み込み型と、そのタプルのアンパック構文についての解説です。

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


[組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#tuples)
>タプルはイミュータブルなシーケンスで、一般的に異種のデータの集まり (組み込みの enumerate() で作られた 2-タプルなど) を格納するために使われます。タプルはまた、同種のデータのイミュータブルなシーケンスが必要な場合 (set インスタンスや dict インスタンスに保存できるようにするためなど) にも使われます。


### （２）アンパック構文
アンパック構文は、**「１つの代入式で複数の値に代入できる」**と言う構文です。 
インデックスでタプルにアクセスする場合よりも行数が少なくシンプルに書くことができます。  
例えば、Pythonではこのアンパック構文を使って値のスワップ（交換）が簡単にできる、と言うのが有名なようです。

次回以降の項目で出てくる`enumerate()`関数, `zip()`関数でもこのアンパック構文を使うことでシンプルに書けるのが特徴です。

## 感想
タプルを使ったアンパック構文は僕もすごく好きで、おそらく初心者が一番最初に触れる「Pythonic」な書き方なのかな？と思います。  
積極的に使っていきたい構文です。


---
## MEMO
【参考記事】
- [組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#typesseq)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---