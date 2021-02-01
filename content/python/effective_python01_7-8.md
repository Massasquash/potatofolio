---
title: "[Effective Python]07-08 enumerate関数とzip関数"
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
`enumerate()`が　**「１つのイテラブルに対してインデックスと要素を取り出す」**関数と捉えると、`zip()`は　**「２つのイテラブルに対して各要素をそれぞれ取り出す」**関数と捉えられるのかな、と思います。  
`zip()`関数の特徴として、異なる長さのイテレータを扱う場合でもエラーにはならず、短いイテレータに合わせて処理が終了するため、場合によっては予期しない不具合が起きる可能性があります。もしこれを防いで一番長いイテレータに合わせて処理したい場合は、`zip()`関数を使う代わりに組み込みモジュール`itertools`の`zip_longest`関数というのもこの項目で紹介されています。

## 解説
`enumerate()`関数と`zip()`関数の基本的な使い方について、整理してきます。  
この中で(前回の項目)[https://massasquash.github.io/potatofolio/python/effective_python01_6/]で見てきたアンパック構文が活用されています。

### （１）range型とrange()関数

### （2）enumerate型とenumerate()関数

### （３）zip型とzip()関数

### （４）組み込みモジュールitertoolsについて


## 感想
forループと組み合わせるとシンプルに書けて便利な2つの関数について見てきました。アンパック構文と合わせて、個人的にも使いやすくて好きな構文です。


---
## MEMO
【参考記事】
- [組み込み型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/stdtypes.html#typesseq)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---