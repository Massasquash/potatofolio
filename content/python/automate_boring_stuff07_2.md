---
title: "[退屈Python]7章 正規表現によるパターンマッチング 演習"
date: 2021-01-01
lead: "「退屈なことはPythonにやらせよう」の学習備忘録"
categories:
  - "退屈Python"
---


# はじめに
この記事はO’Reilly「退屈なことはPythonにやらせよう」をベースに学習した内容を残した備忘録です。　　
このページでは、演習問題でやったことを残しておきます。

## 演習問題
### 7-17

 全ての数字と小文字にマッチする文字集合


```python
import re

re.compile(r'[a-z0-9]')
```




    re.compile(r'[a-z0-9]', re.UNICODE)



### 演習7-18


```python
num_regex = re.compile(r'\d+')
result = num_regex.sub('X', '12 drummers, 11 pipers, five rings, 3hens')
result
```




    'X drummers, X pipers, five rings, Xhens'



### 演習7-20

３桁ごとのカンマのついた数字にマッチする正規表現を書く

- 1〜3桁の数字があり、その後カンマの後に3桁の数字が続くのが、0回以上出現する


```python
import re

# 最初に数値が1〜3個、以下存在は自由で、カンマ、数値3個...
num_with_comma = re.compile(r'''
    ^\d{1,3}
    (,\d{3})+$
    ''', re.VERBOSE)
```


```python
print(num_with_comma.search('12,345').group())
```

    12,345



```python
# 上の例だと、全文が数字文字列の場合にしかマッチしない。文章の途中でもマッチできるコード
num_with_comma = re.compile(r'''
    (?<!\d)
    \d{1,3}
    (,\d{3})+
    (?!\d)
    ''', re.VERBOSE)
```

```python
print(num_with_comma.search('私の所持金は12,345です').group())
```

    12,345

#### メモ
- 上記コードの参考記事　https://teratail.com/questions/85890


```python
# 1〜3桁の数字にマッチ
num_regexp = re.compile(r'\d{1,3}')
print(num_regexp.search('12345').group())
```

    123



```python
# 上の例だと、3桁を超える場合でもマッチしてしまう
# 桁数が合わないとマッチしないようにするには、^と$で行の始めと終わりのマッチを見るのがシンプル
num_regexp = re.compile(r'^\d{1,3}$')
print(num_regexp.search('123').group())
```

    123



```python
print(num_with_comma.search('12345').group())
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-162-f0e31216d05e> in <module>
    ----> 1 print(num_with_comma.search('12345').group())
    

    AttributeError: 'NoneType' object has no attribute 'group'



```python
# 以下だと、12345を検索した時に123がヒットしてしまう
num_with_comma = re.compile(r'''
    \d{1,3}
    (,\d{3})*
    ''', re.VERBOSE)
```

### 7-21

姓が「Nakamoto」である人のフルネームにマッチする正規表現。
- 名は姓の前にあり、姓名とも1単語で大文字から始まるとする
- 名前がなかったり、前の単語に文字以外（Mr. でピリオドなど）が使われているのは省く

条件を正規表現で表すと
- 名の先頭は大文字の英字
- 英字0つ以上の文字列（記号、数字は除く）
- 半角スペース1つ
- Nakamoto


```python
import re

nakamoto_regexp = re.compile(r'[A-Z]\w*[　Nakamoto]')
```


```python
print(nakamura_regexp.search('Satoshi　Nakamoto').group())
```

    Satoshi　Nakamoto



```python
print(nakamura_regexp.search('satoshi nakamoto').group())
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-107-743b7d68bddb> in <module>
    ----> 1 print(nakamura_regexp.search('satoshi nakamoto').group())
    

    AttributeError: 'NoneType' object has no attribute 'group'



```python
print(nakamura_regexp.search('Mr.　Nakamoto').group())
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-104-3ef11e7c868d> in <module>
    ----> 1 print(nakamura_regexp.search('Mr.　Nakamoto').group())
    

    AttributeError: 'NoneType' object has no attribute 'group'



```python
print(nakamura_regexp.search('Nakamoto').group())
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-105-49e3955d3b06> in <module>
    ----> 1 print(nakamura_regexp.search('Nakamoto').group())
    

    AttributeError: 'NoneType' object has no attribute 'group'


