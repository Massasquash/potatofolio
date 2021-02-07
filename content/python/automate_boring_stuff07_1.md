---
title: "07 正規表現について"
date: 2021-01-30T10:00
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - "Python「退屈なことはPythonにやらせよう」"
---

# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」をベースに学習した内容を残した備忘録です。  
「7章 正規表現によるパターンマッチング」をもとにまとめています。

## （１）正規表現とは
- テキストパターンの記述表のこと
- 英語ではregular expression、略してregexと呼ばれる

## （２）テキストパターンを検索する基本的な流れ
- `re`モジュールをインポートする
- `re.compile()`関数で正規表現パターンを使ってRegexオブジェクトを生成する（この時raw文字列を使うと良い）
- Regexオブジェクトの`search()`メソッドに検索対象の文字列を渡し、Matchメソッドを返す
- Matchオブジェクトの`group()`メソッドを使って実際にマッチした文字列を取得する

### 1.Regexオブジェクトの生成
```python
import re
regex = re.compile(r'\d\d\d')  # -> 101
```

- `re.compile()`の引数に正規表現パターンを渡す
- Regexオブジェクトが返る
- 正規表現パターンにはrow文字列を使うと、バックスラッシュを使った文字をエスケープせずそのまま表示させることができる （例：`r'\n'`）
- 正規表現パターンの書き方については後述

**re.compile()のヘルプ**（`help()`でドキュメントを確認）  
>compile(pattern, flags=0)  
>    Compile a regular expression pattern, returning a Pattern object.  


### 2.正規表現パターンを検索する
```python
# 検索して最初のMatchオブジェクトを取得する
mo = regex.search('101匹わんちゃんを133回数見た')

# 取得したMatchオブジェクトの文字列を取得する
print(mo.group())
```

- Regexオブジェクトの`serch()`メソッドで渡された文字列の中から正規表現にマッチする部分を探し、Matchオブジェクトを返す
- マッチするパターンが見つからなければ、`None`を返す
- Matchオブジェクトの`group()`メソッドを使うと、マッチしたテキストを返す
- 正規表現パターンを`()`でグルーピングした場合は、`group()`の引数にインデックスを渡すことでグループを取り出せる
- 引数が`0`また引数なしの時は、マッチした文字列全体を返す


```python
# 全ての文字列を探す場合
mo = regex.findall('101匹わんちゃんを133回数見た')
print(mo)  # -> ['101', '133']
```
- Regexオブジェクトの`findall()`メソッドで、全てのマッチを探すことができる
- `search()`メソッドが見つかった文字列のMatchオブジェクトを返すのに対して`findall()`メソッドは見つかった全ての文字列のリストを返す
- グループのある正規表現パターンの場合は、タプルのリストを返す


**`search()`メソッドのヘルプ**  
>search(pattern, string, flags=0)  
>    Scan through string looking for a match to the pattern, returning  
>    a Match object, or None if no match was found.  

**`search()`メソッドのヘルプ**  
>group(...) method of re.Match instance  
>    group([group1, ...]) -> str or tuple.  
>    Return subgroup(s) of the match by indices or names.  
>    For 0 returns the entire match.  

**`findall()`メソッドのヘルプ**
>findall(string, pos=0, endpos=9223372036854775807) method of re.Pattern instance  
>    Return a list of all non-overlapping matches of pattern in string.  


### 3.文字列を置換する
```python
import re
regex = re.compile(r'\d\d\d')

# 文字列を置換
after = regex.sub('xxx', '101匹わんちゃんを133回数見た')
print(after)  # -> xxx匹わんちゃんをxxx回見た
```

- Regexオブジェクトの`sub()`メソッドの第１引数に置き換える文字列、第２引数に検索置換対象の文字列を入れると、置換後の文字列を返す
- 全てのマッチ部分が置換される

**`sub()`メソッドのヘルプ**  
>sub(repl, string, count=0) method of re.Pattern instance  
>    Return the string obtained by replacing the leftmost non-overlapping occurrences of pattern in string by the replacement repl.  


## （３）正規表現パターン
- Regexオブジェクトを作るために`re.compile(r'<正規表現パターン>')`の引数に渡す際の書き方
- `[]`で囲むとその中の文字列のどれかを表す （例：[a-zA-Z0-9], [0-9._%+-]）
- `[]`の中の最初にキャレット記号`^`をつけると、補集合になる（例：[^0-9] 数値以外）
- 複数のパターンを縦棒`|`で区切ると、「または」のような意味になる （例：`Bat(man|mobile|copter|bat))`）

