---
title: "[Python]プロセス実行・プログラム起動"
date: 2021-02-07T10:00
lead: "subprocessモジュールでのコマンド操作"
categories:
  - “Python”
---


## 1.コマンドを実行する
- Pythonプログラムを実行する

## 2.プロセスを実行する
- `subprocess`モジュール：Pythonからコマンドを実行するための標準モジュール
- 例えば毎日起動する定番のアプリをプログラムを実行することで一発で開いたりできるようになる

**規定のアプリでファイルを開く**  
- プログラム（プロセス）をオープンする関数  `subprocess.Popen()`を使う
- ここではGUI上で「ファイルをダブルクリック」した時と同じ動作をプログラムで実行するやり方を書いておく
- コマンドライン引数を渡したい場合は、リストの<ファイルパス>の後に要素を追加して渡すことができる
- OSによって起動方法や、渡すコマンド（`start`, `open`）が違うので注意

```python
import subprocess

# 直接ファイルを指定して「規定のアプリでファイルを開く」ことができる
proc = subprocess.Popen(['start', <ファイルのパス>], shell=True)  # Windows
proc = subprocess.Popen(['open', <ファイルのパス>])  # Mac
```

| OS | ファイルを開くためのコマンド | ファイルパスの調べ方 |  
| :--- | :--- | :--- |  
| Windows | start | エクスプローラで目的のファイルの上で Shift キー押しながら右クリック<br> -> 「パスのコピー」|  
| Mac | open | Finderで目的のファイルの上で右クリック<br> -> optキーを押しながら「パスのコピー」 |  


- `subproess.Popen()`関数の戻り値はPopenオブジェクト
	- `poll()`メソッド：起動したプログラムが実行しているかどうかをチェックする。プロセス実行していればNone, プログラムが終了していれば終了コードの整数値が返る
	- `wait()`メソッド：起動したプログラムが終了するのを待つメソッド。ユーザーがプログラムを終了するまで、続きのスクリプトは実行されない。戻り値は終了コードの整数値が返る


**Webサイトを開く**
  - 規定のWebブラウザを開くだけなら標準モジュールの`webbrowser.open()`が便利

```python
import webbrowser
webbrowser.open('https://massasquash.github.io/potatofolio/python/automate_boring_stuff15_1/')
```



## 指定した時刻にアプリを起動するようにスケジューリングする方法
1.  `time.sleep()`を活用してタイマーをセットしておく方法
	- 簡単なプログラムならこのやり方が良いが、プログラムを起動し続けている状態でないと実行しない

```python
import datetime from datetime
import time

targate_date = datetime(2021, 3, 1, 0, 0, 0)  # 実行したい日付
while datetime.now() < target_date:
	time.sleep(1)
```

2. OSに組み込まれたスケジューラを利用する方法
	- 設定が必要なのと、PCの電源がついている間でないと実行しない

| OS | ツール名 |  
| :--- | :--- |  
| Windows | タスクスケジューラ |  
| Mac | launchd |  
| Linux | cron |  

3. クラウドを利用する方法
   - Heroku SchedulerやCloud Functionのスケジューラなど
   - PCの電源をつけておく必要はないが、サーバーへアップするための設定などが必要＆ローカルファイルの扱いが難しい

---
## MEMO
【参考】
- [subprocess — サブプロセス管理 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/subprocess.html#popen-constructor)
- [webbrowser — 便利なウェブブラウザコントローラー — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/webbrowser.html)
- [【Python】subprocess.Popen()関数で他のプログラムを起動する | OFFICE54](https://office54.net/python/python-subprocess-popen)
- [Pythonで特定のファイルをアプリケーションで開く方法とフォルダを開く方法](https://tonari-it.com/python-popen-start-folder/)

【参考書籍】
- O’Reilly「退屈なことはPythonにやらせよう」 15章 時間制御、自動実行、プログラム起動
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---



