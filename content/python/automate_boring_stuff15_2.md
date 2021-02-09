---
title: "[退屈なことはPythonにやらせよう]15-2 マルチスレッドと他プログラムの起動"
date: 2021-02-07T10:00
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - “Python「退屈なことはPythonにやらせよう」”
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」をベースに学習した内容を残した備忘録です。
「15章 時間制御、自動実行、プログラム起動」をもとにまとめています。

## （１）プロセス、スレッド、コアについての整理
まず、コンピュータの一般的な用語についてわかりづらいので整理しました。  
書籍の内容のほか、以下のページを参照しています。  
[【図解】CPUのコアとスレッドとプロセスの違い,コンテキストスイッチ,マルチスレッディングについて | SEの道標](https://milestone-of-se.nesuke.com/sv-basic/architecture/cpu/)

**1.プロセス**  
- プロセスとは「実行中のプログラム（アプリケーション）」のこと
- あるアプリケーションが複数起動しているなら、それぞれ別々のプロセスが起動している
- 各プロセスはそれぞれ複数の「スレッド」を持つことができる
- 「スレッド」と違い、プロセスは他のプロセスを直接読み書きすることができない

**2.スレッド**  
- スレッドとはプログラムの最小実行単位であり、一般的にはスレッドはプロセス内で命令を逐次実行する
- スレッドはCPUコアと対応している
- スレッドは「論理コア」とも呼ばれる。対してCPUに実際に搭載されているものは「物理コア」と呼ぶ
- スレッド数は、同時に「並行処理」できる数に相当する
- コンピュータのCPU（≒プロセッサ）によって最大スレッド数が決まる

**3.コア**  
- CPUの物理コアのこと
- 従来のCPUでは、１つのコアで１つのスレッドを動作させられる
- CPUの進歩により、今では１つのコアに対して複数のスレッド（多くは２つのスレッド）を割り当てることができる

**シングル・マルチのイメージ**  
- シングルスレッド　指一本でプログラムを追うイメージ
- マルチスレッド　複数の指でプログラムを追うイメージ
- シングルプロセス　自分だけでプログラムを実行するイメージ
- マルチプロセス　プログラムのソースコードのコピーを作り、自分と友人とで同じプログラムを別々に実行するイメージ


## （２）マルチスレッドの実装
- Pythonでマルチスレッド実装することで「並行処理」ができるようになる
	- 同時に複数のファイルをダウンロードする
	- 指定日時まで停止させている間に別の処理を行う
	- 同時に複数の作業を行う
- デフォルトではPythonスクリプトは１つの実行スレッドでの「逐次処理」であり、コードの最後まで到達するか`sys.exit()`が呼ばれるとプログラムが終了する。
- 複数のスレッドが同時に変数を読み書きすると不具合が生じる（**並列処理問題**）ので、対策として新しいスレッドでは渡す関数の中で定義されたローカル関数のみを使うようにする

**実装手順**
- マルチスレッドを使う簡単なスクリプト

1. `threadhing`モジュールのインポート 
2. スレッドオブジェクトの作成： `threading.Thread(target=func)`
	- キーワード引数`target`には関数を渡す
3. スレッドを作成して開始する： `thread.start()`メソッド

```python
import threading

# 呼び出したい関数
import time
def stopSecond():
    time.sleep(1)
    print('関数終わり')

thread_obj = threading.Thread(target=stopSecond)
thread_obj.start()
print('プログラム終わり')
```

> プログラム終わり   （ <- 先にこれが出力される）  
> 関数終わり  

- スレッドが終了（正常終了・例外による終了・タイムアウト）
するまで待機させる場合は`thread.join()`メソッドを使う
```python
# 
thread_obj = threading.Thread(target=stopSecond)
thread_obj.start()
thread_obj.join()
print('プログラム終了')
```

> 関数終わり  
> プログラム終了     （<- これが一番最後に出力される）  

- スレッドで実行する関数に引数を渡したい場合は `threading.Thread()`にキーワード引数`args=[]`,`kwargs={}`を渡す
```python
thread_obj = threading.Thread(target=print,
									  args = ['a', 'b', 'c']
                              kwargs = {'sep': ' & '})
thread_obj.start()
# -> print('a', 'b', 'c', sep=' & ') と同じ
```



## （３）プロセス実行
書籍の他に下記を参考にさせていただきました。実際の使いやすさを想定して、書籍の流れと違う感じで整理している部分もあります。  
[【Python】subprocess.Popen()関数で他のプログラムを起動する | OFFICE54](https://office54.net/python/python-subprocess-popen)  


- `subprocess`モジュール：Pythonからコマンドを実行するための標準モジュール
- 例えば毎日起動する定番のアプリをプログラムを実行することで一発で開いたりできるようになる

**規定のアプリでファイルを開く**  
	- プログラム（プロセス）をオープンする関数  `subprocess.Popen()`を使う
	- ここではGUI上で「ファイルをダブルクリック」した時と同じ動作をプログラムで実行するやり方を書いておく
	- コマンドライン引数を渡したい場合は、リストの<ファイルパス>の後に要素を追加して渡すことができる

	- OSによって起動方法や、渡すコマンド（`start`, `open`）が違うので注意

```python
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
  - 単純にWebブラウザを開くだけなら標準モジュールの`webbrowser.open()`が便利
  - [webbrowser — 便利なウェブブラウザコントローラー — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/webbrowser.html)

```python
import webbrowser
webbrowser.open('https://massasquash.github.io/potatofolio/python/automate_boring_stuff15_1/')
```



**指定した時刻にアプリを起動するようにスケジューリングする方法**
- 詳しいやり方は別途整理しておきたいです。

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
- [【図解】CPUのコアとスレッドとプロセスの違い,コンテキストスイッチ,マルチスレッディングについて | SEの道標](https://milestone-of-se.nesuke.com/sv-basic/architecture/cpu/)
- [並列処理の用語 - 猫型エンジニアのブログ](http://alexei-karamazov.hatenablog.com/entry/2014/04/20/105644)
- [【Python】subprocess.Popen()関数で他のプログラムを起動する | OFFICE54](https://office54.net/python/python-subprocess-popen)

- [threading — スレッドベースの並列処理 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/threading.html)
- [subprocess — サブプロセス管理 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/subprocess.html#popen-constructor)
- [webbrowser — 便利なウェブブラウザコントローラー — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/webbrowser.html)


<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---



