# Todoアプリ

## モチベーション

- この書籍はアプリの作り方を学ぶものではない
- インターフェイスに注目する
- コードの読み方を考える
- 具体的な説明をするが、アプリの作り方の詳細というよりはどのような原理でコンポーネントを考えるかというところに焦点を置く
- 新しいAPIが大量にでてくるため丁寧に

## 目的

- オブジェクト指向なアプリケーション開発
- クラスの使い方
- 状態をもつアプリ
- モジュールの使い方
    - 理解し易い構造を作ること
    - スケールするためにモジュール化する
- イベント駆動/ライフサイクル
    - DOMイベントと自作イベント
- 入力に対する反応としての反応
    - イベント
    - 描画の更新

## やらないこと

- タイトルの更新
- フィルター

## 守るべきルール

- セクション間で出したコードはそれぞれ独立して動作すること

## 作るもの

- シンプルなTodoアプリ

### できること

- Todoアイテムの追加
- Todoアイテムの削除
- Todoアイテムの更新（✓）

## イベント

### DOM

- load
- unload
- submit
- click
- change

### アプリ

- Change
    - Add
    - Remove
    - Update

## 状態

- TODOリスト
- Todoアイテム
    - completed
  
## アウトライン

- 目的: モジュールなアプリを開発する
- 目的: イベント駆動を理解する
    - ブラウザ、アプリ
- なぜTodoアプリ
    - 実際にウェブアプリを開発する際にはさまざまな機能が実装されたライブラリやある種のアプリ開発の仕組みをサポートするフレームワークを使うことが現実的です
    - 特にウェブアプリ開発の際にそのスタイルを決めるのは表示を管理するViewのライブラリです
    - ここで実装するTodoアプリと呼ばれるシンプルなタスク管理アプリは、どのViewライブラリでもサンプルとして提供されています
    - TodoMVCというTodoアプリのリファレンス実装があり、各ライブラリはこのTodoMVCを実装したものをサンプルとして提供しています
    - これはユーザーからすれば各ライブラリの書き味を確かめることができ、ライブラリ作者からすればユーザーに慣れ親しんだアプリをどうかけるのを示すサンプルとなります
    - ここで実装するTodoアプリはTodoMVCをもっと簡素化したものですが、このアプリの作り方を理解すれば実際にライブラリを使ったアプリの開発に役立つでしょう
- エントリポイント
    - 目的: ローカルサーバでJavaScriptモジュールを読み込める
    - JavaScriptのエントリポイントについて
    - エントリポイントとしてのHTMLとJavaScript
    - JavaScriptモジュールのエントリポイントとしてindex.jsを用意する
    - 他のJavaScriptモジュールはindex.jsから読み込んで実行する
    - スコープが異なるとアクセスできないので
- Todoアプリの構成
    - 目的: Todoアプリを構成するHTML, CSS、 JSの役割を見ていく
    - HTMLとCSSを配置して見た目だけは完成させる。
    - Todoアプリを実行するブラウザにはいわゆるiOSやAndroidのようにOSが提供するようなフレームワークがデフォルトにはありません
    - そのためさまざまなユーザーが実装したライブラリやフレームワークがあり、それを使ってアプリを作るのが一般的です
    - これは１つのTodoアプリをとっても書き方がかなりの種類があることをしめしています
    - まずは全体像を示してみます
        - HTML: コンテンツの構造を記述するためのマークアップ言語
        - CSS: HTMLの見た目を装飾するスタイルシート言語
        - JS: インタラクションといった動作を扱うプログラミング言語
            - JavaScriptヵら動的にHTMLやCSSを生成、操作できる
    - HTMLは静的です。動的な部分をJavaScriptで管理し構築します
    - CSSは見た目を変更する
    - JSは動作を扱います。動作は幅広い概念ですが、ここではTodoリストにTodoを追加したり、Todoのステータスを更新したり、Todoを削除したりといったことです。
    - 今回のTodoアプリはざっくりとした静的な構造と動的な部分を明確に区別したものHTMLで最初に定義します。
    - そして、動的な部分をJavaScriptから扱い、そこにTodoリストを描画したり、ユーザー操作で更新するなどのアプリとしての機能を実装しています。
    - それではまずコンテンツの構造を決めるHTMLから書いていきます。
    - HTMLは次のようにして動的な部分には`<!-- コメント -->`を入れています。
    - 後からこの動的な部分`id=js-app`とついている部分をJavaScriptから操作していきます。
    - `id`属性に`js-` というプロフィックスをついていることからも分かるように、JavaScriptから扱う部分には`js-`プロフィックスを付けています。
    - `id`属性は同じページに1つしか存在できないため、この属性はJavaScriptから操作するためにつけているんだというのを分かりやすくするためです。
    - また各所で`class`属性を付けていることが分かります。
    - これはCSSからこの要素の見た目を指定ために付けているクラス属性です（JavaScriptの`class`とは関係ありません）
    - そしてCSSは`<link rel>`という`link`要素で外部から読み込むことができます。
    - ここでは既に本書で用意済みのCSSをウェブサイトからロードするようにしています。
    - 興味がある人は直接 URL からダウンロードして、同じディレクトリに `index.css` として作成し、`<link>`要素で読み込んでみてください。
    - JavaScriptにおける`script src`と似たようものです。
    - スタイルに関しても`style`タグで直接要素内にスタイルを書けますが、一般的にCSSも外部ファイルとして定義して読み込んで利用する形を取ります。
