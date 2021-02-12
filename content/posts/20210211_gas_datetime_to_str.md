---
title: "[GAS]日付型を簡単なフォーマットで文字列変換する関数"
date: 2021-02-11
lead: "日付を文字列に変換したい場合に使うreplace()とpadStart()メソッド"
categories:
  - "GAS"
tags:
  - "業務効率化"
---

## はじめに
日付オブジェクトをちょっとした文字列に変換して使いたい…などというケースに使える関数です。  

現在受講中のノンプロ研 GAS講座の予習をしていたらゼロ埋めのテクニックが出てきて、まさに「これこれ！」と思うようなかゆいところに手の届く感じでした。
これまで泥臭いコードでなんとなく対応していたのですが、これを機に整理してみたので紹介します。  

今回の関数でできることはシンプルです。

- Date型のオブジェクトと文字列フォーマットを渡すと、それに沿った文字列を返してくれる（例： Y-M-D を渡すと 2021-1-1 に変換）
- Y, M, D, h, m, sという文字が使える
- `zeroPadding`をつけると月以下はゼロ埋めして返してくれる（例： 2021/01/01 10:05:00）


## 全体のスクリプト
```javascript {linenos=table}
function datetimeToStr3_(date, format, zeroPadding=false) {

  let arr = [
    date.getFullYear(),   //Y
    date.getMonth() + 1,  //M
    date.getDate(),       //D
    date.getHours(),      //h
    date.getMinutes(),    //m
    date.getSeconds()     //s
  ];

  if (zeroPadding === true) {
    arr = arr.map(value => String(value).padStart(2, '0'));
  }

  const formatStr = format.replace(/Y/, arr[0])
                    .replace(/M/, arr[1])
                    .replace(/D/, arr[2])
                    .replace(/h/, arr[3])
                    .replace(/m/, arr[4])
                    .replace(/s/, arr[5]);

  return formatStr;
}
```
<br>

使うときは、次のコードの4, 5行目のように使います。  
基本的には`datetimeToStr_()`関数に第1引数として日付型、第2引数としてフォーマットしたい文字列を入れてやります。  
この`testFunction()`を走らせてみると、  

```javascript
function testFunction(){

  const date = new Date();
  const str1 = datetimeToStr_(date, 'Y-M-D');
  const str2 = datetimeToStr_(date, 'Y/M/D h:m:s', zeroPadding=true);

  console.log('Y-M-D でフォーマット -> ', str1);
  console.log('Y/M/D h:m:s でフォーマット -> ', str2);
}
```
<br>

```
実行ログ
> Y-M-D でフォーマット ->  2021-02-12  
> Y/M/D h:m:s でフォーマット ->  2021/02/12 09:03:50  
```



このように、日付型が整形されて文字列として返ってきます。
月以下をゼロ埋め2桁で使いたい場合は、`zeroPadding=true`を引数として渡してあげます（仮引数名をつけずに`true`だけでもOKです）。  
第3引数は省略しても大丈夫で、その場合は1桁の数値はそのまま使うことになります。  

スプレッドシートのセルに年月日と時間を分けて入れたり、保存するファイル名の先頭に日付をちょっと加工して挿入したい場合に手軽に使えるかなと思います。


## スクリプトの補足
全体のスクリプトの27〜29行目で書いた部分が、ゼロ埋めするための処理です。

```javascript
  if (zeroPadding === true) {
    arr = arr.map(value => String(value).padStart(2, '0'));
  }
```
<br>

あまり長く書きたくないので、配列の`map()`メソッドを活用してごにょごにょやってます。  
配列の`map()`メソッドを使って一気に変換してしまっていますが、わかりやすいように要素ごとに書くと、以下のコードに置き換えられるかと思います。

```javascript
  if (zeroPadding = true) {
    // arr[0] = String(arr[0]).padStart(2, '0'); //年は4桁あるのでそのまま出力される
    arr[1] = String(arr[1]).padStart(2, '0'); //月を2桁で出力
    arr[2] = String(arr[2]).padStart(2, '0'); //日
    arr[3] = String(arr[3]).padStart(2, '0'); //時間
    arr[4] = String(arr[4]).padStart(2, '0'); //分
    arr[5] = String(arr[5]).padStart(2, '0'); //秒
  }
```
<br>

**年を4桁ではなく2桁表示にしたい場合**  
例えば「2021」を「21」と表示させたいケースも多いのかなと思いました。  
ちょっとだけカスタマイズをして簡単に動かせれば良いよという場合は、全体のスクリプトの16行目を以下のようにしてやるといけそうです。  

```javascript {hl_lines=[1]}
const formatStr = format.replace(/Y/, String(arr[0]).slice(2))
..(省略)..
```
<br>

## 文字列型のメソッド replace() と padStart()
今回主に活躍しているメソッドはこちらの2つ。

- (1)[String.prototype.replace() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
- (2)[String.prototype.padStart() - JavaScript | MDN](~https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/padStart~)

### (1)String.replace()  
> *構文*
> *str.replace(regexp|substr, newSubstr|function)*

> *解説*
> *replace() メソッドは、pattern にマッチした文字列の一部またはすべてを replacement で置き換えた新しい文字列を返します。pattern は文字列または RegExp、replacement は文字列または各マッチで呼び出される関数です。pattern が文字列の場合、最初に一致した箇所のみを置き換えます。*
>
> *元の文字列は変更されません。*

文字列の中から特定の文字列を探して置換する時に便利なメソッドです。  
第1引数には**正規表現**という文字列パターンを表すオブジェクトを使用しています（`/Y/`などスラッシュで囲まれているのは正規表現のリテラルを表しています）。  
今回は、例えば「Y」という文字列に一致するものを探して別の文字に置き換えています。  

```javascript
const str = 'Y/M/D';
const year = 2021;
console.log(str.replace(/Y/, year));
```
```
実行ログ
> 2021/M/D
```



### (2)String.padStart()  
> *構文*  
> *str.padStart(targetLength [, padString])*  

> *解説*
> *padStart() メソッドは、結果の文字列が指定した長さになるように、現在の文字列を他の文字列で (必要に応じて繰り返して) 延長します。延長は、現在の文字列の先頭から適用されます。*

メソッドの第1引数には長さを数値で、第2引数には埋めたい文字列を指定してあげます。

このメソッドを使うと数値の0埋めが簡単にできてしまいますが、数値と文字列の型の違いに注意が必要です。  
第2引数に文字列でなく数値を渡してしまうと「TypeError: month.padStart is not a function 」つまり「（数値型には）padStartなんていうメソッドがないよ」とエラーが出て怒られてしまいます。
数値は文字列型として変換してやると、うまく動作しました。

```javascript
function testFunction(){
  const number = String(1);  // String()オブジェクトに変換する書き方
  // const number = '1';  // 文字列リテラルでの書き方もOK
  console.log(number.padStart(2, 0));
}
```

```
実行ログ
> 01
```

## おわりに
Javascriptは日付周りが弱く、`moment.js`ライブラリも非推奨になってしまった…という嘆きをよく耳にします。  
僕は`moment.js`を使ったことがなかったのですが、Pythonと比べても日付周りはすごく複雑に感じます。みなさんどうやって対応しているんだろう。

今回の関数もおそらく`zeroPadding`という引数をとるのではなく、「Y」「YY」「YYYY」みたいに受け取ったのをその桁数で返す、とした方が使いやすいのかなーと思います。  
今回そこまでも必要ないなと思っていたのでシンプルな関数にしましたが、汎用性を高めるなら`moment.js`の仕様を勉強してみる必要があるんだろうなあと思いました。