### 7-22

以下のような文とマッチする正規表現。大文字と小文字を区別しない。
- 最初の単語が AliceかBobかCarol
- ２番目の単語が eatsかpetsかthrows
- ３番目の単語が applesかcatsかbaseballs
- 最後はピリオドで終わる文  


```python
import re

sentence_regexp = re.compile(r'(Alice|Bob|Carol) (eats|pets|throws) (apples|cats|baseballs)\.')
```


```python
print(sentence_regexp.search('Alice pets cats.').group())
```

    Alice pets cats.


#### メモ
- ドットを正規表現で表すには、バックスラッシュでエスケープする必要がある

## 演習プロジェクト

### 7.18.1 強いパスワードの検出

引数に渡されたパスワード文字列が強いかどうかを正規表現を用いて確認する関数を作る。 

強いパスワードの条件　　
- ８文字以上
- 大文字と小文字を含む
- １つ以上の数字を含む


```python
# 全体のスクリプト
import re

PASSWORD = '1234ab44'

def check_password(password):
    """パスワード文字列の強さを判定
    
    Arguments:
        password: パスワード文字列
    Returns:
        判定結果を文字列で返す
    """
    # 8桁以上あるかの判定
    if not pass_regex.search(PASSWORD):
        return 'No...PASSWORD should have the above 8 characters.'

    pattern1 = re.compile(r'[a-z]')
    pattern2 = re.compile(r'[A-Z]')
    pattern3 = re.compile(r'\d')
    
    # 大文字・小文字、１つ以上の数字を含む判定
    if all((pattern1.search(PASSWORD),
            pattern2.search(PASSWORD),
            pattern3.search(PASSWORD))):
        return 'OK! This PASSWORD is strong!'
    else:
        return 'No...PASSWORD should be strong.'
```

#### メモ
- もっと短くできそう

### 7.18.2 正規表現を用いたstrip()メソッド

- 文字列を引数にとり、文字列メソッドのstrip()と同等の動きをする関数を書く  
- デフォルトでは、文字列の先頭と末尾から空白文字を除去する
- 追加の文字列引数があれば、それを文字列の先頭と末尾から除去する


```python
import re

def like_strip(string, chars=None):
    """    
    Arguments:
       string: 処理したい文字列
    Returns:
        処理した後の文字列
    """
    if chars:
        delete_str = re.compile(chars)
    else:
        delete_str = re.compile(r'\s')        
    return delete_str.sub('', string)
```


```python
like_strip( '   ああああああ     　　　')
```




    'ああああああ'




```python
like_strip( '   ああああああ     　　　いいい', 'い')
```




    '   ああああああ     \u3000\u3000\u3000'



#### メモ

-  `\u3000`は全角スペースを表すUniode文字


```python
help('aaa'.strip)
```

    Help on built-in function strip:
    
    strip(chars=None, /) method of builtins.str instance
        Return a copy of the string with leading and trailing whitespace removed.
        
        If chars is given and not None, remove characters in chars instead.
    



```python
# より短く書けるが、仮引数の初期値はNoneの方が自然な感じがする
def like_strip_2(string, chars='\s'):
    """    
    Arguments:
       string: 処理したい文字列
    Returns:
        処理した後の文字列
    """
    delete_str = re.compile(r'\s')        
    return delete_str.sub('', string)
```


```python
# 初期値をNoneのまま変えずに、論理演算子を使って短くできた
def like_strip_3(string, chars=None):
    """    
    Arguments:
       string: 処理したい文字列
    Returns:
        処理した後の文字列
    """
    delete_str = re.compile(chars or r'\s')
    return delete_str.sub('', string)
```


```python
# delete_strの挙動
print(re.compile(None or r'\s'))
print(re.compile('何らかの文字列' or r'\s'))
```

    re.compile('\\s')
    re.compile('何らかの文字列')