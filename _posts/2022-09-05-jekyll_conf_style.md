---
layout: post
title: jekyllのカスタマイズ(スタイル編)
description: 
date: 2022-09-05 21:30:00 +0900
last_modified_at: 2022-09-30 17:00:00 +0900
tags: jekyll
comments: true
image: "/img/20220905_headline.png"
summary: サイトのスタイルに関係する設定を紹介します
---
今回はサイト全体に関わるスタイルについて説明します。日本語表示だとフォントや文字の大きさなどが微妙なので修正していきます。

## 設定
### app.scssの設定
```scss
// Import Main CSS file from theme
@import "mytheme";  // これを追加
@import "main";
@import "mystyle";  // これを追加
```
{: .hljs data-title="assets/css/app.scss"}

`assets/css/app.scss`は**bulma-clean-theme**におけるCSSのエントリーポイントになっています。  
記述を見てわかる通り、ここから`_sass/`フォルダの`_main.scss`を参照し、さらにそこからbulmaフレームワーク本体を参照する形になっています。

なのでbulmaフレームワーク内に影響する設定は、`_main.scss`の前の`mytheme.scss`に記述して、bulmaフレームワークの設定を上書きするようにします。  
その他bulmaフレームワークに絡まない個別のスタイルの設定については`_main.scss`の後の`mystyle.scss`に記述する様にします。

### mytheme.scssの設定
```scss
// テーマカラーの変更
$primary: #c4975e;
// 表示フォントの変更
$family-sans-serif: 'Hiragino Kaku Gothic ProN', 'ヒラギノ角ゴ ProN W3', Meiryo, メイリオ, Osaka, 'MS PGothic', arial, helvetica, sans-serif;
$family-monospace:   Menlo, Monaco, Consolas, "DejaVu Sans Mono", "Liberation Mono", "Courier New", 'Hiragino Kaku Gothic ProN', 'ヒラギノ角ゴ ProN W3', Meiryo, メイリオ, Osaka, 'MS PGothic', arial, helvetica, sans-serif; 
// 基本フォントサイズ変更
$body-size: 14px;
```
{: .hljs data-title="_sass/mytheme.scss"}

`$primary:`でプライマリカラーを変更します。  
これはテーマ全体の基本色「テーマカラー」の設定になります。こんがりきつねさん色に変更します。

`$family-sans-serif:`と`$family-monospace:`でゴシック体のフォントと等幅フォントの設定を変更します。  
デフォルトの設定フォントでは`Montserrat`となっており、何か違和感が強いので、標準的なシステムフォントを使用する様に変更します。

`$body-size:`で基本フォントサイズを変更します。  
デフォルトでは16pxとなっており、スマホだと若干大きい見えるので14pxに変更します。

### _main.scssの設定
```scss
@charset "utf-8";
//@import url('https://fonts.googleapis.com/css?family=Montserrat');    // ここ変更

$family-sans-serif: 'Montserrat', sans-serif !default;  // ここ変更
$primary: #188eac !default;

$tabs-link-active-color: $primary;
$tabs-link-active-border-bottom-color: $primary;

$modal-content-width: 800px;

@import "../node_modules/bulma/bulma.sass";
$hero-darken: $dark !default;

@import "layout";
//@import "syntax";   // ここ変更
@import "showcase";
@import "../node_modules/bulma-block-list/src/block-list.scss";
```
{: .hljs data-title="_sass/_main.scss"}

`Montserrat`フォントは使用しないのでimportをコメントアウトします。

`$family-sans-serif:`ですが、なぜかここで設定が強制上書きさせる様になっているので、`!default`オプションをつけて強制上書きされないように変更します。  
`!default`オプションとは、「変数値が未設定の場合にのみ設定され、すでに設定済みの場合は上書きしない」という指定するためのものです。

`@import "syntax"`をコメントアウトしているのは、後述しますがコード構文ハイライトをjekyll標準の`Rouge`から変更するので、Rouge用のスタイル設定ファイルは不要になったためです。

### mystyle.scssの設定
```scss
h2 {
    padding: 16px 16px 16px 10px;
    background-color: findLightColor($primary);
    border-left: 8px solid $primary;
}
  
h3 {
    padding: 6px 6px 6px 13px;
    border-bottom: dotted 1px $grey-light;
    border-left: 5px solid $primary;
}

h4 {
    &::before{
        content: "・";
    }
}
```
{: .hljs data-title="_sass/mystyle.scss"}

デフォルトの設定だと見出しが目立たないのでアレンジしました。

H1,H5,H6はそのままですが、H2は色付きボックス見出し、H3は下線付き見出し、H4はリーダーに中黒(・)だけとしました。  
こんな感じ↓

![Headline sample](/img/20220905_headline.png "見出しサンプル"){: .dropshadow}  

```scss
.hero-body {
    .title, .subtitle{
        text-shadow: 2px 2px 5px #000000;
    }
}
```
{: .hljs data-title="_sass/mystyle.scss"}
記事のタイトルスペース、俗にいう`hero`に白基調の画像を張ると文字が見にくくなるので、影を落とすことで対応しました。


```scss
.content {
    a[data-ext="true"]::after{
        content: ' \f35d';
        font-weight: 900;
        font-family: 'Font Awesome 5 Free';
    }

    table{
        width: initial;
    }
}
```
{: .hljs data-title="_sass/mystyle.scss"}

リンク先が外部であることをわかりやすくするためにアイコンを追加しました。  
アイコンは`fontawesome`の`external-link-alt`を使用しています。  
CSS内での記述なのでユニコード直書きしています。  
アイコンのスタイルが`solid`なので`font-weight: 900;`と設定しています。

ちなみに`fontawesome`をCSS内で使用する場合は、スタイルによってウェイトの指定を変える必要があります。  
html内ではクラスに`prefix`を追加することで指定できます。

|style 　 |font-weight|prefix|
| ----------- |:---------:|:----:|
|light   |300|fal|
|regular,<br>brands|400|far<br>fab|
|solid   |900|fas|
{:.table .is-striped}

使用するには以下のように、リンク記述に`data-ext="true"`という属性をインライン付加することで表示させることができます。

```markdown
[Jekyll](http://jekyllrb-ja.github.io/){:data-ext="true"}←こんな感じになります
```

[Jekyll](http://jekyllrb-ja.github.io/){:data-ext="true"}←こんな感じになります

それともう一つ、デフォルトの設定だとテーブルの横幅が、"100%"になっておりコンテナいっぱいに広がってしまうので、初期値に戻しています。

### _config.ymlの設定
```yml
theme_color: '#c4975e'    # テーマカラー変更
```
{: .hljs data-title="_config.yml"}
ここのテーマカラーとは、パソコンからの閲覧では関係ありませんが、スマホ等で閲覧した際、画面の上にあるステータスバーに色を付ける設定になります。  
こんな感じ↓

![smart phone](/img/20220905_smartphone.png "スマホ画面"){: .dropshadow}

***
