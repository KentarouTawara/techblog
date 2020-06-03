---
title: 名前付きルートの引数には変数を渡せる【25/30 2nd】
date: "2020-06-03"
description: ルートヘルパーの復習
---

こんにちは、たわらです。

今回は、名前付きルートで変数を渡せることの確認です。

## resources で名前付きルートを生成する

resources メソッドを使うと、７つの名前付きルートが自動生成されます。パスヘルパーやら URL ヘルパーとも呼ばれます。呼び方統一してほしいですね。

```ruby
  resources :boards
```

こうすると、「〇〇\_path」や「〇〇\_url」ができるのですね。

この２つの違いは、生成されるパスが違います。

「〇〇\_path」の場合だと、
`/users`や`/users/:id/edit`といったパスを生成します。

一方で、「〇〇\_url」の場合だと、
`http://www.hoge.com/users`などのようにフルパスを生成します。

そういう違いがあったんですね、、、Rails はじめて２ヶ月くらいになるけどはじめて知りました、、、

## 名前付きルートは引数に変数を渡せる

こんなふうに名前付きルートには変数を渡すことができます。

`redirect_to board_path(comment.board)`

この`comment.board`の中身はこのようになってます。

```
 id: 62,
 title: "aaa",
 body: "aaa",
 user_id: 11,
 created_at: Tue, 26 May 2020 19:09:26 JST +09:00,
 updated_at: Tue, 26 May 2020 19:09:26 JST +09:00,
 board_image: "something.jpg"
```

で、実際に生成されたパスは、

```
[4] pry(#<CommentsController>)> board_path(comment.board)
=> "/boards/62"
```

となります。board 変数の id が格納されていますね。

ルーティングを確認すると、、、

```
 board GET    /boards/:id  boards#show
```

となっています。指定されたとおりのパスが変数から生まれています。

変数いれればなんとなくその変数で指定したページが表示されるんだろうな、と思っていましたが、その動作を確認できてよかったです。

## 最後に

復習っていいですね。
読んでくださった、ありがとうございます。

## 参考文献

・[Rails ガイド](https://railsguides.jp/routing.html#%E3%83%91%E3%82%B9%E3%81%A8url%E7%94%A8%E3%83%98%E3%83%AB%E3%83%91%E3%83%BC)

・[Rails のルーティングを極める(前編)](https://techracho.bpsinc.jp/baba/2014_02_17/15665)

・[Rails のルーティングを極める (後編)](https://techracho.bpsinc.jp/baba/2014_03_03/15619)
