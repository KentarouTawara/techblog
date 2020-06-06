---
title: content_forとyieldと三項演算子とデフォルト引数【28/30 2nd】
date: "2020-06-06"
description: content_forとyieldと三項演算子とデフォルト引数の紹介
---

こんにちは、たわらです。

本記事は、タイトルページを動的に変更したい場合に学習したメソッドについて紹介します。

## yield メソッドと content_for メソッド

`yeild`メソッドは、view を挿入するべき場所を指定できます。そして、キーをつけてあげると、下記のように、`yield`する場所を複数に設定できます。

```html
application.html

<html>
  <head>
    <title><%= yield :head %></title>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

`content_for`メソッドを使うと、名前(キー)付きの yield ブロックを生成し、一致するキーをもつ`yield`の場所に挿入することができます。こんな感じです。

```ruby
<% content_for(:title,'ログイン | hogeアプリ ')%>
```

そうすると、html 構造が、こうなります。

```html
application.html

<html>
  <head>
    <title>ログイン | hogeアプリ</title>
  </head>
  <body>
    ・・・
  </body>
</html>
```

すごいですね、、、便利だ。

## デフォルト引数と三項演算子

ただ、これだとページタイトルを変えるたびに、`hoge | hogeアプリ`といちいち記述しないといけません。`| hogeアプリ`をなんとかしたいです。

それにトップページには `hogeアプリ`とだけタイトルに記載したいです。

そこでこんなふうに yield メソッドの戻り値を、引数に持つメソッドをモジュールに作成します。

```html
application.html

<html>
  <head>
    <title><%= page_title(yield(:title)) %></title>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

モジュールで作るメソッドはこんな感じ。

```ruby
module HogeHelper
  #ページタイトルを返すメソッド
  def page_title(page_title = '')
  #使いやすいようにローカル変数にする
    base_title = 'hogeアプリ'

  #三項演算子の活用。
    page_title.empty? ? base_title : page_title + "|" + base_title
  end
end
```

で、ここで、`デフォルト引数`が登場しています。`page_title(page_title = '')`の`= ''`ですね。

デフォルト引数というのは、メソッドの引数に何もなかったら〇〇を代入してね、ということを指示できます。

なのでこの場合、引数がなかったら空文字を入れる、という指示していることになります。

続いて、`page_title.empty? ? base_title : page_title + "|" + base_title`にて三項演算子が出てきます。

三項演算子というのは、`式 ? trueだった場合の処理 : falseだったときの処理`と書きます。

なのでこの場合、引数の値が空か否かで処理を分けています。ログインなどの文言が入れば、`ログイン | hogeアプリ`と自動的に入るということですね。

## 最後に

rails にはいろいろ便利メソッドがあるのですね。

読んでくださったかた、ありがとうございます。

## 参考文献

[Rails ガイド](https://railsguides.jp/layouts_and_rendering.html#yield%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B)

[引数のデフォルト値](https://www.javadrive.jp/ruby/method/index4.html)

[ActionView::Helpers::CaptureHelper](https://api.rubyonrails.org/v5.1.3/classes/ActionView/Helpers/CaptureHelper.html#method-i-content_for)

[Rails: ページタイトルはビューテンプレートの\`content_for`で表示すること（翻訳）
](https://techracho.bpsinc.jp/hachi8833/2018_05_15/55812)
