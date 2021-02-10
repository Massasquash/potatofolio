---
title: "[Effective Python]02 PEP8スタイルガイド"
date: 2021-01-01T01:02
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。 
今回は**１章 Pythonic思考**の項目2について学びました。


## 項目2 PEP8スタイルガイドに従う
Pythonのコードを書くときは[PEP8スタイルガイド](https://pep8-ja.readthedocs.io/ja/latest/)に従うと扱いやすく、読みやすいコードになります。このPEP8は**Pythonコードを書く際のベストプラクティスが記載されたスタイルガイド**で、美しいコードを書くための１つのルールが記載されています。

コミュニティや開発メンバーで行う場合は、このPEP8を土台として独自ルールなどは共通スタイルを共有しておくことで、協働作業が捗ります。  
自分だけが読むコードの場合でも、一貫したスタイルで書くことで後から修正しやすくなります。  
そういった規則が紹介されているのがこの項目の内容でした。

## 解説
PEP8には色々載っていますが、まずはじめに特に抑えておくと良さそうな項目をさらっと挙げてみます。　

### （１）空白
- インデントにはタブではなく4個のスペースを使う
- 各行は、長さを79文字までとする
- ファイルでは、関数とクラスは空白２行で分ける
- 変数代入の前後には、空白を１つ、必ず１つだけを置く
  （良い例 `hoge = 'huga'` ／悪い例 `hoge=     'huga'`）
- 辞書では、キーとコロンの間には空白を置かず、同じ行に値を書く場合には値の前に空白を１つ置く
  （良い例`{key: value}}` ／悪い例`{key:value}`, `{key :value}`）

１行の長さに制限があるのは「かつてエディタで80文字で折り返す際に改行されないようにするため」とのこと（MEMOの参考記事）。
長さ調整には（３）で出てくる「カッコの中身の改行テクニック」が役に立ちます。


### （２）変数名の付け方
- 関数・変数・属性は、小文字のスネークケース （例 `lowercase_underscore`）
- モジュールでの定数は、大文字のスネークケース （例 `ALL_CAPS`）

「モジュールでの定数」と言うのは、ファイルの頭の方のグローバルな部分に書いておく定数値（例えば共通で使いたいAPIの設定に関するものなど）のことかなと思います。


### （３）式と文
- 式の否定（`if not a is b`）ではなく、内側の項の否定（`if a is not b`）を使う
- コンテナやシーケンスの長さ（`if len(somelist) == 0`）を使って、空値（`[]`や`''`など）かどうかをチェックしない。`if not somelist`を使って、空値が暗黙にFalseと評価されることを使う
- 式が1行に治らない場合は、括弧でくくって複数行にして、読みやすいようにインデントする
- `\`で行分けするよりは、括弧を使って複数の指揮を囲む方が良い

カッコを使った改行についてはとても奥深いです。例えば、リストやタプルを途中で改行す場合は、以下のような改行の仕方ができます。  
以下は[PEP8スタイルガイド](https://pep8-ja.readthedocs.io/ja/latest/)に載っている例です。どちらの書き方でも可能ですが、プロジェクト内では統一すると良さそうです。

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]

my_list = [
    1, 2, 3,
    4, 5, 6,
]
```

このように`()`, `[]`などのカッコで囲まれた部分の改行は繋がっているものとして認識してくれるので、これを上手く活用することで自然に長いコードを改行することができます。

```python
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```


### （４）インポート
- import文は常にファイルの先頭におく
- インポートは、①標準ライブラリモジュール、②サードパーティモジュール、③自分の作成したモジュール、の順に行う。
- それぞれの間には一行空白を置いて、それぞれの部分ではアルファベット順にインポートする

基本的にはインストールなしで使えるのが標準ライブラリで、インストールして使うのがサードパーティーモジュール、というところでしょうか。  
例えば`sys`, `os`, `datetime`などが標準ライブラリに当たります。標準ライブラリについては[公式ドキュメント](https://docs.python.org/ja/3/library/index.html)で確認することができます。


## 感想
上で挙げたのはごく一例。[PEP8スタイルガイド](https://pep8-ja.readthedocs.io/ja/latest/)を読むともっと興味深いコーディング規約が載っています。  
VSCodeを使っている場合はPEP8準拠の拡張機能を使ってみるのもコードを自動で整えられて便利です。

プログラミング学習においては、基本を学んでひとまず「正しく動くコードの書き方」を身に着けることができたら、次のステップとして「保守管理しやすいコードの書き方」が必要になってきます。  
その際に **「リーダブルコード」** がプログラミング全般における考え方を学べる書籍だとすると、「[PEP8スタイルガイド](https://pep8-ja.readthedocs.io/ja/latest/)」はリーダブルコードの考え方をPythonで実践する具体的な書き方、と言う感じかもしれません。

---
## MEMO
【参考ページ】
- [はじめに — pep8-ja 1.0 ドキュメント](https://pep8-ja.readthedocs.io/ja/latest/)
- [Python 標準ライブラリ — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/index.html)
- [Python で 1 行が 79 文字以内で、インデントがスペース 4 文字なのはなんで？ | 民主主義に乾杯](https://python.ms/pep7/#_1-%E8%A1%8C%E3%81%8B%E3%82%99-79-%E6%96%87%E5%AD%97%E4%BB%A5%E5%86%85%E3%81%A6%E3%82%99%E3%81%82%E3%82%8B%E7%90%86%E7%94%B1)

【書籍】
- [Effective Python 第2版 ―Pythonプログラムを改良する90項目](https://www.oreilly.co.jp/books/9784873119175/)
- リーダブルコード ―より良いコードを書くためのシンプルで実践的なテクニック
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873115655&linkId=8721a639c1e440dfa00e6d00a2f7b191&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr">
    </iframe>

---