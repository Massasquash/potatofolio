---
title: "[Effective Python]01 Pythonのバージョンについて"
date: 2021-01-01T01:01
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この記事は「Effective Python 第二版」の学習の備忘録です。
今回は**１章 Pythonic思考**の項目1について学びました。


## 項目1 使用するPythonのバージョンを知っておく
Python3.8が2019年10月にリリースされ、多くの便利な構文が導入されました。  
またPython2は2020年1月にサポートがなくなり、さらに2020年4月に使用期限が終わり公式な保守が行われなくなりました。  
自己責任で使うことはできますが、Python2の利用は避け、Python3をプロジェクトに使っていくのが安全です。そのために、自分のPCにどのバージョンがインストールされているのかを確認する方法が整理されているのがこの項目の内容でした。

## 解説
使用しているPythonのバージョンを調べるには、以下の方法があります。
- （１）コマンドラインで調べる方法
- （２）Python実行時（対話モードやJupyter Notebookのセル上などで）調べる方法

それぞれの方法について具体的に見ていきます。

### （１）コマンドラインでバージョンを調べる方法
まずはターミナルなどのコマンドライン確認する方法。自分のPCにインストールしているPythonのバージョンを調べられます。  
`python`コマンドと`python3`コマンドでは結果が違い、インストール状況によって変わってきます。  
例えばPython3をインストールしていない場合は、python3コマンドは動きません。

```bash
$ python --version  #-> Python 2.7.10
$ python3 --version #-> Python 3.8.0
```


### （２）Python実行時にバージョンを調べる方法
次にPythonスクリプト上でバージョンを確認する方法です。組み込み関数`sys`を使います。  
ここではJupyter Notebook上で確認することを想定していますが、ターミナルで`python3`（もしくは`python`）とコマンドを打ち、対話モードにした状態で確認することもできます。

```python
import sys
print(sys.version_info)
# -> sys.version_info(major=3, minor=8, micro=6, releaselevel='final', serial=0)
print(sys.version)
# ->3.8.6 (default, Oct  8 2020, 14:07:53) 
```

## 感想
プログラミング自体を学習しはじめの頃は、プログラミング言語にバージョンがあることや、PCに複数のバージョンが同時に入っていること、という自体のイメージがしづらいかもしれません（自分はそうでした）。  
「プログラミング言語もソフトウェアの一種」という考え方をすると少しイメージしやすいかなと思います。  
ソフトウェアは日々更新されてより良いバージョンが出てきたり、古いバージョンのソフトウェアを使い続けることができるのと同じように、Python言語もにもバージョンがあり日々進化を続けています。

プログラミング言語自体も、その中身は誰かがプログラミング言語で書いて作ったものです。  
Pythonはオープンソースで作られている言語なので、裏側でコミュニティの皆さんが保守管理してくれているため、僕らは安心してコードを書いてプログラミングをすることができています。  
より良くなるようにバグの改善・セキュリティ向上など改善を続けているので、しっかりと「サポートが受けられているバージョン」をプロジェクトに使っていくのが良さそうです。

---
## MEMO
【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---

