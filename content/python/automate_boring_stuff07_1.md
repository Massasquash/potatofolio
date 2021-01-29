---
title: "[退屈Python]7章 正規表現によるパターンマッチング"
date: 2021-01-29T19:02:28+09:00
lead: "description"
categories:
  - "退屈Python"
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」の学習の備忘録です。

## 正規表現Regexオブジェクト
Regexオブジェクトの生成

```python
import re

phone_num_regex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')
```


## 正規表現に用いる記号
|  文字  |  説明  |
| ---- | ---- |
|  *  |  直前の文字の0回以上の繰り返しにマッチ  |
|  +  |  直前の文字の1文字以上の繰り返しにマッチ  |
|  ?  |  直前の文字の0回か1回の出現にマッチ |
|  {n}  |  直前の文字をn回一致。 [0-9]{3}で３桁の数字 |
|  {n,}  |  直前の文字をn回以上と一致。 [0-9]{3,}で３桁以上の数字 |
|  {m, n}  |  直前の文字をm〜n回一致。 [0-9]{3, 5}で３〜５桁の数字 |

|  文字  |  説明  |
| ---- | ---- |
|  .  |  改行以外の１文字  |
|  \s  |  空白文字に一致  |
|  \S  |  空白文字以外に一致（[^\s]と同意）  |

|  文字  |  説明  |
| ---- | ---- |
|  \w  |  文字（大文字/小文字の英字・数字・アンダースコア）。 [A-Za-z0-9]と同じ  |
|  \W  |  文字（大文字/小文字の英字・数字・アンダースコア）以外。 [^\w]と同じ  |
|  \d  |  数字。 [0-9]と同じ  |
|  \D  |  数字以外。 [^0-9]と同じ  |





## スニペット
### 
```python
# 改行も含めて複数行をマッチ
/[\s\S]*/
```


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

# おわりに


---
## MEMO
【参考書籍】
- 公式Web版（英語）[Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
- 公式サンプルコード [GitHub - oreilly-japan/automatestuff-ja: 『退屈なことはPythonにやらせよう』のリポジトリ](https://github.com/oreilly-japan/automatestuff-ja)
---