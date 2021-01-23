---
title: "[Effective Python]項目4 Cスタイルフォーマット文字列とstr.formatは使わずf文字列で埋め込む"
date: 2021-01-04
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**１章 Pythonic思考**  
>Pythonコミュニティでは、特定のスタイルに沿ったコードを表すのにPythonicという形容詞を使います。Pythonのイディオムは、この言語を使い仲間と作業する経験から時間をかけて発展してきました。１章では、Pythonで最も中心的な共通的に使われる最良の方法を扱います。（まえがき xii より引用）

（目次）
- 項目1 使用するPythonのバージョンを知っておく
- 項目2 PEP8スタイルガイドに従う
- 項目3 bytesとstrの違いを知っておく
- **項目4 Cスタイルフォーマット文字列とstr.formatは使わずf 文字列で埋め込む**
- 項目5 複雑な式の代わりにヘルパー関数を書く
- 項目6 インデックスではなく複数代入アンパックを使う
- 項目7 rangeではなくenumerateを使う
- 項目8 イテレータを並列に処理するにはzipを使う
- 項目9 forループとwhileループの後のelseブロックは使わない
- 項目10 代入式で繰り返しを防ぐ



## 項目4 Cスタイルフォーマット文字列とstr.formatは使わずf文字列で埋め込む
Python3では４つの出力フォーマット方式があります。これを使うと、文字列にデータ値を埋め込んでテキストとして出力できます。  

1. Cスタイルフォーマット文字列（フォーマット演算子`%`を使う）
2. 組み込みのformat関数
3. str.formatメソッド
4. フォーマット済み文字列（f文字列）

この中でどの書き方が良いのか、というのを検証しているのががこの項目。結論から言うと、4つ目の**フォーマット済み文字列（f文字列）** の方法で書くのが一番簡潔で表現もしやすい、と言うことです。Python3.6以降にできた一番新しい構文です。  
他の書き方だと課題が多いので、f文字列の書き方をマスターするのが良さそうです。

## 解説
### （１）f文字列の基本構文
フォーマット済み文字列（f文字列）は、文字列の中に式や値を埋め込むことができる構文です。
文字列リテラルの頭に`f`をつけて、文字列の中に埋め込みたい式や値を`{}`の中に書くことができます。
（この埋め込む式や値の入った`{}`を **「プレースホルダ」** と呼びます）

[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_04.py)を記載します。
```python
# Example 23
key = 'my_var'
value = 1.234

formatted = f'{key} = {value}'
print(formatted)

# -> my_var = 1.234
```


### （２）f文字列で書式設定をつける
f文字列を使って、プレースホルダーの`{}`の式や値の後に`:`を使って、様々な書式設定ができます。  
（例えば整列（アライン）、文字埋め、数値の桁区切り、パーセンテージ表記など）

この書式オプションの書き方は、ドキュメントでは **「書式指定ミニ言語」** と呼ばれています。


**整列と文字埋め**  
- 整列（左寄せ・中央寄せ・右寄せ）　　`:<10``:^10``:>10`
- 整列の際に埋める文字を指定する　`:a<10`　

2つ目の埋める文字については、`a`を埋めたい文字列にします）
実際にコードを書いてみます。

```python
value = 12345

print(f'{value:<10}')   # 12345
print(f'{value:^10}')   #   12345   
print(f'{value:>10}')   #     12345
print(f'{value:a<10}')  # 12345aaaaa
```

**数値をゼロ埋め・桁区切り** 
- 数値をゼロ埋めして10桁で表示　`:0=10`, `:010`
- 数値を3桁ずつカンマ区切りで表示 `:,`

先ほどの文字列で埋める書式を利用すると、数値のゼロ埋めができます。  
大きい数のカンマ区切りも簡単にできて便利です。  
これらは使う機会も多いかもしれません。

```python
value = 12345

print(f'{value:0=10}')  # 0000012345（0埋め10桁）
print(f'{value:06}')    # 012345（0埋め6桁）
print(f'{value:,}')     # 12,345（カンマ区切り）
```

**小数点以下の桁数指定** 
- 小数点以下3桁で丸める　`:.3f`

```python
value = 123.45678

print(f'{value:.3f}')  # 123.456
```

**パーセント表記**
- 数値をパーセント表記にする　`:%`

デフォルトでは小数点以下が6桁で表示されるそうです。
パーセント表記も、上で行った表記で小数点以下n桁で丸めることができます。

```python
value = 0.123

print(f'{value:%}')   # 12.300000%
print(f'{value:.2%}') # 12.30%

```

### f文字列以外の出力フォーマットの記述法
最初にあげた他の3つの書き方
1. Cスタイルフォーマット文字列（フォーマット演算子`%`を使う）
2. 組み込みのformat関数
3. str.formatメソッド

これらについてもさらっと見ておきます。


```python
# 1. Cスタイルフォーマット文字列（%演算子）
# %d（10進数整数値）, %s（文字列）などをプレースホルダーとし、置き換えたい変数は%の後にタプルで指定
name = 'Bob'
age = 30
formatted = '%s は %d 歳です。' % (name, age)
print(formatted)  # Bob は 30 歳です。


# 2. 組み込みのformat関数
# Python3より。第1引数に表示したい値、第2引数に書式化するためのミニ言語
num = 123456789
formatted = format(num, ',')
print(formatted)  #  123,456,789


# 3. str.formatメソッド
# {}でプレースホルダの指定ができるようになる
name = 'Bob'
age = 30
formatted = '{} は {} 歳です。'.format(name, age)
print(formatted)  # Bob は 30 歳です。
```

1つ目の方式だと文字列が長い場合に可読性が落ちたり、メンテナンス性が悪くなります（現在は非推奨になっているようです）。  
新しい3つ目の方式のほうが冗長性はなくなるが、それでもやはり最新のf文字列が一番スッキリする書き方ができます。

書籍のこの項目では、古い書き方それぞれの問題点について深掘りされていて、f文字列がなぜ優れているのかが解説されていました。

### 備考：変数名の後の !r について
[TODO]
## 感想
f文字列でもミニ言語でフォーマットを指定できるのは上手く活用していきたいです。特に数値のゼロ埋めやカンマ区切りなどが短く書けるのが実用性のありそうな印象でした。  
その他のフォーマットについては正直不要だと思うのですが、古いコードを読んでいると出てくることもあるのかなと思います。こんな書き方があるんだと言うのを知っていれば、出てきた際に読み解きやすくなるのかなと思いました。

---
## MEMO
【参考記事】
- フォーマット済み文字列 [7. 入力と出力 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/tutorial/inputoutput.html#formatted-string-literals)
- ミニ言語 [string — 一般的な文字列操作 — Python 3.9.1 ドキュメント](https://docs.python.org/ja/3/library/string.html#formatspec)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---