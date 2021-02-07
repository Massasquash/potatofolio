---
title: "[Effective Python]84 docstring"
date: 2021-01-01T10:84
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
draft: true
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**10章 協働作業（コラボレーション）**  
Pythonプログラムで協働作業を行うには、コードをどのように書くかについて、よく考えておかねばなりません。1人で作業している場合でも、他の人が書いたモジュールをどのように使うとよいか理解する必要があります。10章では、Pythonプログラムで一緒に作業できるようにする標準的なツールやベストプラクティスを扱います。

## 項目84 すべての関数、クラス、モジュールについてdocstringを書く

# 解説
あらゆるモジュール、クラス、メソッド、関数にdocstringを使ってドキュメンテーションを書くことが推奨されています。  
学習の順序として、わかりやすいように関数でのdocstringについて整理します。

## （１）関数のdocstring
書籍で紹介されている関数のdocstringの例です。
[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_84.py)より記載させていただきます。

```python
# Example 5
import itertools
def find_anagrams(word, dictionary):
    """Find all anagrams for a word.
    This function only runs as fast as the test for
    membership in the 'dictionary' container.
    Args:
        word: String of the target word.
        dictionary: collections.abc.Container with all
            strings that are known to be actual words.
    Returns:
        List of anagrams that were found. Empty if
        none were found.
    """
    permutations = itertools.permutations(word, len(word))
    possible = (''.join(x) for x in permutations)
    found = {word for word in possible if word in dictionary}
    return list(found)

```
日本語訳についても並べてみます。

```python
def find_anagrams(word, dictionary):
    """単語の全アナグラムを見つける。
    
    この関数はdictionaryコンテナのメンバー検査の速度で実行する。
    
    引数:
        word: 単語の文字列
        dictionaly: 単語として分かっている全文字列を含む
    戻り値:
        得られたアナグラムのリスト。なければ空
    """
  .
  .
  .
```


## （２）PEP257を参考にする


# おわりに


---
## MEMO
【参考ページ】
- [PEP 257 — Docstring Conventions | Python.org](https://www.python.org/dev/peps/pep-0257/)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---