- formイベントから学ぶイベント
    - TODOアプリでは次のような動作をJavaScriptで実装する必要があります。
    - まずはTodoの追加を考えみましょう。
    - Todoの追加は、form要素の子要素であるinput要素に入力し、form要素が`submit`されたイベントを受け取ります、
    - input要素に入力された内容をタイトルにしたTodoアイテムを作成し、Todoリストにアイテムを追加します
- Todoアプリの動作
    - ここからJavaScriptを使いTodoアプリを作成していきます。
    - ここで作成するTodoアプリの機能としては次のものを作成していきます。
        - Todoアイテムを追加する
        - Todoアイテムを更新する
        - Todoアイテムを削除する
    - 例えば、ユーザーがTodoのタイトルを入力してEnterを押したらTodoリストに**Todoを追加する**といったような機能をJavaScriptで実装しています。
    - 基本的にはユーザーが〜したら、JavaScriptで処理を行い、Todoアプリの表示が更新されるということを実装します。
    - このように（ユーザーが）〜したらといったことをイベントと呼び、多くのウェブアプリケーションはイベントが発生したら何かを処理するという形で動作します。
    - このイベントが元に動作を進めていく考え方をイベント駆動と呼び、Todoアプリもこの考え方に乗っ取り実装しています。
    - イベントはブラウザ側が定義しているDOMイベントと自分で定義するイベントがあります。
    - まずはDOMイベントとしてどのようなものがあり、DOMイベントの扱い方について見ていきましょう。
    - submitを例にリストにTodoアイテムを追加してみる
        - submitイベントの受け取り
        - 入力内容の取得
        - コンソールに表示
        - 実際に実装
    - これと同様に実装する機能がどのように実現されるかを考えてみましょう
        - Todoアイテムを追加する: フォームから送信する -> Todoリストにアイテムを追加しする
        - Todoアイテムを更新する: チェックボックスをクリックする -> Todoリスト表示を更新する
        - Todoアイテムを削除する: 削除ボタンを押す  ->  Todoリスト表示を更新する
- イベントとモデル
    - 先ほどの方法ではイベントの発火という入力に対して、表示の更新という出力が1対１でおこなわれていました
    - そのため、Todoアイテムを更新するにもTodoアイテムというデータはDOM上にしか存在しないことになります
    - この場合、DOM要素にTodoアイテムの情報(タイトルや識別子となるidなど)をすべて埋め込む必要があります
    - DOM要素は文字列でしか情報を入れることができないため、Todoアイテムというデータを表現するのに制限がかかります。
    - この問題を避けるために、Todoアイテムという情報をただのJavaScriptオブジェクトまたはクラスとして表現して、
    - そのオブジェクトを元に表示の更新を行うという段階を踏むことで柔軟性が生まれます。
    - ただのJavaScriptと表現するメリットは、ブラウザのAPIに依存せずにTodoListを表現することにあります。
    - これによってユニットテストなどもブラウザに依存せずにNode.jsでも検証することというメリットがあります
    - (一般的にブラウザは巨大なソフトウェアであるため起動時間などコストが掛かりやすいです)
    - 先ほどの処理をJavaScriptで表現したTodoList、ここではモデルと呼ぶ。
    - モデルを通して見てみましょう
        - [ ] Tableにする
        - フォームから送信する -> TodoリストへTodoアイテムを追加する -> Todoリストの表示を更新する
        - チェックボックスをクリックする -> TodoリストのTodoアイテムを更新する -> Todoリスト表示を更新する
        - 削除ボタンを押す -> TodoリストからTodoアイテムを削除する ->  Todoリスト表示を更新する
    - ここで表示についてを注目してみましょう
        - いずれもTodoリストが更新されたことで表示を更新するようになっています
        - つまり、Todoリスト自体が「自分が更新された」というイベントを発火すれば、
        - 表示側はそのイベントを受け取ってTodoリストの表示を更新すれば常にTodoリストと表示は同期できることが分かります。
        - これはオブザーブパターンとも言われる古典的なデザインパターンで、何を隠そうaddEventListnerもこのパターンに該当しています
        - しかし、addEventListenerはDOM APIなので
    - イベントという考え方はブラウザ(DOMに限らず)に、Node.jsでもEventEmitterという形でイベントを発火/受信する処理があります。
    - EventEmitterという名前でこのイベントの仕組みを行います
        - EventEmitterの実装
        - 解説と使い方
    - 実際にTodoListとTodoItemというモデルを作成して、先ほどのTodoアイテムの追加を実装し直してみます。
    - 先ほどの直接DOMを更新するのではなく、TOdoアイテムの追加というものをModelで表現して、そのModelをViewに対して描画するという形にします
    - Model/TodoList, Model/TodoItemの実装
    - App.jsでModelを使って描画し直してみましょう。
    - 以前とはことなり、状態はModelにあるため、一度すべてを消して現在の状態で作ったものを再描画すれば完成です
