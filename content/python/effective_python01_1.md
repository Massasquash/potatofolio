---
title: "[Effective Python]項目1 使用するPythonのバージョンを知っておく"
date: 2021-01-21T09:26:50+09:00
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "python"
---

# はじめに
この記事は「Effective Python 第二版」を学習していく備忘録です。

## 項目1 使用するPythonのバージョンを知っておく
Python3.8が2019年10月にリリースされてから、多くの構文が導入されました。
Python2は2020年1月にサポートがなくなり、さらに2020年4月に使用期限が終わり公式な保守が行われなくなりました。自己責任で使うことはできますが、Python2の利用は避けましょう。
Python3をプロジェクトに使っていきましょう。

### 解説
使用しているPythonのバージョンを調べる方法は
（１）コマンドラインで調べる方法
（２）Python実行時（Jupyter Notebookのセル上で）調べる方法

（１）の方法は、ターミナルで
```bash
$ python --version  #-> Python 2.7.10
$ python3 --version #-> Python 3.8.0
```

（２）
```python
import sys
print(sys.version_info)
print(sys.version)
```

### 感想
言語自体もプログラミングで作られています。
Pythonはオープンソースなので、裏側でコミュニティの皆さんが保守管理してくれているため、僕らは安心してPythonでプログラミングを書くことができています。
よりよくなるようにバグの改善、セキュリティ向上を続けているので、しっかりと「サポートが受けられているバージョン」をプロジェクトに使っていくのが良さそうです。


---
## MEMO
### 参考書籍
- [O’Reilly Japan - Effective Python 第2版](https://www.oreilly.co.jp/books/9784873119175/)

### 参考記事
- こちらも合わせて参考にさせていただいています。
[『Effective Python 第2版』 第1章＜Pythonic思考＞ - Qiita](https://qiita.com/takadowa/items/44db07e227a58287b193)
---