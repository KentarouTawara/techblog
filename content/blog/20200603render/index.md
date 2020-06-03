---
title: renderで繰り返し表示する【25/30 2nd】
date: "2020-06-03"
description: renderメソッドの復習
---

こんにちは、たわらです。

本記事は、render メソッドの復習です。特に繰り返し表示の方法です。

掲示板を作れるアプリケーションを作成しています。で、掲示板一覧を取得して、そのうちの１つを選ぶと、詳細場面に移動します。そのページにはコメント作成フォームと、コメント一覧がある、という具合です。

## render で繰り返し表示をする

で、コメント一覧を表示するのに、render を使います。

コントローラーはこんな感じ。

```ruby
  def show
    @board = Board.find(params[:id])
    @comment = Comment.new
    @comments = @board.comments
  end
```

で、show.html.erb はこんな感じになります。

```html
<!-- コメントフォーム -->
<%= render "comments/form", {comment: @comment, board: @board} %>

<!-- コメントエリア -->
<%= render "comments/comments", {comments: @comments} %>
```

コメントエリアの方に着目です。

comments フォルダの`_comments.html.erb`をレンダーしています。で、@comments を comments というローカル変数で扱えるようにしています。

`_comments.html.erb`の中で、、、

```html
<table id="js-table-comment" class="table">
  <%= render comments %>
</table>
```

としています。また render メソッドが登場してる！！！

で、`_comment.html.erb`を推測して、呼び出し、そのなかで、`comment.id`とか`comment.body`のように`comment変数`を使っています。

## @hoges でなくてもよいの？

こんな書き方できるんだ、というのが感想です。＠hoges みたいなインスタンス変数じゃないとできないのでは？と思っていました。

Rails ガイドをよく読めば、次のようにあります。

> パーシャルはデータの繰り返し (コレクション) を出力する場合にもきわめて便利です。:collection オプションを使用してパーシャルにコレクションを渡すと、コレクションのメンバごとにパーシャルがレンダリングされて挿入されます。

> index.html.erb

> ```html
> <h1>Products</h1>
> <%= render partial: "product", collection: @products %>
> ```

> \_product.html.erb

> ```html
> <p>Product Name: <%= product.name %></p>
> ```

> パーシャルを呼び出す時に指定するコレクションが複数形の場合、パーシャルの個別のインスタンスから、出力するコレクションの個別のメンバにアクセスが行われます。このとき、パーシャル名に基づいた名前を持つ変数が使用されます。上の場合、パーシャルの名前は\_product であり、この\_product パーシャル内で product という名前の変数を使用して、出力されるインスタンスを取得できます。

注目すべきは、

`パーシャルを呼び出す時に指定するコレクションが複数形の場合、パーシャルの個別のインスタンスから、出力するコレクションの個別のメンバにアクセスが行われます`

の部分です。

@ではじまる変数でないといけないとは書いてないですね、、、。

コレクションが複数形の場合ならこのように使えるということなのですね。

`render hoges(コレクション)`とすると、よしなに`_hoge.html.erb`を呼び出し、そのなかで、`hoge.id`とか`hoge.name`とか使える、ということですね。

なので、パーシャルのファイル名を間違えないようにしないといけません。

## 最後に

額面どおりに学習していると、@がないといけない、のような謎のマイルールで脳みそを縛ってしまいます。

復習してスッキリしました。

読んでくださったかた、ありがとうございます。

## 参考文献

[Rails ガイド](https://railsguides.jp/layouts_and_rendering.html#%E3%82%B3%E3%83%AC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0%E3%81%99%E3%82%8B)