### 1.文字の表現（ワイルドカード、文字集合）
|  記号  |  説明  |
| ---- | ---- |
|  .  |  ワイルドカード。改行以外の１文字  |
|  \w（[A-Za-z0-9]）  |  文字（大文字/小文字の英字・数字・アンダースコア）  |
|  \W（[^\w]）  |  文字（大文字/小文字の英字・数字・アンダースコア）以外  |
|  \d（[0-9]）  |  数字  |
|  \D（[^0-9]）  |  数字以外  |
|  \s  |  空白文字に一致  |
|  \S（[^\s]）  |  空白文字以外に一致  |

### 2.出現回数を表す表現
- 文字集合の後にくっつける記号
- 例えば`.*`で「改行以外の全ての文字列」

|  記号  |  説明  |
| ---- | ---- |
|  ?  |  直前のグループの0回か1回の出現にマッチ |
|  *  |  直前のグループの0回以上の出現にマッチ  |
|  +  |  直前のグループの1文字以上の出現にマッチ  |
|  {n}  |  直前のグループのn回の出現にマッチ。例えば[0-9]{3}で3桁の数字 |
|  {n,}  |  直前のグループのn回以上の出現にマッチ。例えば[0-9]{3,}で3桁以上の数字 |
|  {m, n}  |  直前のグループのm〜n回の出現にマッチ。例えば[0-9]{3, 5}で3〜5桁の数字 |

### 位置関係を表す表現
|  記号  |  説明  |
| ---- | ---- |
|  ^  |  キャレット記号。検索対象の文字列の先頭にマッチ （例: `^\d`）|
|  $  |  ドル記号。検索対象の文字列の末尾にマッチ （例: `\d$`） |
<!-- |  \<  |  単語の先頭にマッチ | -->
<!-- |  \>  |  ド単語の末尾にマッチ | -->

### その他の表現
- `()`でグルーピングができる
- Matchオブジェクトが複数の可能性がある場合に、デフォルトでは「貪欲マッチ」＝最も長いものにマッチする
- `{n,m}?`, `*?`, `+?`と出現回数を表す記号の後に`?`をつけると、直前のグループの「非貪欲マッチ」＝最も短いものにマッチする（例: `\d{3,5}?`, `.*?`）



## （４）re.compile()に渡すオプション
[サンプルコード](https://github.com/oreilly-japan/automatestuff-ja/blob/master/ch07/phoneAndEmail.py)より記載させていただきます。

```python
# 日本の電話番号用の正規表現
# （市外局番が0から始まる1〜4桁、市内局番が1〜4桁、加入者番号4桁）
phone_regex = re.compile(r'''
  (\d{1,4}|\(\d{1,4}\))            # 市外局番
  (\s|-)                           # 区切り
  (\d){1,4})                       # ３桁の番号
  (\s|-)                           # 区切り
  (\d{3,4})                        # ４桁の番号
  (\s*(ext|x|ext.)\s*(\d{2, 5}))?  # 内線番号
  ''', re.VERBOSE)
```

- `re.compile()`の第２引数にオプションを指定することができる
- `re.VERBOSE`を渡すと冗長モードに。正規表現パターンの空白文字やコメントを無視してくれるので、コードが見やすくなる（上記コード参照）
- `re.IGNORECASE`もしくは`re.I`を渡すと、大文字・小文字の区別をなくす
- `re.DOTALL`を渡すと、ドット文字`.`（ワイルドカード）が改行を含む全ての文字とマッチするようになる
- これらのオプションを組み合わせる場合は`|`でつなぐ（例： `re.IGNORECASE | re.DOTALL`）


<!-- 
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


-->


---
## MEMO
【参考】
- 公式Web版（英語）  
  [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
- 公式サンプルコード  
  [GitHub - oreilly-japan/automatestuff-ja: 『退屈なことはPythonにやらせよう』のリポジトリ](https://github.com/oreilly-japan/automatestuff-ja)
- 公式の演習問題の解答例  
  [GitHub - oreilly-japan/automatestuff-ja: 『退屈なことはPythonにやらせよう』のリポジトリ](https://github.com/oreilly-japan/automatestuff-ja)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=487311778X&linkId=691e891718cdd36feb75e664a0a2f53a&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---