- 次に残りの更新と削除を実装していきます
- 目的: 最初に実装を完成させる、そのあとにViewをリファクタリングする
    - 残る機能は「Todoの状態を更新」と「Todoの削除」です。
    - Todoの更新
        - まずは現在のTodoの状態をチェックボックス使って表現してみましょう。
        - TODOの状態を表現するinputを追加
        - 次はこのinputをクリックしたときに、Todoの表示を更新してみましょう。
        - チェックボックスの更新を知るには`change`イベントが利用できます
        - チェックが`change`した現在の状態と反対の状態にする。いわゆるトグルします
        - そして完了済みのTodoアイテムは`<s>`で打ち消し線をつけましょう。
    - Todoの削除
        - 基本的な流れは更新処理と同じです。
        - deleteTodoを追加し、削除ボタンを追加
        - clickイベントをリッスンして、deleteTodoを行います
    - まとめ
        - これですべてのTodoの機能が実装できました。
        - Todoを追加、チェックを更新、削除できるようになっています
        - 最低限のTodoアプリとしての性質を実装できました
    - 実装後
    - `App.js`に表示の更新時の実装されています。しかし、App.jsの中でDOMを組みたてるとAPp.js自体がの行数が増えていくことがわかります。
    - これから削除ボタンを増やすとさらにApp.jsが膨れていきます。
    - 今回のTodoアプリならともかく、様々な要素がでてくるほどDOM – 表示に関するコード行は増えていきます。
    - このままで見通しが悪いApp.jsとなるため、表示 – Viewに関するコードをモジュールに分離していきましょう。
- Viewを分離する
    - 目的: App.jsが膨れるの防止する、View自体にはロジックを含めない
    - App.jsの役割はモデルと表示の中継を行う管理するクラスです。
    - view/というディレクトリを作成します
    - ここでは`TodoItemView`と`TodoListView`を作成していきます。
    - TodoListViewはTodoListItemViewを複数持ったHTML要素を作るモジュールです。
    - モデルはアプリの操作や状態もつのに対して、Viewは表示とイベントのリスナー関数を登録するだけです。
    - 簡単にいうとViewにはロジック（操作）に関する処理はなく、発生したイベントをModelに伝えるだけです。
    - Viewクラスは外から状態とDOMイベントが発火したときに呼び出す関数(イベントリスナー)を受け取ります
    - このようにすることで、Viewにはロジックがなくイベントが発火したら、特定の関数を呼ぶというイベントの仕組みがそのままです
    - これはDOM APIを使ったHTML要素をラップしたクラスを作るイメージに近いです。
    - 最後にApp.jsでViewとModelを紐づけている処理を整理してみましょう
    - 今まで実装してきた追加、更新、削除のリスナーとなるメソッドを作成し、それぞれのイベントリスナーからはこの処理を呼ぶようにしてみましょう。
    - これでApp.jsが追加と更新、削除を処理しているクラスということがわかりやすくなります。
- まとめ
    - これでModel、ViewとModelとViewを中継するAppという役割の分離をしたTodoアプリができました。
