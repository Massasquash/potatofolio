---
title: "[Effective Python]10 代入式で繰り返しを防ぐ"
date: 2021-01-01T02:15
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
draft: true
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。  
今回は**2章 リストと辞書**の項目15について学びました。


## 項目15 dictの挿入順序に依存する場合は注意する
dict型のキー・バリューの順序についての項目です。  

Pythonのバージョンによってはいてレート

Python3.5以前ではdictが作成時の順番を

このような性質が前提にあります。

ところが、



## 解説
### （１）dict型のリテラルと表示
dict型はリテラルでは以下のように`{key: value}`の形で定義することができます

```python
my_dict = {key: value}

my_dict = {
  'name': 'Bob',
  'age': 30,
  'language': 'English' 
}
```

### (2)静的型付けと動的型付け
プログラミング言語の区分けとして **「静的型付け」**と **「動的型付け」**という２つのパターンがあります。  
変数や関数

- 静的型付け
  プログラマが変数や関数定義時に型を決めて扱う方法のこと

- 動的片付け
  プログラマが型を指定しなくても、コンパイラやインタプリタがプログラム実行時に想定して型を当てはめて使うこと




list, tupleはイテラブルなシーケンス型（順番がありインデックスで取り出せる）のに対して、dict型はイテラブルではありますがシーケンス型ではありません。  


## (3)ダックタイピング



## 感想


---
## MEMO
【参考ページ】


【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---