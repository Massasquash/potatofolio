---
title: "[Effective Python]項目6 インデックスではなく複数代入アンパックを使う"
date: 2021-01-23
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
draft : true
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
Pythonの組み込み型のtupleは、値の変更不可能なシーケンスを作る型。listとの違いは、ミュータブルかイミュータブルか、というところです。
このtupleを使った「アンパック構文」というのがあり、１つの代入式で複数の値に代入でき、この構文を使うと見た目がすっきりして、行数が少なくなります。この構文はインデックスを使用しないので、より明快でよりPythonicなコードになります。

このアンパック構文を利用した組み込み関数`enumerate()`関数の活用が、次の項目7で出てきます。

スワップがシンプルにできるのがポイントです。

## 項目7 rangeではなくenumerateを使う


## 項目8 イテレータを並列に処理するにはzipを使う

## 解説



## 感想



---
## MEMO
【参考記事】
- [Pythonの辞書のgetメソッドでキーから値を取得（存在しないキーでもOK） | note.nkmk.me](https://note.nkmk.me/python-dict-get/)
- [Pythonの論理演算子and, or, not（論理積、論理和、否定） | note.nkmk.me](https://note.nkmk.me/python-boolean-operation/)
- [6. 式 (expression) — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/reference/expressions.html#boolean-operations)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---