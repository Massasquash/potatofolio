---
title: "GoogleDriveの画像ファイルを直接ダウンロードするためのURL"
date: 2021-04-05
lead: ""
categories:
  - "GoogleWorkspace"
draft: true
---

## はじめに
IFTTTでGoogleDriveとDropboxを繋げて、
「GoogleDriveに画像ファイルがアップロードされたらDropboxに保存する」
というような連携をしている途中で発見したのでメモ。


## 
### 1. 画像ファイルへの直リンクURL
Driveファイルのダウンロードリンクはこんな感じらしい
`https://drive.google.com/uc?id=1XxsqQwh_lH1S6UhfwUZM5PZo0uHGjq16&export=download`

普通のドライブに保存されているファイルのURLは
`https://drive.google.com/file/d/1gU_bx1acOCEhICfdjvwo7MmMD3-2uGQG/view`

という感じなので、`drive.google.com/`以下を`/uc?id=xxxxxxx&export=download`としてやると良い様子。

### 2. 画像ファイルのダウンロードURL

## おわりに

GoogleDriveのファイルは全てがURLで得られるため、ダウンロードリンクを直接指定できる方法がわかり、何かと便利だなーと思いました。





これは使えそう
