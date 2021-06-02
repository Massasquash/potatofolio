---
title: "[Python]スレッド・プロセスの概念とマルチスレッド"
date: 2021-02-07T10:00
lead: "threadingモジュールでのマルチスレッド実装"
categories:
  - “Python”
---

## 1.コア・スレッド・プロセスについての整理
まず、コンピュータの一般的な用語についてわかりづらいので整理しました。  
書籍の内容のほか、以下のページも参照にさせていただきました。  

[【図解】CPUのコアとスレッドとプロセスの違い,コンテキストスイッチ,マルチスレッディングについて | SEの道標](https://milestone-of-se.nesuke.com/sv-basic/architecture/cpu/)

「コア > スレッド > プロセス」という順に、ハードからソフト（アプリケーション）に近付いてくるように捉えました。一番ユーザーに近いプロセスから考えた方がわかりやすそうだったので、以下そのような順で整理しています。


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

## 2.シングルスレッド・マルチスレッドについての整理
- シングルスレッド　指一本でプログラムを追うイメージ
- マルチスレッド　複数の指でプログラムを追うイメージ
- シングルプロセス　自分だけでプログラムを実行するイメージ
- マルチプロセス　プログラムのソースコードのコピーを作り、自分と友人とで同じプログラムを別々に実行するイメージ


## 3.マルチスレッドの実装
デフォルトではPythonスクリプトは１つの実行スレッドでの「**逐次処理**」であり、コードの最後まで到達するか`sys.exit()`が呼ばれるとプログラムが終了します。  
そこでPythonで`threading`モジュールを使って**マルチスレッド**実装することで「**並行処理**」ができるようになります。これにより例えば、
- 同時に複数のファイルをダウンロードする
- 指定日時まで停止させている間に別の処理を行う
- 同時に複数の作業を行う

というような処理を同時並行で行うことができます。

複数のスレッドが同時に変数を読み書きすると不具合が生じる（**並列処理問題**）ことがあります。  
対策として新しいスレッドのオブジェクトを作る際には、渡す関数の中で定義されたローカル関数のみを使うようにする

**実装手順**
マルチスレッドを使う簡単なスクリプトの例です。

1. スレッドオブジェクトの作成： `threading.Thread(target=func)`  
   このキーワード引数`target`には関数を渡す

2. スレッドを作成して開始する： `thread.start()`メソッド

```python
import threading
import time

# 呼び出したい関数
def stopSecond():
    time.sleep(1)
    print('関数終わり')

thread_obj = threading.Thread(target=stopSecond)
thread_obj.start()
print('プログラム終わり')
```

```
プログラム終わり   （ <- 先にこれが出力される）  
関数終わり  
```

- スレッドが終了（正常終了・例外による終了・タイムアウト）
するまで待機させる場合は`thread.join()`メソッドを使う
```python
thread_obj = threading.Thread(target=stopSecond)
thread_obj.start()
thread_obj.join()
print('プログラム終了')
```

```
> 関数終わり  
> プログラム終了     （<- これが一番最後に出力される）  
```

- スレッドで実行する関数に引数を渡したい場合は `threading.Thread()`にキーワード引数`args=[]`,`kwargs={}`を渡す
```python
thread_obj = threading.Thread(target=print,
									  args = ['a', 'b', 'c']
                              kwargs = {'sep': ' & '})
thread_obj.start()
# -> print('a', 'b', 'c', sep=' & ') と同じ
```


---
## MEMO
【参考】
- [threading — スレッドベースの並列処理 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/threading.html)
- [【図解】CPUのコアとスレッドとプロセスの違い,コンテキストスイッチ,マルチスレッディングについて | SEの道標](https://milestone-of-se.nesuke.com/sv-basic/architecture/cpu/)
- [並列処理の用語 - 猫型エンジニアのブログ](http://alexei-karamazov.hatenablog.com/entry/2014/04/20/105644)

【参考書籍】
- O’Reilly「退屈なことはPythonにやらせよう」 15章 時間制御、自動実行、プログラム起動
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---



