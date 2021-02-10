---
title: "[Effective Python]09 for, whileループの後のelseブロック"
date: 2021-01-01T01:9
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**１章 Pythonic思考**の項目9について学びました。


## 項目9 forループとwhileループの後のelseブロックは使わない
Pythonでは他の言語と違って、`for`や`while`のループブロックの直後に`else`ブロックを書くことができる構文があります（通常`else`は`if`の条件分岐の際に使う）。  
このループの後の`else`ブロックは、ループ本体で`break`文が実行されなかったときのみに実行される、という構文です。  
このような書き方はできるのですが、`if/else`文で使うときと意味合いが違います。誤解を意味やすいので使用しないようにしましょう、というのがこの項目の内容でした。

## 感想
こんな構文があるんだ...という印象でした。  
特殊な構文を紹介されたものの、結論として「出来るだけ使わない方が良い」どころ「使うべきではない」とバッサリ切られて不思議な感覚です。


---
## MEMO
【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---