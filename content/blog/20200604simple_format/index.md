---
title: コメント表示で便利なsimple_formatメソッド【26/30 2nd】
date: "2020-06-04"
description: simple_formatの学習
---

こんにちは、たわらです。

本記事は、**simple_format** メソッドの紹介です。

## コメント表示に便利

掲示板にコメント表示をする機能を実装しています。

で、コメント入力欄で、こんなふうに改行しても、

```
hogehoge
hogehoge
```

実際の表示では、`hogehoge hogehoge`のようになってしまうのです。

ただ単に、view で`<%= comment.body %>`と出力するだけでは、
改行が反映されないのですね。

## テキストを HTML に作り変える simple_format

simple_format メソッドを使うとこの問題を回避できます。

view での出力は、簡単で、

`<%= simple_format(comment.body) %>`

となります。

こうすれば、簡単に実装することができます。

コメント表示など、オブジェクトが抱えている改行されて入力されたテキストを、

入力された通りに改行してブラウザ上で出力したい場合に、とても便利なメソッドです。

## 参考文献

https://apidock.com/rails/ActionView/Helpers/TextHelper/simple_format
