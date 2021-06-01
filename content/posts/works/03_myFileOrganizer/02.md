---
title: "[ファイル整理ツール02]ファイルのリネーム処理を行う[Python]"
date: 2021-05-31
lead: "pathlib、datetime、ファイル作成日8桁を頭につける"
categories:
    - "Works"
tags: ["ツール制作",  "Python"]
---

## はじめに
こちらのイシューを進めます。

[リネーム処理の関数作成 · Issue #1 · Massasquash/MyFileOrganizer · GitHub](https://github.com/Massasquash/MyFileOrganizer/issues/1)


## 1. ファイル名の頭に日付8桁を付与する関数
第一引数に与えたパス名を変更する関数です。  
作成日の日付8桁（例：20210531_）をファイル名の頭に加えます。  

パスがフォルダの場合は、中身はいじらずそのフォルダ名のみ変更します。  
戻り値には変更後のパスを指定しておきました（たぶん使わないけど次の関数と合わせるため）。  

```python
# 必要なインポート
from datetime import datetime
from pathlib import Path


def rename_path_add_prefix(path, prefix_format='%Y%m%d'):
    """リネーム処理：ファイル名の頭に日付8桁を付与する関数

    Args:
        path(pathlib.Path): インプットフォルダのパスオブジェクト
        prefix_format(:obj: `str`, optional): 付与したい接頭辞のフォーマット（datetime.strftime()の引数に渡す）
    Returns:
        new_path(pathlib.Path): リネーム後のパスオブジェクト
    """
    birthtime_epoc = content.stat().st_birthtime
    birthtime = datetime.fromtimestamp(birthtime_epoc)

    prefix = birthtime.strftime(prefix_format)
    new_path = content.parent / f'{prefix}_{content.name}'

    path.rename(new_path)

    return new_path
```

### Path.stat()
`パスオブジェクト.stat()`は、そのパスオブジェクトの作成日時や更新日時などのタイムスタンプ情報を調べることができるメソッドです。

こんな感じで情報を得られます。  

```
os.stat_result(st_mode=33188, st_ino=942852, st_dev=16777223, st_nlink=1, st_uid=501, st_gid=20, st_size=974565, st_atime=1620471805, st_mtime=1617714680, st_ctime=1620471803)
```

属性にはこんな意味があるようです。 
日時は全てエポックタイムになっているので注意。

| TH | TH |
| :--- | :--- |
| st_atime | 最終アクセス日時 |
| st_mtime | 最終内容更新日時 |
| st_ctime | Macではメタデータの最終更新日時／ Windowsでは作成日時 |
| st_birthtime | Macでは作成日時 |

上記はテストファイルで適当な環境で出したのですが`st_birthtime`がありませんでした。環境によって違うそうです。 

### datetime.fromtimestamp()
エポックタイムとして得たファイルの作成日時を、`datetime.fromtimestamp(エポックタイム)`でdatetimeオブジェクトに変換しています。  


## 2. ファイル名の頭の日付6桁を削除する関数
この関数は本来なら不要なのですが、以前日付6桁をファイル名の先頭に加えたデータを作ってしまっていたので、それを日付8桁に揃えるために作りました。
（6桁にした方がファイル名が短くなって良いかと思ったのですが、どうも8桁の方が運用しやすかった）

汎用性を高めるために、頭の日付を削除する処理だけを行う関数としました。  
戻り値として変更後のパスオブジェクトを返すようにすることで、メイン処理の方で続けて`rename_path_add_prefix()`関数に渡せるようにしています

```python
def rename_path_remove_prefix(path, prefix_nums=7):
    """リネーム処理：ファイル名の頭の日付6桁を削除する関数

    Args:
        path(pathlib.Path): インプットフォルダのパスオブジェクト
        prefix_regexp(:obj: `int`, optional): 削除したい接頭辞の文字数（デフォルトは7:日付6桁＋アンスコ）
    Returns:
        new_path(pathlib.Path): リネーム後のパスオブジェクト
    """
    new_path = path.parent / path.name[prefix_nums:]

    path.rename(new_path)

    return new_path
```


## 3. メイン処理
最後に必要な処理。

```python
# 必要なインポート
import re

# インプットフォルダ
IN_PATHS = [
    Path(r'/Users/massa/Dropbox/Lab/20210530_MyFileOrganizer/test_in_folder')    
]

# メイン処理

# IN_PATHSの0つ目について処理を行う
# [TODO] forループでIN_PATHS内を回す
in_folder = IN_PATHS[0]
contents = in_folder.iterdir()

for content in contents:
    name = content.name

    if name == '.DS_Store':
        continue

    # ファイル名の先頭が数値8桁の場合
    if re.compile(r'^(\d{8}_)').search(name):
        continue
    # ファイル名の先頭が数値6桁の場合
    elif re.compile(r'^(\d{6}_)').search(name):
        content = rename_path_remove_prefix(content)
    rename_path_add_prefix(content)
```

 

## おわりに
久々にコードを書いていろいろ手間取りました。  
このツール制作過程のログを書くのも少し時間がかかってしまっているので、もう少しさくっと書いていきたいです。  
  
ちなみに今回のコード、`rename_path_remove_prefix()`と`rename_path_remove_prefix()`を対になるような感じで書けたのが個人的に良かった。  


### MEMO
今回の実装
[Feat: インプットフォルダ1つに対してリネーム処理を行う · Massasquash/MyFileOrganizer@319ff73 · GitHub](https://github.com/Massasquash/MyFileOrganizer/commit/319ff7358dc6ff5dca42daefbc64e57a69833010)

次回の実装
- DS.storeをgitignoreに追加

### GitHub
[GitHub - Massasquash/MyFileOrganizer](https://github.com/Massasquash/MyFileOrganizer)