---
title: "[Effective Python]項目10 代入式で繰り返しを防ぐ"
date: 2021-01-24
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
- 項目6 インデックスではなく複数代入アンパックを使う**
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- **項目10 代入式で繰り返しを防ぐ**



## 項目10 代入式で繰り返しを防ぐ
Python3.8より**代入式**という新たな構文（**walrus（セイウチ）演算子**`:=`を使う）が追加され、これによって「変数への代入（初期化）」と「変数の使用（評価）」を１つの式で行うことができるようになりました。この構文が追加される前（通常の変数代入と使用のやり方）は、例えば`if`文の条件式などで変数の評価をする際にはその前に変数を初期化しておく必要があったものが、それらをひとまとめにして条件式の中に書くことができ、無駄な行を省くことができるようになりました。

また、PythonにはC言語など他の言語と異なり「`switch/case`文がない」「`do/while`文がない」など制御構文を書く際に不便なことがありますが、これらが代入式を使うと簡潔に書くことができます。


## 解説

### （１）代入式の基本構文（walrus演算子）
基本的な使い方が[ドキュメント](https://docs.python.org/ja/3/reference/expressions.html#assignment-expressions)、またより詳しくは[PEP 572](https://www.python.org/dev/peps/pep-0572/)にてまとめられています。

PEP572より１つ、イメージのわきやすそうな代入式を使ったコードを見てみます。  
`input()`関数でユーザー入力を繰り返し受け付けてその内容を表示し、"quit"が入力されたら終了するコードです。  
参考として、代入式を使わないパターンも書いてみました。

```python
# 代入式を使って書いた場合
while (command := input("> ")) != "quit":
    print("You entered:", command)


# 代入式を使わないで書いた場合
command = ''
while command != "quit":
    command = input("> ")
    print("You entered:", command)
```

明らかに前者の方が簡潔に書けています。`command := input("> ")`の部分でwalrus演算子を使って、`command`という変数への代入と評価を１回で行っています。  

`if`条件式の中で代入して比較する場合は、カッコで括るのがポイントです。カッコがないと誤った値（式）が代入されてしまいます。

### （２）Pythonにはない制御構文
この項目の中で「Pythonを使い始めたプログラマが苛立つこと」として２点、Pythonには「`switch/case`文がないこと」「`do/while`文がないこと」が挙げられています。これらの構文の理解のためにフローチャートで整理しておきます。

**条件分岐：switch/case文**  
`if`文のような場合分けに使う構文ですが、通常の`if`文は２択の分岐を行う（`else if`のような文と組み合わせた場合は２択の分岐を繰り返す形になる）のに対して、`switch/case`文は式の値に応じてその値と合致する分岐を行います。

以下のようなフローチャートのイメージです。

<img src="/img/posts/20210125_if.png" width="300">
<img src="/img/posts/20210125_switch_case.png" width="300">

<br>
<br>

**繰り返し処理：do/while文**  
通常の`while`文と同じく繰り返しを行う構文だですが、通常の`while`文が「先にループ条件の判定を行ってから中身の処理を行う」のに対して、`do/while`文は「先に中身の処理を行ってから、最後にループ条件の判定を行う」という違いです。

以下のようなフローチャートのイメージです。

<img src="/img/posts/20210125_while.png" width="300">
<img src="/img/posts/20210125_do_while.png" width="300">

## 感想
walrus演算子`:=`はセイウチの横顔に見えるためこのような名前がついているようで、ちょっと面白いです。
またPythonには他の言語ではあるような制御構文が一部存在しないというのも興味深いです。こちら[pythonにswitch文がない経緯・理由 - gogochephy’s diary](http://gogochephy.hatenablog.com/entry/2015/07/02/135614)を参照させていただくと、公式ドキュメントにもしっかり`switch`や`case`文が無い理由が載っており、Pythonコミュニティでも議論がなされている話題のようです。


---
## MEMO
【参考記事】
- [6. 式 (expression) — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/reference/expressions.html#assignment-expressions)
- [PEP 572 — Assignment Expressions | Python.org](https://www.python.org/dev/peps/pep-0572/)

- [pythonにswitch文がない経緯・理由 - gogochephy’s diary](http://gogochephy.hatenablog.com/entry/2015/07/02/135614)



<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---