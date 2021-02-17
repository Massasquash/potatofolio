---
title: "[GAS]日付をフォーマット指定で文字列変換するのに便利なクラス"
date: 2021-02-17
lead: "Utilities.formatDate()とクラスの静的メソッドの活用"
categories:
  - "GAS"
---


## はじめに
今回は、Date型を文字列に変換するために使える簡単なクラスの紹介です。  

ファイル名に日付文字列を入れたり、スプレッドシートに書き込むときにフォーマット指定した文字列を作りたいことが多々あると思います。  

GASには**Ultilitiesサービス**という便利ツールのようなものを提供してくれる機能があります。  
その中にある日付の文字列変換に便利なメソッドを活用しながら、さらにそれを自作クラスの中に書いてパーツ化することで使いやすくしてみた、というのが今回の内容になります。

今回の記事も、GAS中級講座受講後に[先日の記事](https://massasquash.github.io/potatofolio/posts/20210211_gas_datetime_to_str/)を見てもらった際に、先生に教えてもらった内容を整理した記事になります。  

## 1.全体のスクリプトと使い方
全体のスクリプトを眺めてみてから、細かい内容を見ていきたいと思います。  

`MyFormatter`という自作の便利機能をまとめられるようなクラスを想定して作ってみます。  
その中に、日付を文字列に変換する`dateToString()`というメソッドを、頭に`static`をつけて定義しています（この`static`については後述）。  

```javascript {linenos=table}
class MyFormatter {
/**
 * 日付を文字列にフォーマットする関数
 */
  static dateToString(date, format=‘yyyy/MM/dd’) {
    return Utilities.formatDate(date, ‘JST’, format);
  }
}
```
<br>

この機能を使うときは、以下のようにDate型を`MyFormatter.dateToString(date)`のように放り込んでやると、日付文字列を返してくれます。  
このときデフォルトのフォーマット（`yyyy/MM/dd`）が自動で適用されており、月日はゼロパディングした文字列となっています（MM, ddと2つつなげることで2桁表示を表しています）。

```javascript
function sampleFunction() {
  const today = new Date();
  console.log(MyFormatter.dateToString(today));
}
```

```
<実行ログ>
2021/02/16
```

使うときに自分でフォーマットを指定することもできます。  
`MyFormatter.dateToStr(date, format)`と第2引数にフォーマットとなる文字列を入れてやると、そのフォーマットに従った日付文字列が得られます。  

```javascript
function sampleFunction() {
  const today = new Date();
  console.log(MyFormatter.dateToString(today, 'yyMMdd'));
  console.log(MyFormatter.dateToString(today, 'y-M-d(E) h:m:s'));
  console.log(MyFormatter.dateToString(today, '現在時刻はkk:mmです。'));
}
```

```
<実行ログ>
210216
2021-2-16(Tue) 10:32:49
現在時刻は22:34です。
```

**デフォルトのフォーマットを変更したい場合**  
もしよく使う文字列のフォーマットがある場合は、全体のスクリプトの5行目の仮引数formatのデフォルト値を変更することでカスタマイズできます。  
例えば「21年1月1日 10時01分05秒」みたいな文字列に変換させて使う場合が多い場合は、5行目を以下のようにしてあげます。  
```javascript
...(省略)...
  static dateToString(date, format=‘yy年M月d日 hh時mm分ss秒’) {
...(省略)...
```

このようにクラスの中身を必要に応じていじってあげることで、呼び出すときに毎回フォーマットを指定する必要がなくなるので、全体としてコードがスッキリします。

**指定できる文字(一例)**  
以下に、フォーマットに指定できる文字の中でよく使いそうなものを表にしてみました。  

| 文字 | 意味 | 使用例 |
| :--- | :--- | :--- |
| y | 年 | yy, yyyy |
| M | 月 | M, MM|
| d | 日 | d, dd |
| k | 時間（24時間表記） | k, kk |
| h | 時間（12時間表記） | M, MM|
| m | 分 | m, mm |
| s | 秒 | s, ss |

月を表す場合に大文字（M）を使うのが少し紛らわしい感じがするので、注意が必要そうですね。  
詳しくは`Utilities.formatDate()`のドキュメントから一覧へのリンクがありますので、そちらをご覧いただけたらと思います。  

参考 [Class Utilities  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/reference/utilities/utilities#formatdatedate,-timezone,-format)



## 2.Utilities.formatDate()
ここで使っているUtilitiesサービスははGASが独自で提供している色々な便利機能を集めたサービスです。  

ドキュメントを見てみると、
> This service provides utilities for string encoding/decoding, date formatting, JSON manipulation, and other miscellaneous tasks.  

文字のエンコード・デコード、データフォーマット、JSONの操作、その他様々なタスクのためのユーティリティ（実用ツール）を提供するサービス、とあります。

今回はこのサービスが提供してくれる`Utilities`クラスの`formatDate()`というメソッドを使っています。  

`Utilities.formatDate()`の引数にDate型、タイムゾーン、フォーマットしたい文字列を入れて呼び出すことで、文字列が返ってきます。  
以下のコードでは、Date型を`toString()`で文字列に変換するのと、このフォーマット変換を使うのとで比較してみました。

```javascript
function sumpleFunction() {

  const date = new Date();
  console.log(date.toString())
  console.log(Utilities.formatDate(date, 'JST', 'yy/MM/dd'));

}
```

```
<実行ログ>
Wed Feb 17 2021 23:14:29 GMT+0900 (Japan Standard Time)  
21/02/17
```

このように、自分の好きなフォーマットに指定して変換してくれるとても便利な機能です。


## 3.クラスの静的メソッド（staticメソッド）
先ほどの`Utilities`を使った1文ですが、1つのスクリプトのなかで1回や2回ぐらいならこの書き方で問題ありません。  
ただもし何度も使っているなーと思ったら、それを関数なりクラスなりでパーツ化しておくと、コードが短くなり便利に使えるようになります。

そのパーツ化の1つのやり方が今回のクラス、と言えます。  

全体のスクリプトを見てみると、クラスの中には`static`と頭についたメソッドが1つだけある状態です。  
（これのようなメソッドを **静的メソッド（staticメソッド）** と呼びます）  

```javascript
class MyFormatter {
  static dateToString(date, format=‘yyyy/MM/dd’) {
    return Utilities.formatDate(date, ‘JST’, format);
  }
}
```

構造は非常にシンプルですが、これでも立派なクラスになるのですね。  

このように定義されたメソッドは、呼び出す時には`クラス名.メソッド名()`とクラス名を先頭につけて書くだけで、普通の関数と同じように呼び出すことができます。  
普段よく使ってるなーという関数がもしもあれば、このように`static`を頭につけて静的メソッドとしてクラス内にいくつか定義しておくと、自作の便利機能集をまとめたようなものも作ることができそうです。  

普通にいくつも関数を書いてしまうとバラバラだけど、その中の同じグループの関数を1つのクラスとして名前をつけてまとめておくことができる、というイメージで捉えました。

参考  [static - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes/static)


## おわりに
今回はGASが提供しているUltilitiesサービスを使って日付型のフォーマットを見てみました。  
また、それをクラスの静的メソッドとしてパーツ化して使いやすくする、というテクニックもまずは体感できたかな、と思います。  

むやみにクラスを使ってもコードが複雑になってしまってはいけません。ちょっとしたコードなら、関数を作れば十分だったりすることが多いように思います。  
クラスについては少しずつ使いながら理解して仲良くなっていければ良いなーと思います。  