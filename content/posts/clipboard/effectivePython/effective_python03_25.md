---
title: "[Effective Python]25 キーワード専用引数・位置専用引数の活用"
date: 2021-01-01T03:25
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**3章 関数**の項目25について学びました。


## 項目25 キーワード専用引数と位置専用引数で明確さを高める
関数呼び出しの際に、仮引数名を指定して渡す実引数を **「キーワード引数」**、指定しないで渡す実引数を **「位置引数」** と呼びます。  
Python関数の引数のデフォルトはこのどちらの方式でも呼び出すことができますが、関数定義時に **「キーワード専用引数」** **「位置専用引数」** として呼び出し方を指定してしまう方法があります。  
これを活用すると関数呼び出し時の書き方が統一されるので、読みやすいコードにすることができます。  

また、`/`と`*`に挟まれた引数はどちらの呼び出し方でも呼び出せます（Pythonの関数引数のデフォルト）。

## 解説
関数定義の時の仮引数に、`*`や`/`の記号を使って指定することができます。

|  表記  |  解説  |  構文例  |
| :---: | :---: | :---: |
|  * |  以降がキーワード専用引数になる  | `def(page, *, ignore_error=False):` |
|  /  |  その前までが位置専用引数になる  | `def(x, y, /):` |


**使用例**  
[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_25.py)を記載します。  
この関数定義の部分に、この記法が使われています。

```python
# Example 13
def safe_division_d(numerator, denominator, /, *,  # Changed
                    ignore_overflow=False,
                    ignore_zero_division=False):
    try:
        return numerator / denominator
    except OverflowError:
        if ignore_overflow:
            return 0
        else:
            raise
    except ZeroDivisionError:
        if ignore_zero_division:
            return float('inf')
        else:
            raise
```


### （１）呼び出し方の違い
仮引数に`*`を使うと、それ以降が **「キーワード専用引数」** になり、呼び出し時にキーワードを指定しないとエラーが出て呼び出せなくなります。  
OK -> `safe_division_d(2, 5, ignore_overflow=True, ignore_zero_division=True)`  
NG -> `safe_division_d(2, 5, True, True)`

仮引数に`/`を使うと、その前までが **「位置専用引数」** になり、呼び出し時にキーワード指定するとエラーが発生します。  
OK -> `safe_division_d(2, 5)`  
NG -> `safe_division_d(numerator=2, denominator=5)`


### （２）それぞれのメリット
**キーワード専用引数が有効な例**  
上の例のようにブール値がオプションとして並ぶ場合、呼び出し時にキーワードで指定する方が何の意味を表しているのかが一目でわかります。

**位置専用引数が有効な例**  
のちに呼び出す関数の仮引数名を変更したくなることがあるかと思います。  
呼び出し時にキーワードを使えないようにするだけで、仮引数名を変えても呼び出し時に影響が出ないので、メンテナンスが楽になります。  


### 感想
この表記はシンプルで活用しやすそうだな、と言う印象でした。関数の引数まわりの用語に関してはプログラミングに慣れていないうちはすごく複雑に感じるもので、

- 様々な用語「仮引数」「実引数」「位置引数」「キーワード引数」「可変長引数（位置・キーワード）」
- 記述の仕方として「デフォルト値」「アノテーション」
- 記号や慣例としての`*`, `/`, `*arg`, `**kwargs`

色々と混乱しそうですが、関数の基礎の使い方を覚えた後は用語や記号を整理をして、書かれているコードを読み解けるようになると関数の活用度がグッと上がりそうです。

---
## MEMO
【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)

【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
- Python実践入門
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>
　
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=B0842JDVBZ&linkId=25d949cbd1c5fb4187836e2a7ab30cb3&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---