---
title: "[Effective Python]項目29 代入式を使い内包表記での繰り返し作業をなくす"
date: 2021-01-01T04:29
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
draft: true
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**4章 内包表記とジェネレータ**  
>Pythonには、リスト、辞書、集合の要素を簡単にイテレーションする特別な構文が用意されていて、派生的なデータ構造が作れます。また、列挙可能なストリームから値を取り出し、1つずつ返す関数も作れます。4章では、これらの機能を使い、性能向上、メモリ使用量削減、読みやすさの改善をどのようにして達成するかを述べます。（まえがき xii より引用）

（目次）  
- 項目27 mapやfilterの代わりにリスト内包表記を使う  
- 項目28 内包表記では、3つ以上の式を避ける
- **項目29 代入式を使い内包表記での繰り返し作業をなくす**
- 項目30 リストを返さずにジェネレータを返すことを考える  
- 項目31 引数に対してイテレータを使うときには確実さを優先する  
- 項目32 大きなリスト内包表記にはジェネレータ式を考える  
- 項目33 yield fromで複数のジェネレータを作る  
- 項目34 sendでジェネレータにデータを注入するのは避ける  
- 項目35 ジェネレータでthrowによる状態遷移を起こすのは避ける  
- 項目36 イテレータとジェネレータの作業ではitertoolsを使う  



## 項目29 代入式を使い内包表記での繰り返し作業をなくす


## 解説

- 辞書型のgetメソッドについては  
[Effective Python項目5 複雑な式の代わりにヘルパー関数を書く - Potatofolio](https://massasquash.github.io/potatofolio/python/effective_python01_5/#1%E6%A7%8B%E6%96%87dictget%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)  

- 代入式（ウォルラス演算子）については  
[Effective Python項目10 代入式で繰り返しを防ぐ - Potatofolio](https://massasquash.github.io/potatofolio/python/effective_python01_10/#%EF%BC%91%E4%BB%A3%E5%85%A5%E5%BC%8F%E3%81%AE%E5%9F%BA%E6%9C%AC%E6%A7%8B%E6%96%87walrus%E6%BC%94%E7%AE%97%E5%AD%90)

---
## MEMO
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---