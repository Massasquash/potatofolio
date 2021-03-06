---
title: "[Effective Python]00 目次"
date: 2021-01-01T00:30
lead: "「Effective python第２版」の学習備忘録"
categories:
  - "Python「Effective Python 2th」"
---

# はじめに
この一連の記事は「Effective Python 第二版」の学習の備忘録です。  
学習した内容について、わかりづらかった点を整理したり補足事項をまとめたりして学習しています。  

**１章 Pythonic思考**  
>Pythonコミュニティでは、特定のスタイルに沿ったコードを表すのにPythonicという形容詞を使います。Pythonのイディオムは、この言語を使い仲間と作業する経験から時間をかけて発展してきました。１章では、Pythonで最も中心的な共通的に使われる最良の方法を扱います。（まえがき xii より引用）

（目次）
- [項目1 使用するPythonのバージョンを知っておく](https://massasquash.github.io/potatofolio/python/effective_python01_1/)  
- [項目2 PEP 8スタイルガイドに従う](https://massasquash.github.io/potatofolio/python/effective_python01_2/)  
- [項目3 bytesとstrの違いを知っておく](https://massasquash.github.io/potatofolio/python/effective_python01_3/)  
- [項目4 Cスタイルフォーマット文字列とstr.formatは使わずf 文字列で埋め込む](https://massasquash.github.io/potatofolio/python/effective_python01_4/)  
- [項目5 複雑な式の代わりにヘルパー関数を書く](https://massasquash.github.io/potatofolio/python/effective_python01_5/)  
- [項目6 インデックスではなく複数代入アンパックを使う](https://massasquash.github.io/potatofolio/python/effective_python01_6/)  
- [項目7 rangeではなくenumerateを使う](https://massasquash.github.io/potatofolio/python/effective_python01_7-8/)  
- [項目8 イテレータを並列に処理するにはzipを使う](https://massasquash.github.io/potatofolio/python/effective_python01_7-8/)  
- [項目9 forループとwhileループの後のelseブロックは使わない](https://massasquash.github.io/potatofolio/python/effective_python01_9/)
- [項目10 代入式で繰り返しを防ぐ](https://massasquash.github.io/potatofolio/python/effective_python01_10/)  


**2章 リストと辞書**  
> Pythonで情報を扱う場合に、最も一般的なのはlistに格納されたデータのシーケンスです。リストと対応する値をキーに対して与える辞書（dict）とは補完し合います。2章では、こういった有用なプログラミング要素を使ってどのようにプログラムを構成すると良いかを述べます。（まえがき xii より引用）

（目次）  
- 項目11 シーケンスをどのようにスライスするか知っておく  
- 項目12 1つの式では、ストライドとスライスを同時に使わない  
- 項目13 スライスではなくcatch-allアンパックを使う  
- 項目14 key引数を使い複雑な基準でソートする  
- 項目15 dictの挿入順序に依存する場合は注意する  
- 項目16 辞書の欠損キーの処理にはinやKeyErrorではなくgetを使う  
- 項目17 内部状態の欠損要素を扱うにはsetdefaultではなくdefaultdictを使う  
- 項目18 _ _missing_ _ でキー依存デフォルト値を作成する方法を把握しておく  


**3章 関数**  
>Pythonの関数には、プログラマを助けるためのさまざまな機能があります。一部は、他のプログラミング言語にもある機能ですが、多くはPythonに特有のものです。3章では、関数をどのように使えば、プログラムの意図が明確になり、再利用を促進し、バグを減らせるかを示します。（まえがき xii より引用）

（目次）  
- 項目19 複数の戻り値では、4個以上の変数なら決してアンパックしない  
- 項目20 Noneを返すのではなく例外を送出する  
- 項目21 クロージャが変数スコープとどう関わるかを把握しておく  
- 項目22 可変長位置引数を使って、見た目をすっきりさせる  
- 項目23 キーワード引数にオプションの振る舞いを与える  
- 項目24 動的なデフォルト引数を指定するときにはNoneとdocstringを使う  
- [項目25 キーワード専用引数と位置専用引数で明確さを高める](https://massasquash.github.io/potatofolio/python/effective_python03_25/)  
- [項目26 functools.wrapsを使って関数デコレータを定義する](https://massasquash.github.io/potatofolio/python/effective_python03_26/)  


**4章 内包表記とジェネレータ**  
>Pythonには、リスト、辞書、集合の要素を簡単にイテレーションする特別な構文が用意されていて、派生的なデータ構造が作れます。また、列挙可能なストリームから値を取り出し、1つずつ返す関数も作れます。4章では、これらの機能を使い、性能向上、メモリ使用量削減、読みやすさの改善をどのようにして達成するかを述べます。（まえがき xii より引用）

（目次）  
- [項目27 mapやfilterの代わりにリスト内包表記を使う](https://massasquash.github.io/potatofolio/python/effective_python04_27-28/)  
- [項目28 内包表記では、3つ以上の式を避ける](https://massasquash.github.io/potatofolio/python/effective_python04_27-28/)  
- 項目29 代入式を使い内包表記での繰り返し作業をなくす  
- [項目30 リストを返さずにジェネレータを返すことを考える](https://massasquash.github.io/potatofolio/python/effective_python04_30-32/)  
- 項目31 引数に対してイテレータを使うときには確実さを優先する  
- [項目32 大きなリスト内包表記にはジェネレータ式を考える](https://massasquash.github.io/potatofolio/python/effective_python04_30-32/)  
- 項目33 yield fromで複数のジェネレータを作る  
- 項目34 sendでジェネレータにデータを注入するのは避ける  
- 項目35 ジェネレータでthrowによる状態遷移を起こすのは避ける  
- 項目36 イテレータとジェネレータの作業ではitertoolsを使う  


**5章 クラスと継承**  
>Pythonはオブジェクト指向言語です。Pythonで作業するには、新たなクラスを書いて、そのインタフェースと階層とを介してどのように相互作用するかを定義する必要がよくあります。5章では、クラスをどのように使ってオブジェクトの意図した振る舞いを表現するかを扱います。

（目次）  
- 項目37 組み込み型の深い入れ子にはせずクラスを作成する  
- 項目38 単純なインタフェースにはクラスの代わりに関数を使う  
- 項目39 @classmethodポリモルフィズムを使ってオブジェクトをジェネリックに構築する  
- 項目40 superを使ってスーパークラスを初期化する  
- 項目41 Mix-inクラスで機能合成を考える  
- 項目42 プライベート属性よりパブリックな属性が好ましい  
- 項目43 カスタムコンテナ型はcollections.abcを継承する  

**6章 メタクラスと属性**  
>メタクラスと動的属性とは、Pythonの強力な機能です。ところが、これらは、予期してもいなかった信じられないような振る舞いを実装することにもなりかねません。6章では、これらの仕組みを使ってプログラマの意図した通りに実行されることを確実にする、よく知られたイディオムを扱います。（まえがき xii より引用）

（目次）  
- 項目44 getメソッドやsetメソッドは使わず属性をそのまま使う  
- 項目45 属性をリファクタリングする代わりに@propertyを考える  
- 項目46 再利用可能な@propertyメソッドにディスクリプタを使う  
- 項目47 遅延属性には_ _getattr_ _, _ _getattribute_ _, _ _setattr_ _ を使う  
- 項目48 サブクラスを_ _init_subclass_ _ で検証する  
- 項目49 クラスの存在を_ _init_subclass_ _ で登録する  
- 項目50 クラス属性に_ _set_name_ _ で注釈を加える  
- 項目51 合成可能なクラス拡張のためにはメタクラスではなくクラスデコレータを使う  


**7章 並行性と並列性**  
同じ時間内に多くの異なることを実行する並行プログラムを、Pythonでは簡単に書くことができます。Pythonでは、システムコール、サブプロセス、C拡張によって並列作業を行うこともできます。7章では、このようなさまざまな状況下でどのようにすればPythonを最もうまく活用できるかを扱います。

**8章 頑健性と性能**  
Pythonには、プログラムを強化するための組み込みの機能とモジュールがあり、ディペンダブルです。さらに、最小限の手間で高性能を達成するのに役立つツール類もあります。8章では、プログラムの読みやすさとプロダクションでの効率を最大化するためにPythonをどのように使えばよいかを述べます。

**9章 テストとデバッグ**  
どのような言語を使っていてもコードは常にテストしなければなりません。Pythonの動的な性質によって、実行時エラーが増える危険があります。しかし、この動的な機能を使って、テストを書いたりプログラムの間違いを診断したりすることが簡単になります。9章では、テストとデバッグ用のPython組み込みツールを扱います。

**10章 協働作業（コラボレーション）**  
Pythonプログラムで協働作業を行うには、コードをどのように書くかについて、よく考えておかねばなりません。1人で作業している場合でも、他の人が書いたモジュールをどのように使うとよいか理解する必要があります。10章では、Pythonプログラムで一緒に作業できるようにする標準的なツールやベストプラクティスを扱います。


---
## MEMO
【書籍】
- Effective Python 第2版 ―Pythonプログラムを改良する90項目
<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=qf_sp_asin_til&t=massasquash08-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4873119170&linkId=b01ad363c615cc9408dfcc360b1a85de&bc1=ffffff&amp;lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr"></iframe>

---