- Todoアプリのまとめ
    - 今回は`html-util.js`というシンプルなユーティリティモジュールを作成してHTML要素を構築していました。
    - 実際にウェブアプリを開発する際にはさまざまな機能が実装されたライブラリやある種のアプリ開発の仕組みをサポートするフレームワークを使うことが現実的です。
    - 特にウェブアプリ開発の際にそのスタイルを決めるのは表示を管理するViewのライブラリです
    - ここで実装したTodoアプリと呼ばれるシンプルなタスク管理アプリは、どのViewライブラリでもサンプルとして提供されています
    - TodoMVCというTodoアプリのリファレンス実装があり、各ライブラリはこのTodoMVCを実装したものをサンプルとして提供しています
    - これはユーザーからすれば各ライブラリの書き味を確かめることができ、ライブラリ作者からすればユーザーに慣れ親しんだアプリをどうかけるのを示すサンプルとなります
    - ここで実装するTodoアプリはTodoMVCを簡素化したものですが、このアプリのような作り方を理解することで他のライブラリを実際に触って見たときにも関係することが学べると思います。
    - あらゆるライブラリには基礎が存在し、魔法のような使い勝手があってもそのベースは基礎からきています
    - そのことを意識して見るといいかもしれませｎ。
- チェックリスト
    - [x] オブジェクト指向的なアプリ開発
    - [ ] クラスを使った開発
    - [ ] 状態を持つアプリの開発
    - [ ] モジュールによるスコープの分離
    - [ ] イベント駆動と表示の更新
- まとめ
- 未使用
    - モデル
        – この意味は場所によってバラバラですが、ここではアプリの状態を表しDOM APIに依存していないコードのことを言いましょう
        - DOM APIに依存していないということはブラウザだけではなく、Node.jsでもそのまま取り込み実行できることを意味しています
        - この構造を取る率直なメリットはテストがしやすいことです
    - モデルと表示
        - さきほども書いたウェブアプリはイベント駆動であることが多いです。
        - モデルに置いてもイベント駆動で、モデルの状態と表示の更新を考えてみましょう
        - ものすごく簡単にいえば、モデルの状態が更新されたら表示は更新されるように作れば、モデルの実装に集中できます。
        - 一般に表示の確認は実際にブラウザを起動して、URLにアクセスして、特定の操作をして、その時の表示を確認すると言った過程があり確認コストが高いです。
        - 一方モデルはただのJavaScriptなので、単純なユニットテストで状態を確認できます。
        - もちろん表示を確認するテスト手法もありますが、多くの場合はユニットテストよりも手間がかかることには変わらないでしょう。


## コンポーネント

説明するものをコンポーネントとして見ていく。

- 静的HTML
- CSS
- ディレクトリ構造
- ローカルサーバ    
    - モジュールがCORSにかかるため
- index.js
    - App
    - エントリポイント
- DOMイベント
    - 目的: イベントで表示を更新することを学ぶ
        - ゲームなどを除きDOM要素を使ったウェブアプリは殆どがイベント駆動
        - 時間経過で変化するのではなくイベントによって変化する部分が大体を占める
        - イベントとは何かが**起きた**ことを通知するもの
        - またDOM要素のイベントには発生時にデフォルトの動作が規定されているものがあります。
        - 例えば"click"イベントはクリックと同時に発生し、デフォルトの動作はその要素をclickすることです。
        - DOM Eventはこれらのデフォルトの動作を停止することができます
    - submitを止めてみましょう
    - 止まらなかった場合はリロードされています
    - イベントが発生したらViewを更新してみましょう
        - html-utilを作る?
- EventEmitter
    - 自分でイベントを作ってみましょう
    - なぜEventTargetではない
        - EventTargetはDOMにしかないから
- Model
    - TodoItem
    - TodoList
- View
    - html-util
    - TodoListView

### 動作フロー

- window onload
- App mount
    - Initialize
- Event
    - submit -> update view
    - delete -> update view
    - change -> update view
- View
    - call -> update model
- Model
    - called -> update state -> emit event
- Controller
    - receive event -> call update view

## 新しく登場するAPI

- EventEmitter
- querySelector
- innerHTML
- addEventListner
- event
    - `preventDefault`
- Element
- HTML
    - `<template>`

## 注意事項

他のユースケースと表記は揃える

- HTTPサーバ
- DOM取得（query selector?）
- addEventListnerの第三引数


### 構造を知る

- アプリを作るにあたって重要なのは構造
- 常にセクションで構造を把握できるディレクトリを表示する

### 用語

- TODOアプリ: アプリケーション全体
- TODOリスト: リスト部分
- TODOアイテム: リストのそれぞれのアイテム