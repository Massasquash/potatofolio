---
title: "[温度管理ツール01]T&D温度計デバイスを使って現在気温を取得する[GAS]"
date: 2021-02-19
lead: "おんどとりWebStorage APIを使ってみる"
categories:
  - "Works"
tags:
  - "GASツール制作"
---

## はじめに
T&Dの「おんどとり」という温度計デバイスから定期的にデータを取得して、毎日の温度と積算温度を可視化、温度が一定以下・以上になるとアラートを飛ばしてくれるような温度管理ツールをGASで制作しています。

当農場でたまたま使っていた温度計（データロガー）がAPIを提供しているようだったので、これを使って温度データを扱ってみることにしました。  
今回は、現在気温を取得してみたいと思います。

## 1.使用するデバイス
- T&D 温度測定データロガー「おんどとり」 TR-71wb

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B07NHYL461&linkId=7122ef05ade3a94e8f1379ede13586f6&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr">
</iframe>

このデバイスのデータはクラウドに蓄積されるため、Webやスマホアプリでの確認も可能です。  
（機種にもよるので注意が必要です）
- [おんどとり Web Storage ](https://ondotori.webstorage.jp/) 
- [T&D Thermo - Google Play のアプリ](https://play.google.com/store/apps/details?id=com.tandd.android.thermoweb&hl=ja&gl=US)
- [‎「T&D Thermo」をApp Storeで](https://apps.apple.com/jp/app/t-d-thermo/id703327096)

用意されている[APIドキュメント](https://ondotori.webstorage.jp/docs/api/)を見ていくと、今回活用する「TR-7wb/nw/wfシリーズ」の機体でできることは
- 現在値の取得
- 直近データの取得（最新300件）
- 指定期間・件数によるデータの取得

とあります。この中で、今回の記事では **「現在値の取得」** を使っていきます。


## 2.作りたいツールの概要
今回プログラムで実現したいのは、温度を把握しやすくすること、指定した日からの積算温度を取り出すこと、そして一定温度になったらアラートを飛ばしてくれるような仕組みを作れないかなと思い、試作してみました。  
取得した温度やアラートはLINEに通知が来るようにします。  
（※追記：アラートはスマホアプリを活用すると簡単に実現できそうなので、要件からは外すことにしました）

実際の温度管理作業については、春先（3月〜4月）の育苗・種子の催芽（種を植える前に高温多湿な環境を作って出芽させることで、発芽を揃えるための作業）をビニルハウスや倉庫の中で温度管理しながら行なっています。  

これまで温度の上げ下げのタイミングを温度計はつけているもののだいぶ感覚頼りにやっていたので、数値で測定できると良いなと思っています。  


## 3.現在の温度を取得するスクリプト
ドキュメントの以下のページにしたがって、APIを叩いてみます。  
[現在値の取得 | おんどとり WebStorage API](https://ondotori.webstorage.jp/docs/api/reference/devices_device.html)

コードを書く前に、いくつか確認しておく必要があります。
- デバイスの「シリアルナンバー」
- おんどとりWeb Storageの「ログインID」と「パスワード」
- Web Storageログイン後に取得できる「APIキー」（メニューの「アカウント管理」の「開発者向けAPI管理」より取得）

また、デバイスの電池が入っていて温度の記録が取れている状態かも確認しておきましょう。  

これらを準備できたら、GASで以下のスクリプトを書いていきます。

```javascript
// 必要な情報
const SERIAL = '<デバイスのシリアル>' ;
const LOGIN_ID = '<ログインID>';
const LOGIN_PASS = '<パスワード>';
const API_KEY = '<APIキー>';

function myFunction() {

  const uri = 'https://api.webstorage.jp/v1/devices/current';

  const headers = {
    'Content-Type':'application/json',
    'X-HTTP-Method-Override':'GET'
  }

  const body = {
      'api-key': API_KEY,
      'login-id': LOGIN_ID,
      'login-pass': LOGIN_PASS
    };

  const options = {
    method: "POST",
    headers: headers,
    payload: JSON.stringify(body),
    muteHttpExceptions: true
  };

  const response = UrlFetchApp.fetch(uri, options);
  console.log(response.getContentText());
  }
```

うまくこのスクリプトが実行できれば、得られるResponseオブジェクト`response`の文字列`response.getContentText()`は以下のようなJSON文字列になっているはずです。

```
<実行ログ>
{"devices":[{"num":"1","serial":"xxxxxxx","model":"TR-71wf","name":"TR-71wf",
"battery":"5","rssi":"","time_diff":"540","std_bias":"0","dst_bias":"60",
"unixtime":"1613745724","channel":[{"num":"1","name":"Ch.1","value":"-6.1","unit":"C"},
{"num":"2","name":"Ch.2","value":"Sensor Error","unit":"C"}],"baseunit":
{"serial":"52126FF8","model":"TR-71wf","name":"TR-71wf"},"group":{"num":"0",
"name":"GROUP1"}}]}
```

このオブジェクトから温度を取得するときは、以下のようにするとよさそうです。
```javascript
const sensorData = JSON.parse(response.getContentText());
console.log('ch1の現在温度: ', sensorData.devices[0].channel[0].value);
console.log('ch2の現在温度: ', sensorData.devices[0].channel[1].value);
```

```
<実行ログ>
ch1の現在温度: -6.1
ch2の現在温度: Sensor Error
```

それぞれのチャンネルから温度を取得できました。  
センサーをつないでいないチャンネルについては、Sensor Errorとなるようです。

## 4.ハマった点
APIにリクエストを送るために必要な情報（`UrlFetchApp.fetch()`の引数に入れる情報）は、スクリプトで定義した
- `uri`（リクエストURI）
- `header`（リクエストヘッダ）
- `body`（リクエストパラメータ・リクエストボディ）

の3つです。  
それぞれ[ドキュメント](https://ondotori.webstorage.jp/docs/api/reference/devices_device.html)に従いながら、`header`と`body`はjavascriptのオブジェクト形式で定義していきました。  

ちょっと詰まってしまったのが、`header`（リクエストヘッダ）のところです。  
ドキュメントだと`Host: api.webstorage.jp:443`が記載されていますが、実際にはこの`Host`を記述しているとエラーが発生してしまったことでした。  

```
<エラーログ>
Attribute provided with invalid value: Header:Host
```

「？？？」と思ってGASではなくPythonで`requests`モジュールを使って同じAPIを叩く操作を検証してみたところ、そちらは正常にデータを取得することができました。  
詳しい方に聞いてみたところ、おそらくGAS側の`UrlFetchApp`で何か制約があるのではないか、との意見をいただきました（本当のところはちょっと僕にはわかっておりません）。  

結果的に、今回記載したスクリプトのように、リクエストヘッダから`Host`の記述を省くことで動作させることができました。  

またこれはJavascriptの文法の話ですが、オブジェクトリテラルを書く際に、今回の例のようにプロパティ名にハイフンが入る場合は、`''`か`""`で囲ってやる必要があるので注意が必要です。

## おわりに
今回は、温度計デバイスから現在値を取得してみました。  
今後はLINEに現在気温を送ったり、積算温度を算出したりする実装をしていきたいと思います。