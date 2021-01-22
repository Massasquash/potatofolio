---
title: "[Effective Python]項目25:キーワード専用引数と位置専用引数で明確さを高める"
date: 2021-01-21T09:26:50+09:00
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Effective Python"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。

**3章 関数**  
>Pythonの関数には、プログラマを助けるためのさまざまな機能があります。一部は、他のプログラミング言語にもある機能ですが、多くはPythonに特有のものです。3章では、関数をどのように使えば、プログラムの意図が明確になり、再利用を促進し、バグを減らせるかを示します。（まえがき xii より引用）

（目次）
- 項目19 複数の戻り値では、4個以上の変数なら決してアンパックしない
- 項目20 Noneを返すのではなく例外を送出する
- 項目21 クロージャが変数スコープとどう関わるかを把握しておく
- 項目22 可変長位置引数を使って、見た目をすっきりさせる
- 項目23 キーワード引数にオプションの振る舞いを与える
- 項目24 動的なデフォルト引数を指定するときにはNoneとdocstringを使う
- **項目25 キーワード専用引数と位置専用引数で明確さを高める**
- 項目26 functools.wrapsを使って関数デコレータを定義する



## 項目25:キーワード専用引数と位置専用引数で明確さを高める
関数呼び出しの際に、仮引数名を指定して渡す実引数を「キーワード引数」、指定しないで渡す実引数を「位置引数」と呼びます。  
Python関数の引数のデフォルトはこのどちらの方式でも呼び出すことができますが、関数定義時に「キーワード専用引数」「位置専用引数」として呼び出し方を指定してしまう方法があります。  
これを活用すると関数呼び出し時の書き方が統一されるので、読みやすいコードにすることができます。  

また、`/`と`*`に挟まれた引数はどちらの呼び出し方でも呼び出せます（Pythonの関数引数のデフォルト）。

### 解説
関数定義の時の仮引数に、`*`や`/`の記号を使って指定することができます。

|  表記  |  解説  |  構文例  |
| :---: | :---: | :---: |
|  * |  以降がキーワード専用引数になる  | `def(page, *, ignore_error=False):` |
|  /  |  その前までが位置専用引数になる  | `def(x, y, /):` |


**使用例**  
[サンプルコード](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_25.py)を記載します。

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

この関数定義の部分に使っています。

```python
def safe_division_d(
  numerator, denominator, /, *,
  ignore_overflow=False,
  ignore_zero_division=False):
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
この表記はシンプルで活用しやすそうだな、と言う印象でした。関数の引数に関しては初心者にはすごく複雑で、

- 様々な用語「仮引数」「実引数」「位置引数」「キーワード引数」「可変長引数（位置・キーワード）」
- 記述の仕方として「デフォルト値」「アノテーション」
- 記号や慣例としての`*`, `/`, `*arg`, `**kwargs`

などあって混乱しそうですが、基礎の使い方を覚えた後は用語を整理をして、色々な書き方を読み解けるようになると関数の活用度がグッと上がりそうです。

---
## MEMO
【参考書籍】
- [O’Reilly Japan - Effective Python 第2版](https://www.oreilly.co.jp/books/9784873119175/)
- [Python実践入門](https://www.amazon.co.jp/Python%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80-%E8%A8%80%E8%AA%9E%E3%81%AE%E5%8A%9B%E3%82%92%E5%BC%95%E3%81%8D%E5%87%BA%E3%81%97%E3%80%81%E9%96%8B%E7%99%BA%E5%8A%B9%E7%8E%87%E3%82%92%E9%AB%98%E3%82%81%E3%82%8B-WEB-PRESS-plus-ebook/dp/B0842JDVBZ)

【Effective Pythonサンプルコード】
- [GitHub - bslatkin/effectivepython: Effective Python: Second Edition — Source Code and Errata for the Book](https://github.com/bslatkin/effectivepython)
---