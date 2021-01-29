---
title: "[Effective Python]項目3 bytesとstrの違いを知っておく"
date: 2021-01-19
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
draft: true
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**１章 Pythonic思考**  
>Pythonコミュニティでは、特定のスタイルに沿ったコードを表すのにPythonicという形容詞を使います。Pythonのイディオムは、この言語を使い仲間と作業する経験から時間をかけて発展してきました。１章では、Pythonで最も中心的な共通的に使われる最良の方法を扱います。（まえがき xii より引用）

（目次）
- 項目1 使用するPythonのバージョンを知っておく
- 項目2 PEP8スタイルガイドに従う
- **項目3 bytesとstrの違いを知っておく**
- 項目4 Cスタイルフォーマット文字列とstr.formatは使わずf 文字列で埋め込む
- 項目5 複雑な式の代わりにヘルパー関数を書く
- 項目6 インデックスではなく複数代入アンパックを使う
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ



## 項目3 bytesとstrの違いを知っておく


## 解説
項目3にして少し込み入った話。ノンプログラマーは`str`だけ知っていれば使えると思う。


### （１）str型とbyte型のイメージ
str型は人が読み書きしやすい文字列、bytes型はコンピュータにとって扱いやすいバイト列を扱う。
相互に変換が可能。


### （２）エンコーディング







## 感想


---
## MEMO
【参考記事】
- [はじめに — pep8-ja 1.0 ドキュメント](https://pep8-ja.readthedocs.io/ja/latest/)
- [Python 標準ライブラリ — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/index.html)
- [Python で 1 行が 79 文字以内で、インデントがスペース 4 文字なのはなんで？ | 民主主義に乾杯](https://python.ms/pep7/#_1-%E8%A1%8C%E3%81%8B%E3%82%99-79-%E6%96%87%E5%AD%97%E4%BB%A5%E5%86%85%E3%81%A6%E3%82%99%E3%81%82%E3%82%8B%E7%90%86%E7%94%B1)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---