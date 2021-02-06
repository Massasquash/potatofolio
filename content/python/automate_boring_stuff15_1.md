---
title: "[退屈Python]15章 時間制御、自動実行、プログラム起動"
date: 2021-02-06T10:00
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - "退屈Python"
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」をベースに学習した内容を残した備忘録です。


## （１）Pythonの時間関数のまとめ
- Pythonで日付と時間を扱うデータ型は主に３種類

| 呼称 | データ型 | モジュール |  表示例 | 備考 |  
| :--- | :--- | :--- | :--- | :--- |  
| ①UNIXエポックに基づくタイムスタンプ | float型 | time | 1612587698.479415 | エポックタイぷからの秒数<br>整数値または浮動小数点値　|  
| ②特定の時刻・時間 | datetime型 | datetime | 2021-02-06 14:02:23.592996 | date(), time()メソッドの戻り値はdatetime型<br>他それぞれの属性は整数値 |  
| ③datetime同士の時間差 | timedelta型 | datetime | 0:00:03.003681 | それぞれの属性は整数値 |  

- **UNIXエポックタイム**は多くのプログラミング言語で標準的な参照時間 =  1970/1/1 0:00 UTC（協定世界時）
- その時間からの経過時間を**エポックタイムスタンプ**と呼ぶ

**参考コード**
```python
import time
import datetime

# ①UNIXエポックに基づくタイムスタンプ
epoch = time.time()
print(epoch)  # -> 1612587698.479415
print(type(epoch)) # -> <class 'float'>

# ②特定の時間 datetime型
dt = datetime.datetime.now()
print(dt)  # -> 2021-02-06 14:02:23.592996
print(type(dt))  # -> <class 'datetime.datetime'>

# ③datetime同士の時間差 timedelta型
dt1 = datetime.datetime.now()
time.sleep(3)
dt2 = datetime.datetime.now()

td = dt2 - dt1
print(td)  # -> 0:00:03.003681
print(type(td))  # -> <class 'datetime.timedelta'>
```


## （２）datetime型→文字列 に変換
- datetimeオブジェクトを`datetime.strftime()`メソッドでフォーマットして文字列表示させることができる。
- 引数に変換したい書式文字列を渡す
- 「f」はフォーマット（整形）の意味

**strftime()書式一覧**
| 書式 | 意味 |  
| :--- | :--- |  
| |  | 


## （３）文字列→datetime型 に変換
- 日付情報の入った文字列を`datetime.strptime()`メソッドでフォーマットして文字列表示させることができる。
- 第１引数に変換したい文字列, 第２引数に対応する書式文字列を渡す
- 「p」はパース（構文解析）の意味




---
## MEMO
【参考】
- [time — 時刻データへのアクセスと変換 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/time.html)
- [datetime — 基本的な日付型および時間型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/datetime.html)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---