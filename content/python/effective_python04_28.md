---
title: "[Effective Python]項目28 内包表記では、3つ以上の式を避ける"
date: 2021-01-29T12:00
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
- **項目28 内包表記では、3つ以上の式を避ける**
- 項目29 代入式を使い内包表記での繰り返し作業をなくす  
- 項目30 リストを返さずにジェネレータを返すことを考える  
- 項目31 引数に対してイテレータを使うときには確実さを優先する  
- 項目32 大きなリスト内包表記にはジェネレータ式を考える  
- 項目33 yield fromで複数のジェネレータを作る  
- 項目34 sendでジェネレータにデータを注入するのは避ける  
- 項目35 ジェネレータでthrowによる状態遷移を起こすのは避ける  
- 項目36 イテレータとジェネレータの作業ではitertoolsを使う  



## 項目28 内包表記では、3つ以上の式を避ける
[項目27](https://massasquash.github.io/potatofolio/python/effective_python04_27/)で見た内包表記は`for`や`if`を繰り返して多重ループや複数の条件を使うこともできます。便利な反面、あまり多用しすぎると読むのが難しく可読性が下がってしまい、内包表記の「短く書ける」という長所も生かされなくなってしまいます。  
目安として**内包表記では３つ以上の式を使うことを避ける**、つまり
- ２つの条件
- ２つのループ
- １つの条件と１つのループ
までにして、それよりも複雑になる場合は通常の`if`文や`for`文を使って書くのが良い、というのがこの項目の内容です。

---
## MEMO
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---