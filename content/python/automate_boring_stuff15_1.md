---
title: "[退屈なことはPythonにやらせよう]15-1 時間の表現について"
date: 2021-02-09T12:00
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - "Python「退屈なことはPythonにやらせよう」"
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」をベースに学習した内容を残した備忘録です。
「15章 時間制御、自動実行、プログラム起動」をもとにまとめています。

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
- datetimeオブジェクトを`.strftime()`メソッドでフォーマットして文字列表示させることができる。
- 引数に変換したい書式文字列を渡す
- 「f」はフォーマット（整形）の意味

**strftime()書式一覧**  
**よく使うやつ**  
| 書式 | 意味 | 例 |  
| :--- | :--- | :--- |  
| %Y | 西暦 4桁 | '2021' |
| %y | 西暦 2桁 | '00'〜'99' |
| %m | 月（0埋め） | '01'〜'12' |
| %d | 日 | '01'〜'31' |
| %w | 曜日 | '0'（日曜）〜'6'（土） |
| %H | 時（24時間制） | '00'〜'23' |
| %h | 時（12時間制） | '01'〜'12' |
| %M | 分（0埋め） | '00'〜'59' |
| %S | 秒（0埋め） | '01'〜'31' |

**こんなのもあるよ**
| 書式 | 意味 | 例 |  
| :--- | :--- | :--- |  
| %B | 月名 | 'January' |
| %b | 月の略称 | 'Jan' |
| %A | 曜日名 | 'Sunday' |
| %a | 曜日の略称 | 'Sun' |
| %j | 年初からの日数 | '001'〜'366' |
| %p | AMかPMか | 'AM', 'PM' |
| %c | 日時をまとめて表示 | 'Sun Feb  7 12:57:18 2021' |
| %x | 日付をまとめて表示 | '02/07/21' |
| %X | 時間をまとめて表示 | '12:57:18' |
| %% | %文字 | |

- より詳細はドキュメント参照
  [datetime — 基本的な日付型および時間型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/datetime.html#strftime-strptime-behavior)

## （３）文字列→datetime型 に変換
- 日付情報の入った文字列を`datetime.datetime.strptime()`関数でdatetimeオブジェクトに変換することができる
- 第１引数に変換したい文字列, 第２引数に対応する書式文字列を渡す(どちらも文字列)
- 文字列が正確にマッチしないと`ValueError`例外が発生する
- 「p」はパース（構文解析）の意味

**使用例**
```python
date = datetime.datetime.strptime('20200207', '%Y%m%d')
print(date)  # -> 2020-02-07 00:00:00
print(type(date))  # -> <class 'datetime.datetime'>
```

---
## MEMO
【参考】
- [time — 時刻データへのアクセスと変換 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/time.html)
- [datetime — 基本的な日付型および時間型 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/datetime.html)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---