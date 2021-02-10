---
title: "[Effective Python]26 関数をラップするデコレータについて"
date: 2021-01-01T03:26
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**3章 関数**の項目26について学びました。

## 項目26 functools.wrapsを使って関数デコレータを定義する
Pythonの**デコレータ**構文を使うと、関数実行時にその関数の動きに装飾を加えることができます。  
しかし自分でデコレータを定義して実際に関数に適用してみると、表向きの動作には問題がないのですが、デコレータを適用した関数名が変わってしまい`help`関数や`docstring`が働かない、という副作用があります。  

これを解消するために、デコレータを定義する時には組み込みモジュール`functools`の`wraps`ヘルパー関数を使うと良い、とのことです。この`functools.wraps()`は「デコレータを書くのを助けるデコレータ」と言えます。

## 解説


### （１）デコレータ構文

### （２）デコレータが役に立つ例

## 感想
この項目は僕のようにデコレータそのものに馴染みがない人だと、わかりづらいところかもしれません。  
今回は **「デコレータの構文と使うと便利な例を知る」**こと、そして今後自分でデコレータを作ることになった際には **「活用するべき標準ライブラリがある」** ということを抑えておけばいいのかな、と思いました。

以前flaskアプリを制作した際に、ルーティングを定義するために以下のような構文がありました。

```python
@app.route('/')
def hello():
    return 'Hello World!'
```

ここの`@app.route('/')`でデコレータ構文が使われています。  
ひとまずはこのような`@`のついた構文が出てきても身構えず「その下に出てくる関数に対して何かしらの処理を追加している関数なんだな〜」と思えるようになれば良いのかな、と思います。

---
## MEMO
【参考記事】
- [Pythonのデコレータを理解するための12Step - Qiita](https://qiita.com/_rdtr/items/d3bc1a8d4b7eb375c368)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
- Python実践入門
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---