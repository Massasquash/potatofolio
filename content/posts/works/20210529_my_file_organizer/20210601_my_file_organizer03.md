---
title: "[ファイル整理ツール03]ファイルのリネーム処理を行う2[Python]"
date: 2021-06-01
lead: "テスト用フォルダと本番用フォルダの切り替え"
categories: 
    - "Works"
tags: ["ツール制作",  "Python"]
---

## はじめに
前回に引き続き、イシュー#1を続けていきます。  

https://massasquash.github.io/potatofolio/posts/works/20210529_my_file_organizer/20210531_my_file_organizer02/

[リネーム処理の関数作成 · Issue #1 · Massasquash/MyFileOrganizer · GitHub](https://github.com/Massasquash/MyFileOrganizer/issues/1)

## 1. メイン処理のforループ
メイン処理に修正を加えます。  
`IN_PATHS`の中身が増えても全てのフォルダに対して実行できるように、`for in`で回すように変更します。  

変数名`in_folder`はそのまま使うため、特に中身の変更点はなかったです。

実行し、テスト用のフォルダで問題なく動作するかを確認しました。


## 2. 本番用のフォルダ
本番用のフォルダパスを用意します

開発を楽にするために、テスト用のフォルダと本番用のフォルダをそれぞれ残しておき、コメントアウトで切替する、というような書き方をとってみます。  
変数名を同じにすることで中のコードに変更を加えなくても良いようにするためです。  
もっとスマートなやり方があるかもしれません。


```python
# test： テスト用のフォルダ
# IN_PATHS = [
#     hogehoge,
#     hugahuga
# ]

# prod： 本番用のフォルダ
IN_PATHS = [
    hogehoge_prod,
    hugahuga_prod
]
```

実行し、本番用のフォルダでもリネームされるかどうかをチェック。  

ファイルにプログラムで一気に手を加えるので、誤って削除されてしまったりすると面倒なことになります。念のためフォルダのコピーをとってから実行確認を行います（dropboxなら元に戻せるのであまり心配はいりませんが）。


問題も起こらずうまくいきました。  


## おわりに
今回はシンプルに。イシュー#1 完了しました。

### MEMO
今回の実装  
[Task: 複数のIN_PATHSへの対応・本番用フォルダ追加 · Massasquash/MyFileOrganizer@f5df0b1 · GitHub](https://github.com/Massasquash/MyFileOrganizer/commit/f5df0b17adcc38b40c778b4c7cc7f1d0296a6de8)

次回の実装  
[移動処理（同フォルダ）の関数作成 · Issue #2 · Massasquash/MyFileOrganizer · GitHub](https://github.com/Massasquash/MyFileOrganizer/issues/2)

### GitHub
[GitHub - Massasquash/MyFileOrganizer](https://github.com/Massasquash/MyFileOrganizer)