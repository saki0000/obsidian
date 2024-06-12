---
tags:
  - kotlin
date: 2024-05-27 22:16
source:
  - https://www.geeksforgeeks.org/what-is-context-in-android/
---
# Contextとは
```ad-faq
android開発をしていくなかでContextという言葉をよく聞くことがある。Contextとは何を指しているのか？
```
### contextの文字的な意味
> イベントや状態、アイデアなどの設定を形成し、それを完全に理解できる状態
- Contextは周りの情報を伝えてくれるもの
## KotlinにおけるContext
アプリケーションの現在の状態を提供してくれるもの。以下の３つが主な用途である。
- リソースにアクセスする
- 他のAndroidコンポーネントとメッセージを送ることで連携する
- アプリケーションの環境についての情報を取得する
### AndroidにおけるContextの種類
1. Application Context
2. Activity Context
### Application Context
- ActivityとApplicationで取得できる
- Application Contextはアプリケーションのライフサイクルと結びついてる
- Application Contextは主にシングルトンのインスタンスである
- `getApplicationContext()`を使ってアクセスできる
#### 用途
- シングルトンのオブジェクトを作る必要がある時
- アクティビティでライブラリが必要な時
#### 機能
- リソースバリューをロードする
- [[Service]]を始める
- [[Service]]をバインドする
- ブロードキャストを送る
- [[BroadcastReciever]]を登録する
#### getApplicationContext()
これは、アプリケーションとリンクしたContextを返してくれる。
- このメソッドは通常、アプリケーションレベルで使用され、すべてのアクティビティを参照するために使用される
### Activity Context
- Activityのライフサイクルと結びついている
- ActivityごとにActivity Contextがある
- `getContext()`と`this`を使ってアクセスできる
- UIに関するContextを扱う
```ad-note
ユースケース
- アクティビティのライフサイクルに基づいてオブジェクトを作成するとき
- トーストやダイアログなどのUI関連のアクティビティを作る時
```
#### 用途
- リソースバリューをロードする
- レイアウトの拡張
- アクティビティを開始する
- ダイアログを表示する
- [[Service]]を開始する
- [[Service]]をバインドする
- [[Brouadcast]]を送る
- [[BroadcastReciever]]を登録する
#### getContext()
呼んだアクティビティにリンクしたContextを返す。