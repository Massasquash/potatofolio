---
title: "[退屈Python]7章 正規表現によるパターンマッチング"
date: 2021-01-29T19:02:28+09:00
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - "退屈Python"
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」の学習の備忘録です。

## （１）正規表現Regexオブジェクト
### 1.Regexオブジェクトの生成
```python
import re
phone_num_regex = re.compile(r'<正規表現パターン>')
```

`re`モジュールの`compile()`関数を使うと、Regexパターンオブジェクト（Regexオブジェクト）が返る。
正規表現パターンを表す文字列には、raw文字列を渡すと便利（バックスラッシュがエスケープされないようになるため）

### 2.パターン検索と置換
```python
# 単純な検索
# regex.searchメソッドでmatchオブジェクトが返る
mo = regex.search('<文字列>')

# 全てを検索
# 返り値は文字列のリスト
mo = regex.findall()

# 文字列の置換
regex.sub('')

# matchオブジェクトのテキストを返す
mo.group()
```

`regex.serch`メソッドを使うと、Matchオブジェクトが帰る


### 3.

## （２）正規表現に用いる記号
### 文字集合
|  記号  |  説明  |
| ---- | ---- |
|  .  |  ワイルドカード。改行以外の１文字  |
|  \w, [A-Za-z0-9]  |  文字（大文字/小文字の英字・数字・アンダースコア）  |
|  \W, [^\w]  |  文字（大文字/小文字の英字・数字・アンダースコア）以外  |
|  \d, [0-9]  |  数字  |
|  \D, [^0-9]  |  数字以外  |
|  \s  |  空白文字に一致  |
|  \S, [^\s]  |  空白文字以外に一致  |

### 出現回数と繰り返し系の表現
- 文字集合の後にくっつける記号
- 正確には「文字」 -> 「グループ」。正規表現の中で`()`で括られたものが１グループになる。

|  記号  |  説明  |
| ---- | ---- |
|  ?  |  直前の文字の0回か1回の出現にマッチ |
|  *  |  直前の文字の0回以上の出現にマッチ  |
|  +  |  直前の文字の1文字以上の出現にマッチ  |
|  {n}  |  直前の文字のn回の出現にマッチ。例えば[0-9]{3}で3桁の数字 |
|  {n,}  |  直前の文字をn回以上の出現にマッチ。例えば[0-9]{3,}で3桁以上の数字 |
|  {m, n}  |  直前の文字をm〜n回の出現にマッチ。例えば[0-9]{3, 5}で3〜5桁の数字 |

### その他
- デフォルトでは「貪欲マッチ」＝最も長いものにマッチする。

|  記号  |  説明  |
| ---- | ---- |
|  ^  |  キャレット記号。検索対象の文字列の先頭にマッチ （例: `^\d`）|
|  $  |  ドル記号。検索対象の文字列の末尾にマッチ （例: `\d$`） |
|  |  |  複数のパターンを列記したい時に使う（`or`のイメージ） |
|  ()  |  グループを表す  |
|  ?  |  正規表現グループに?をつけると非貪欲マッチになる（例: `\d{3,5}?`, `.*?`） |

## （3）解読

### 電話番号とメールアドレスの正規表現
[サンプルコード](https://github.com/oreilly-japan/automatestuff-ja/blob/master/ch07/phoneAndEmail.py)より一部変更して（書籍版に記載されているバージョン）記載させていただきます。

```python
# 日本の電話番号用の正規表現
# （市外局番が0から始まる1〜4桁、市内局番が1〜4桁、加入者番号4桁）
phone_regex = re.compile(r'''
  (\d{1,4}|\(\d{1,4}\))  # 市外局番
  (\s|-)           # 区切り
  (\d){1,4})            # ３桁の番号
  (\s|-)           # 区切り
  (\d{3,4})             # ４桁の番号
  (\s*(ext|x|ext.)\s*(\d{2, 5}))?  # 内線番号
  ''', re.VERBOSE)


# 電子メールの正規表現
phone_regex = re.compile(r'''
  [a-zA-Z0-9._%+-]+   # ユーザー名
  @
  [a-zA-Z0-9.-]+      # ドメイン名
  (\.[a-zA-Z]{2,4})  # ドットなんとか
  ''', re.VERBOSE)
```


## 学習



---
## MEMO
【参考書籍】
- 公式Web版（英語）[Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
- 公式サンプルコード [GitHub - oreilly-japan/automatestuff-ja: 『退屈なことはPythonにやらせよう』のリポジトリ](https://github.com/oreilly-japan/automatestuff-ja)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---