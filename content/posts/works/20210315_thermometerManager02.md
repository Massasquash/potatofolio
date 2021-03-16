---
title: "[温度管理ツール02]APIに関する情報と処理をオブジェクト・クラスにまとめる[GAS]"
date: 2021-03-15
lead: "前回のコードのリファクタリング、センサーデータを取得・保持するためのクラス作成"
categories:
  - "Works"
tags:
  - "GASツール制作"
images: ["img/header/20210315.png"]
---

## はじめに
[前回の記事](https://massasquash.github.io/potatofolio/posts/works/20210219_thermometermanager01/)で、GASを使って**おんどとり WebStorage API**にリクエストを送り、現在の温度の取得に成功しました。  

次のLINEに送ったり積算温度を算出したりという実装を進めていきたいところですが、その前に今後の実装とメンテナンスをしやすくするためにリファクタリングをしておくことにしました。  
APIを使うための情報と処理をメインの処理とは分離して、オブジェクトやクラスを活用して整理しておきたいと思います。

かなり蛇足的な記事になるかもしれませんが、クラスの学習も兼ねてコードを書いたのでそれついての記事となります。

## 1. スクリプトファイルを分ける
以下のようにGASのスクリプトファイルを分けてコードを整理することにしました。

### （１）認証に必要な秘匿情報を記述する credentials.gs
APIを活用するために必要な情報で外部に公開したくない秘匿情報をまとめておくファイルです。この中に必要な情報を、APIごとにオブジェクト形式で保管しておくことにしました。  
GASを使っているのでプロパティサービスを活用する方がより安全かと思いますが、手間なのでファイルにベタ書きしてしまいます。  
このファイルは間違えても外部に漏らさないように気をつけます。GitHubを利用する場合は`.gitignore`にこのファイル名を記述しておきます。

### （２）APIに関する情報と処理を記述する  apis.gs
APIに関する情報と処理をまとめておくファイルです。ここではヘッダー情報などはオブジェクトに、またAPIにリクエストを送る具体的な処理と取得したデータなどはクラスに定義しておくことにしました。  
これにより、メインのコードにはAPIに関係する処理を書く必要がなくなり、コードがすっきりします。処理した結果が欲しい場合は、ここで作成したクラスのインスタンスを作りメンバーを呼び出せば良いのです。  
またもし今後APIの仕様変更などがあった際には、メインのコードを触らずこのファイルの中身だけを書き換えれば良いので、メンテナンス性が楽になります。

### （３）メインの処理を記述する main.gs
メインの処理となるスクリプトを記述するためのスクリプトファイルです。この中身については今後の記事で詳しく見ていきたいと思います。  


## 2. 秘匿情報をオブジェクトにまとめる
`credentials.gs`に秘匿情報をまとめておきます。

`credentials.gs`というスクリプトファイルを作り、その中に今回の「おんどとり WebStorage API」で使用するキー（トークン）、ID、passwordなどの秘匿情報をオブジェクトとして入れておきます。
ここではグローバル領域に`CRED_THERMO_API`という定数を定義し、秘匿情報となる値を次のように入れました。  

```javascript
/*
T&D Thermo おんどとりWebStoraage
*/
const CRED_THERMO_API = {
  apiKey: '<APIキー>',
  loginId: '<ログインID>',
  loginPass: '<パスワード>',
  serial: '<デバイスのシリアル>'
}

```

この値を呼び出す際には、`CRED_THERMO_API.apiKey`などと**定数名.プロパティ名**としてその値を取り出すことができます。

今後「LINE Messaging API」など他のサービスのAPIも活用することを考えているので、サービスごとに必要な情報をまとめたオブジェクトを作っておくようなイメージになります。


## 3. APIの情報をオブジェクトにまとめる
次に`apis.gs`というスクリプトファイルを作り、ここにAPIに関係する情報や処理を記述していきます。

APIドキュメントを参考に、リクエストURIやヘッダー情報などのAPIにリクエストを送るために必要な情報をオブジェクトとして入れておきます。  
ここではグローバル領域に`APIINFO_THERMO`という定数を定義し、その中にさらに`current`という「現在値の取得」をするためのAPIの情報をオブジェクトでまとめました。

```javascript
/*
T&D Thermo おんどとりWebStoraage
使用機種　TR-71W（TR-7シリーズ）
APIドキュメント -> https://ondotori.webstorage.jp/docs/api/
*/
const APIINFO_THERMO = {
  /*
  現在値の取得
  https://ondotori.webstorage.jp/docs/api/reference/devices_device.html
  */
  current:{
    uri: 'https://api.webstorage.jp/v1/devices/current',
    headers: `{
      "Content-Type":"application/json",
      "X-HTTP-Method-Override":"GET"
    }`
  }
}
```

こうすることで、例えば`APIINFO_THERMO.current.headers`としてやれば現在値を取得するAPIを叩くためのヘッダー情報を取り出すことができるようになります。

わざわざ辞書の入れ子構造にしたのは、この「おんどとり WebStorage API」サービスが複数のAPIを提供しているためです。  
今回は現在地の取得を行うためのAPIしか使わないのであまり恩恵はないのですが、今後必要に応じてAPIの情報を追加していくことが容易にできます。  
また、ヘッダー情報をオブジェクトでなくJSON形式の文字列リテラルで書いたのも理由がありますが詳細はここでは割愛します（LINE Messaging APIを活用する際にその方が記述しやすかったため）。

ここでオブジェクトに入れた情報は次に定義するクラスの中でのみ使用するようにして、メインスクリプトでは使用しないようにすることで役割を分担させるように心がけます。


## 4. AIP関連の処理とデータをクラスにまとめる
`apis.gs`のスクリプトファイルに続けて記述します。  

ここでクラスを活用することにしました。APIにリクエストを送る処理などは全てこのクラスの中にメソッドとして記述するようにして、そのレスポンスオブジェクトや必要なデータを取り出しやすいようにプロパティに持たせるようにしました。  

なぜわざわざクラス化するのか？というところを先に整理しておくと、
- メインとなるスクリプトのコード量を減らすため
- APIにアクセスする処理などをこのクラス内で完結させることでメンテナンス性を上げるため

という2点です。

まずは以下がクラス全体のスクリプトになります。
```javascript
class SensorData{
  /**
   * @param {Object} data - センサーから取得したレスポンスオブジェクト
   * @param {Number or String} temp_ch1 - チャンネル1の温度(dataより)
   * @param {Number or String} temp_ch2 - チャンネル2の温度(dataより)
   */
  constructor(){
    this.data = this.getSensorData();

    this.temp_ch1 = this.data.devices[0].channel[0].value;
    this.temp_ch2 = this.data.devices[0].channel[1].value;
  }

  /**
   * APIを叩いてセンサー情報を取得する
   * @returns {Object} センサーから取得したレスポンスオブジェクト
   */
  getSensorData() {
    const uri = APIINFO_THERMO.current.uri;
    const headers = APIINFO_THERMO.current.headers;
    const body = {
        'api-key': CRED_THERMO_API.apiKey,
        'login-id': CRED_THERMO_API.loginId,
        'login-pass': CRED_THERMO_API.loginPass
      };
    const options = {
      method: "POST",
      headers: JSON.parse(headers),
      payload: JSON.stringify(body),
      muteHttpExceptions: true
    };

    const response = UrlFetchApp.fetch(uri, options);

    return JSON.parse(response.getContentText());
  }

}
```

**このクラスを作成する際のポイント**は、
- **APIを叩いたレスポンスデータを持つプロパティを作っておく**
- **その処理はメソッドとして持たせて、返り値にレスポンスデータを返すようにする**
というところかな、と思います。

ざっくりですがクラスの中身を見ていきます。  
まず`SensorData`というクラスを定義し、コンストラクタ`constructor()`でこの`SensorData`オブジェクトに持たせたいプロパティを定義します。ここでは、

- レスポンスを格納する`data`オブジェクト
- その`data`オブジェクトの中で特によく使う温度データ `temp_ch1`, `temp_ch2`

というプロパティを持たせておくことにしました。  
この`data`プロパティがポイントの1つ。これにAPIを叩いて取得したレスポンスオブジェクトをそのまま格納しておくというイメージで、その具体的な処理は`getSensorData()`メソッドに記述しています。

ポイント2つ目の`getSensorData()`メソッドの中身は[前回の記事](https://massasquash.github.io/potatofolio/posts/works/20210219_thermometermanager01/)で書いた現在温度を取得する処理と変わりがないのですが、一部変数名を修正しています（先ほど必要な情報をオブジェクトに格納したためそれに変更しています）。
そして返り値に`UrlFetchApp.fetch()`でAPIを叩いて得られたレスポンスデータを指定しているので、この値がコンストラクタ起動時にプロパティに格納されるようになっています。


## 5.メインスクリプトでのこのクラスの使い方
上記のようにクラス化したことで、メインスクリプトでセンサーから取得したデータを簡単にオブジェクトとして生成することができるようになります。  
また必要な情報に`オブジェクト名.プロパティ名`を使って、短いコードでアクセスできるようにもなりました。  

実際のコードについては次回以降に見ていきますが、ここではクラスがどう使われるかの確認をします。  
`main.gs`スクリプトファイルに以下のように書いて、先ほど整理したクラスを呼び出して見ることにします。

```javascript
function mainFunction(){
  const sensorData = new SensorData();

	console.log('sensorData', sensorData)
	console.log('sensorData.data', sensorData.data)
	console.log('ch1の現在温度: ', sensorData.temp_ch1)
	console.log('ch2の現在温度: ', sensorData.temp_ch2)
}
```

```
<実行ログ>
sensorData { 
  data: { devices: [ [Object] ] },
  temp_ch1: '16.2',
  temp_ch2: '14.5' }

sensorData.data { devices: 
   [ { num: '1',
       serial: '52126FF8',
       model: 'TR-71wf',
       name: 'TR-71wf',
       battery: '5',
       rssi: '',
       time_diff: '540',
       std_bias: '0',
       dst_bias: '60',
       unixtime: '1615792332',
       channel: [Object],
       baseunit: [Object],
       group: [Object] } ] }

ch1の現在温度:  16.2

ch2の現在温度:  14.5
```

`const sensorData = new SensorData();`の１行で内部的にAPIを叩く処理が行われるため、メインのスクリプトでこのAPI周りの処理を書く必要がなくなりました。  
また、レスポンスオブジェクトから温度データを取り出すのもとても簡単なコードで書けるようになりました。これもクラスの中でオブジェクトのプロパティとしてその値を保持するようにしたためです。


## おわりに
必要な情報をオブジェクトにまとめ、APIで取得する情報と処理をクラスにまとめておくことで、コードがすっきりしてとても使い勝手が良くなるのを実感できました。  

今回は実装は進めずコードを抽象化するだけの記事となってしまいました。次回はLINE Messaging APIを使ってLINEにメッセージを送る処理について書いてみたいと思います。