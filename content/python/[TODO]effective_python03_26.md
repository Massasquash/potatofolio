---
title: "[Effective Python]項目26 functools.wrapsを使って関数デコレータを定義する"
date: 2020-03-25
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**3章 関数**  
>Pythonの関数には、プログラマを助けるためのさまざまな機能があります。一部は、他のプログラミング言語にもある機能ですが、多くはPythonに特有のものです。3章では、関数をどのように使えば、プログラムの意図が明確になり、再利用を促進し、バグを減らせるかを示します。（まえがき xii より引用）

（目次）
- 項目19 複数の戻り値では、4個以上の変数なら決してアンパックしない
- 項目20 Noneを返すのではなく例外を送出する
- 項目21 クロージャが変数スコープとどう関わるかを把握しておく
- 項目22 可変長位置引数を使って、見た目をすっきりさせる
- 項目23 キーワード引数にオプションの振る舞いを与える
- 項目24 動的なデフォルト引数を指定するときにはNoneとdocstringを使う
- 項目25 キーワード専用引数と位置専用引数で明確さを高める
- **項目26 functools.wrapsを使って関数デコレータを定義する**



## 項目26 functools.wrapsを使って関数デコレータを定義する
Pythonの「デコレータ」構文を使うと、関数実行時にその関数の動きに装飾を加えることができる。






## 解説

### デコレータ




---
## MEMO
【参考書籍】
- [Python実践入門](https://www.amazon.co.jp/Python%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80-%E8%A8%80%E8%AA%9E%E3%81%AE%E5%8A%9B%E3%82%92%E5%BC%95%E3%81%8D%E5%87%BA%E3%81%97%E3%80%81%E9%96%8B%E7%99%BA%E5%8A%B9%E7%8E%87%E3%82%92%E9%AB%98%E3%82%81%E3%82%8B-WEB-PRESS-plus-ebook/dp/B0842JDVBZ)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---