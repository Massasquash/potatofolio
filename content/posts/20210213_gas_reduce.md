---
title: "[GAS]reduce()メソッドを活用して文字列をまとめて置換する"
date: 2021-02-14
lead: "配列のreduce()メソッドと仲良くなる"
categories:
  - "GAS"
---

## はじめに
今回は、配列の`reduce()`メソッドの理解を深める記事です。  
[先日の記事]((https://massasquash.github.io/potatofolio/posts/20210211_gas_datetime_to_str/#%E6%96%87%E5%AD%97%E5%88%97%E5%9E%8B%E3%81%AE%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89-replace-%E3%81%A8-padstart))をGAS中級講座受講後に先生に見てもらった時に、文字列型の`replace()`を使って文字列置換する時に`reduce()`を合わせて使えるよーというのを教えてもらってすごく面白かったです。  

こちらが教えてもらった内容です。  
[etau the non programmer coder  — GAS replace メソッドと reduce メソッド](https://etauthenonprogrammercoder.tumblr.com/post/637788437137719296/gas-replace-%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%A8-reduce-%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89)


## 1.今回のスクリプト
[先日の記事](https://massasquash.github.io/potatofolio/posts/20210211_gas_datetime_to_str/#%E6%96%87%E5%AD%97%E5%88%97%E5%9E%8B%E3%81%AE%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89-replace-%E3%81%A8-padstart)のスクリプトから枝葉の部分は省略して独立した関数にしました。  
日付のフォーマットを表す文字列、例えば「Y/M/D h:m:s」みたいな文字列を、それぞれDate型の年月日などの値に置き換える、という処理です。

```javascript {linenos=table}
function sampleFunction() {

	const date = new Date();
	const format = 'Y/M/D h:m:s';

  let replaceLists = [
  	// [置き換えたい文字列(正規表現), 置き換える値]
    [/Y/, date.getFullYear()],
    [/M/, date.getMonth() + 1],
    [/D/, date.getDate()],
    [/h/, date.getHours()],
    [/m/, date.getMinutes()],
    [/s/, date.getSeconds()]
  ];

  // replaceListsを使って置換
  const reducer = (acc, list) => acc.replace(...list);
  const formatStr = replaceLists.reduce(reducer, format);
  console.log(formatStr);
}
```
<br>

この17, 18行目のところで`reduce()`メソッドを使った置換を行っています。  
第1引数に置換した文字列を返すコールバック関数、第2引数に初期値として`format`の文字列を渡しています。  

普通なら文字列に`replace()`メソッドを適用させて順番に１つずつ置き換えていく必要があるところを、配列と組み合わせて短く書くことができました。

まとめて置換するために`replaceLists`という配列を先に用意しておいて、そこに「置き換えたい文字列(正規表現)」と「置き換える値」を配列で入れています。  

これによりどの文字を置き換えるのか対象がはっきりするし、今後また置換するものを増減させた場合にもこの配列だけをいじれば良いので、わかりやすくメンテナンス性が良さそうに思います。

この17, 18行目の置換の処理を１つ１つ書いてみたのがこちらのコードです。  
（[先日の記事](https://massasquash.github.io/potatofolio/posts/20210211_gas_datetime_to_str/#%E6%96%87%E5%AD%97%E5%88%97%E5%9E%8B%E3%81%AE%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89-replace-%E3%81%A8-padstart)のようにreplaceメソッドを繋げて書くこともできます）
```javascript
const formatStr;
formatStr = format.replace(/Y/, date.getFullYear())
formatStr = formatStr.replace(/M/, date.getMonth() + 1)
formatStr = formatStr.replace(/D/, date.getDate())
formatStr = formatStr.replace(/h/, date.getHours())
formatStr = formatStr.replace(/m/, date.getMinutes())
formatStr = formatStr.replace(/s/, date.getSeconds());
```

このコードだと処理手順が見えてわかりやすいですが、同じ変数とメソッドが重複してしまっています。  
また、置き換えたい文字列が増えた場合にはコピペして1行を書き足さなければいけないので、あまりスマートじゃなないなあと思ってしまいます。



## 2.reduce()メソッド
reduceは「減らす」という和訳の他に「変える」とか「まとめる」という意味もあるようなので、こちらの意味合いが近そうに思いました。  

`reduce()`メソッドは **「（初期値に対して）配列の各要素を順番に関数で処理していって最終的に1つの値を得る」** と言う感じかと思います。  
得られる値は配列ではなく「1つの値」になるのがポイントかなと思いました。

以下、リファレンスより引用。  
[Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)  

> *構文*    
> *arr.reduce(callback( accumulator, currentValue[, index[, array]] ) {*  
>  *// return result from executing something for accumulator or currentValue*  
> *}[, initialValue]);*  

> *解説*    
> *reduce() メソッドは、配列の各要素に対して (引数で与えられた) reducer 関数を実行して、単一の出力値を生成します。*  
>   
> *reducer 関数は 4 つの引数を取ります。*  
> *1. アキュムレーター (acc)*  
> *2. 現在値 (cur)*  
> *3. 現在の添字 (idx)*  
> *4. 元の配列 (src)*  
> *reducer 関数の返値はアキュムレーターに代入され、配列内の各反復に対してこの値を記憶します。最終的に単一の結果値になります。*  


構文と説明だけだと見るとかなり難しく感じますね。  
簡単なコードを書いて、このメソッドの動きを見てみます。  





### (1)初期値がない場合
配列に対して「全ての値を合算（+の演算）した結果を返す」というような例を考えます。  
まずは初期値がない場合に、数値と文字列でそれぞれ`reduce()`を使った結果がどうなるか見てみます。

```javascript
function sampleFunction() {

  // 例1
  const numbers = [1, 2, 3, 4, 5];
  console.log(numbers.reduce((acc, cur) => acc + cur))

  // 例2
  const chars = ['a', 'b', 'c', 'd', 'e'];
  const reducer = (acc, cur) => acc + cur;
  console.log(chars.reduce(reducer, 'XYZ');

}
```

```
実行ログ  
> 15
> abcde
```

配列の要素を頭から順番に足し合わせていった結果がログ出力されています。  

例2で使っている`reducer`はいわゆるコールバック関数。  
コールバック関数は、**配列内のそれぞれの要素について呼び出す関数** のことで、今回の`reduce()`の他`map()`, `filter()`などの反復メソッドを使う時にも登場します。


この`reducer`の仮引数で出てくる登場人物2つだけを基本として抑えておくようにします。

- **acc**（accumulator、蓄積者とか累算器みたいな意味）：  
  これまでの処理の返り値を蓄積しているもの

- **cur**（currentValue、現在値と言う意味）：  
  今処理している配列の値

配列の要素を順番に`cur`として取り出しながら、これまでの処理結果`acc`に処理を加えて結果を次に渡していく、という感じかなと思います。  
なんとなく、伝言ゲームのように順番に次に伝えていく感じをイメージしました。


### (2)初期値がある場合
先ほどのスクリプトとほぼ同じですが、`reduce()`メソッドの引数に、コールバック関数の他に第2引数として初期値を入れることができます（5, 10行目のハイライト行）。  
今度は「初期値に全ての値を合算（+の演算）した結果を返す」というような処理になります。

```javascript {hl_lines=[5, 10]}
function sampleFunction() {

  // 例1
  const numbers = [1, 2, 3, 4, 5];
  console.log(numbers.reduce((acc, cur) => acc + cur, 100));

  // 例2
  const chars = ['a', 'b', 'c', 'd', 'e'];
  const reducer = (acc, cur) => acc + cur;
  console.log(chars.reduce(reducer, 'XYZ'));

}
```

```
実行ログ  
> 115
> XYZabcde
```

最初の`acc`にその初期値が入った状態から処理がスタートする、と言う感じでしょうか。  

今回の冒頭のスクリプトのような次々に置換していく処理は、この初期値として文字列フォーマットを入れておいてそれに配列と`reduce()`を使って`replace()`を適用していくことで実現できています。

## おわりに
`reduce()`の活用についてはこちらのブログも参考にさせていただきました。色々な活用例が載っていてわかりやすかったです。  
[【JavaScript基礎】Array.prototype.reduce() をしっかり理解する＆サンプル集 - KDE BLOG](https://kde.hateblo.jp/entry/2018/10/13/065738)

またGASでのDate型の文字列フォーマットについてはクラスと`Utilities.formatDate()`を使うやり方を教えてもらったので、改めて整理してみます。