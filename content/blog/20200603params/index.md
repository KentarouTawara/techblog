---
title: paramsメソッドはルーティングパラメータとpostデータを取得できる【25/30 2nd】
date: "2020-06-03"
description: paramsメソッドの復習
---

こんにちは、たわらです。

本記事は Rails の params メソッドの情報を整理した記事になります。

### params メソッドが理解できずに困る

掲示板アプリを作成していて、掲示板の詳細にコメントを付けられる機能を実装しようと思いました。

フォームでコメントされた内容（body）を持ってきて保存する、ということですね。
で、こんな感じのコントローラーになります。

```ruby
  def create
    comment = current_user.comments.build(comment_params)
    if comment.save
      hoge
    else
      hoge
    end
  end

  private

  def comment_params
    params.require(:comment).permit(:body).merge(board_id: params[:board_id])
  end
```

でわからなかったのは、、、

```ruby
def comment_params
params.require(:comment).permit(:body).merge(board_id: params[:board_id])
end
```

の merge メソッドの中身ですね。

なんでこんなものが登場するの？ と思ってました。

で、講師の方に尋ねると、borad_id は url から取ってくるんだよ、と教えてもらいました。

なるほどー、と思ったのですが、「あれ、params ってフォームから入力された情報しか取得しないのでは？」と思い、復習しようと思ったのですね。

で、あらためて、[Rails ガイド](https://railsguides.jp/action_controller_overview.html#%E3%83%91%E3%83%A9%E3%83%A1%E3%83%BC%E3%82%BF)を見てみると、

> ルーティングで定義されるその他の値パラメータ (id など) にもアクセスできます。

とあります。つまり url からも持ってこれるのですね！

### params メソッドで ルーティングで定義された board_id を取得する

comment は DB に保存するのに、User モデルと Board も出ると関連しているので、user-id と board-id が必須です。

なので、フォームから comment[:body]を取得しつつ、board_id がいるのです。

で、board_id はルーティングで url 内に下記のように含まれています。

`boards/:board_id/comments`

これは、ルーティングで、

```
  resources :boards do
    resources :comments, shallow: true
  end
```

と指定しているからです。だから params で board_id を取得できるのですね。

## merge メソッド

ハッシュを合体させてくれるメソッド。今回これを実施することで、`content_params'に例えばこんなふうに入ります。

`{"body"=>"テスト", "board_id"=>"62"}`

## 最後に

params は

1. ルーティングで定義されたパラメータ

2. フォームからの post データ
   を取得できるのですね！

読んでくださったかた、ありがとうございます。
