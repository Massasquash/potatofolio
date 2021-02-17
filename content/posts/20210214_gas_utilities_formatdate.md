---
title: "[GAS]日付をフォーマット指定で文字列変換するのに便利なクラスを作る"
date: 2021-02-17
lead: "Utilities.formatDate()とクラスの静的メソッドの活用"
categories:
  - "GAS"
draft: true
---


## はじめに
今回は、Date型を文字列に変換するために使える簡単なクラスの紹介です。  

ファイル名に日付文字列を入れたり、スプレッドシートに書き込むときにフォーマット指定した文字列を作りたいことが多々あると思います。  

GASには**Ultilitiesサービス**という便利ツールのようなものを提供してくれる機能があり、その中にある`formatDate()`というメソッドを活用することで、このような文字列変換ができるようになっています。  
さらにそれを自作クラスの中に書いてパーツ化することで使いやすくしてみたいと思います。  

今回の内容もGAS中級講座受講後に[先日の記事](https://massasquash.github.io/potatofolio/posts/20210211_gas_datetime_to_str/)を見てもらった際に先生に教えてもらった内容を題材にして、整理してみた内容になります。  
クラスといってもとてもシンプルなものなので、クラスの扱いに慣れていない場合でも使ってみるのにちょうど良い題材かと思います。

## 1.全体のスクリプトと使い方
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
このときデフォルトのフォーマット（`yyyy/MM/dd`）が自動で適用されており、月日はゼロパディングした文字列となっています。

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


**指定できる文字(一例)**  
以下に、フォーマットに指定できる文字の中でよく使いそうな分だけまとめておきました。  

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


## 2.スクリプトの解説
全体のスクリプトを見てみると、今回のクラスの中には`static`と頭についたメソッドが1つだけある状態です。  

```javascript
class MyFormatter {
  static dateToString(date, format=‘yyyy/MM/dd’) {
    return Utilities.formatDate(date, ‘JST’, format);
  }
}
```

構造は非常にシンプルですが、これでも立派なクラスになるのです。  

普段よく使う関数がもしあれば、このように`static`を頭につけて **staticメソッド（静的メソッド）** としてクラス内にいくつか定義しておくと、自作の便利機能集のようなものも作ることができます。  
（呼び出して使う際には`クラス名.メソッド名()`とクラス名を先頭につけて書くだけで、グローバル領域に書いた関数と似たように呼び出すことができます）


**デフォルトのフォーマットを変更したい場合**  
もしよく使う文字列のフォーマットがある場合は、全体のスクリプトの5行目の仮引数formatのデフォルト値を変更することでカスタマイズできます。  
例えば「21年1月1日 10時01分05秒」みたいな文字列に変換させて使う場合が多い場合は、5行目を以下のようにしてあげます。  
```javascript
...(省略)...
  static dateToString(date, format=‘yy年M月d日 hh時mm分ss秒’) {
...(省略)...
```

このようにクラスの中身をいじってあげることで、呼び出すときに毎回フォーマットを指定する必要がなくなるので、全体としてコードがスッキリします。


## 3.Utilities.formatDate()
GASが独自で提供している色々な便利機能を集めたUtilitiesというサービスがあります。  
[Class Utilities  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/reference/utilities/utilities#formatdatedate,-timezone,-format)

ドキュメントを見てみると、
> This service provides utilities for string encoding/decoding, date formatting, JSON manipulation, and other miscellaneous tasks.  

文字のエンコード・デコード、データフォーマット、JSONの操作、その他様々なタスクのためのユーティリティ（実用ツール）を提供するサービス、とあります。

今回はこの中の`formatDate()`というメソッドを使っています。  
日付型を書式指定で文字列に変換するのを簡単にできる機能が、GASではあらかじめ用意してくれているようです。  

`Utilities.formatDate()`の引数にDate型、タイムゾーン、フォーマットしたい文字列を入れて呼び出すことで、文字列が返ってきます。  
以下のコードでは、Date型を`toString()`で変換するのと、このフォーマット変換を使うのとで比較してみました。

```javascript
function sumpleFunction() {

  const date = new Date();
  console.log(data.toString())
  console.log(Utilities.formatDate(date, 'JST', 'yyMMdd'));

}
```

このように、自分で好きなフォーマットに指定して変換してくれるので、とても便利な機能です。


これだけでも便利なのですが、さらにこの`Utilities.formatDate()`の呼び出しを、関数やクラスのstaticメソッドとしてパーツ化しておくことで、毎回このような長いコードを書かなくても良くなります。  
1つのスクリプトに1回や2回ぐらいなら直接呼び出す書き方の方が良いですが、もし何度も使うなーと思ったら、冒頭のスクリプトのようにパーツ化しておくと便利なのかな、と思います。



## おわりに
「関数が１つの機能を持つもの」なのに対して、クラスは「複数の変数と機能を１つにまとめたもの」というような捉え方ができるかと思います。  
学習していると、どうしてもクラスという概念が身近に感じられないと思ってしまいます。
そんな時に「staticメソッドとしてよく使い回す関数を定義しておいて、便利ツールクラスを作ってみる」というのは、１つの良いアプローチかもしれないなあと思いました。
