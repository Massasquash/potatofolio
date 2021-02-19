---
title: "[温度管理ツール01][GAS]T&D温度計デバイスから現在気温を取得する"
date: 2021-02-19
lead: ""
categories:
  - "Works"
---

## はじめに
T&Dの「おんどとり」という温度計デバイスから定期的にデータを取得して、毎日の温度と積算温度を可視化、温度が一定以下・以上になるとアラートを飛ばしてくれるような温度管理ツールをGASで制作しています。

今回は、APIを叩いて現在気温を取得するのをやってみたいと思います。

## 1.作りたいツール
当農場でたまたま使っていたT&D Thermoの「おんどとり」という温度計（データロガー）がAPIを提供していたので、これを使って温度データを扱ってみることにしました。  

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B07NHYL461&linkId=7122ef05ade3a94e8f1379ede13586f6&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr">
</iframe>

この温度計はクラウド対応なので、取得した温度データはWebやスマホでも専用のアプリで見られます。  
ですが今回プログラムで実現したいのは、指定した日からの積算温度を取り出したり、一定温度になったらアラートを飛ばしてくれるような仕組みを作れないかなと思い、試作してみることにしました。  
取得した温度やアラートはLINEに通知が来るようにします。  

実際の温度管理作業ですが、春先（3月〜4月）の育苗・種子の催芽（種を植える前に高温多湿な環境を作って出芽させることで、発芽を揃えるための作業）をビニルハウスや倉庫の中で温度管理しながら行なっています。  

これまで温度の上げ下げのタイミングを温度計はつけているもののだいぶ感覚頼りにやっていたので、数値で判断できるようになると良いなと思っています。  

## 2.使用するデバイス
- T&D 温度測定データロガー「おんどとり」 TR-71wb

このデバイスのデータはクラウドに蓄積されるため、Webやスマホアプリでの確認も可能です。  
（機種にもよるので注意が必要です）
- [おんどとり Web Storage ](https://ondotori.webstorage.jp/) 
- [T&D Thermo - Google Play のアプリ](https://play.google.com/store/apps/details?id=com.tandd.android.thermoweb&hl=ja&gl=US)
- [‎「T&D Thermo」をApp Storeで](https://apps.apple.com/jp/app/t-d-thermo/id703327096)

用意されている[APIドキュメント](https://ondotori.webstorage.jp/docs/api/)を見ていくと、今回活用する「TR-7wb/nw/wfシリーズ」の機体でできることは
- 現在値の取得
- 直近データの取得（最新300件）
- 指定期間・件数によるデータの取得

とあります。この中で、今回の記事では **「現在値の取得」** を試してみたいと思います。


## 3.現在の温度を取得するスクリプト
ドキュメントの以下のページにしたがって、APIを叩いてみます。  
[現在値の取得 | おんどとり WebStorage API](https://ondotori.webstorage.jp/docs/api/reference/devices_device.html)

コードを書く前に、いくつか確認しておく必要があります。
- デバイスの「シリアルナンバー」
- おんどとりWeb Storageの「ログインID」と「パスワード」
- Web Storageログイン後に取得できる「APIキー」（メニューの「アカウント管理」の「開発者向けAPI管理」より）

また、デバイスの電池が入っていて温度の記録ができているかも確認しておきましょう。  

これらを準備できたら、GASで以下のスクリプトを書いていきます。

```javascript
// 必要な情報
const API_KEY = '<APIキー>';
const LOGIN_ID = '<ログインID>';
const LOGIN_PASS = '<パスワード>';
const SERIAL = '<デバイスのシリアル>' ;

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

うまくこのスクリプトが実行できれば、得られるResponseオブジェクトの文字列`response.getContentText()`は以下のような文字列になっているはずです。

```
<実行ログ>
{"devices":[{"num":"1","serial":"xxxxxxx","model":"TR-71wf","name":"TR-71wf","battery":"5","rssi":"","time_diff":"540","std_bias":"0","dst_bias":"60","unixtime":"1613745724","channel":[{"num":"1","name":"Ch.1","value":"-6.1","unit":"C"},{"num":"2","name":"Ch.2","value":"Sensor Error","unit":"C"}],"baseunit":{"serial":"52126FF8","model":"TR-71wf","name":"TR-71wf"},"group":{"num":"0","name":"GROUP1"}}]}
```


## 4.ハマった点
APIを叩くには、デバイス側が提供してくれているAPIのURLにリクエストを送る必要があります。

リクエストを送るために必要な情報は、`uri`（リクエストURI）, `header`（リクエストヘッダ）, `body`（リクエストパラメータ・リクエストボディ）です。

それぞれ[ドキュメント](https://ondotori.webstorage.jp/docs/api/reference/devices_device.html)に従いながら、`header`と`body`はjavascriptのオブジェクト形式で定義していきました。  

ちょっと詰まってしまったのが、`header`（リクエストヘッダ）のところです。  
ドキュメントだと`Host: api.webstorage.jp:443`が記載されていますが、実際にはこの`Host`のところでエラーが発生してしまったことでした。  

```
<エラーログ>
Attribute provided with invalid value: Header:Host
```

GASではなくPythonの`requests`モジュールを使って同じ操作を検証してみたところ、そちらは正常にデータを取得することができました。  
なのでおそらくGAS側のUrlFetchAppで何か制約があるのではないか、というところ（本当のところはちょっとわかっておりません）。
結果的に、今回記載したスクリプトのように、リクエストヘッダから`Host`を丸々無くして動作させることができました。  

またこれはJavascriptの書き方の話ですが、オブジェクトリテラルを書く際に、今回の例のようにプロパティ名にハイフンが入る場合は、`''`か`""`で囲ってやる必要があるので注意が必要です。

## おわりに
今回は、現在地を取得してみることを行いました。  

引き続き、この温度管理ツール制作の過程を紹介していきたいと思います。