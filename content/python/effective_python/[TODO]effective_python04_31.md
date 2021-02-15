---
title: "[Effective Python]31 引数に対してイテレータを使うときには確実さを優先する"
date: 2021-01-01T04:31
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
draft: true
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。
今回は**4章 内包表記とジェネレータ**の項目31の内容です。

## 項目31 引数に対してイテレータを使うときには確実さを優先する
「ジェネレータはステートフル」というのがキーワード。




## 解説

### ステートフル


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