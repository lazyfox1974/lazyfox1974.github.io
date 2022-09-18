---
layout: post
title: jekyllのカスタマイズ(ポスト編)
description: 
date: 2022-09-19 06:00:00 +0900
tags: jekyll
comments: true
mathjax: true
render_with_liquid: false
image: "/img/20220919_message.png"
summary: ブログの投稿記事に関係する設定を紹介します
excerpt: ""
---
今回はブログ投稿記事で使用されるスタイル等の細々したものを説明していきます。  
ここで紹介されているカスタマイズは、一部bulmaフレームワークの変数やヘルパー関数を使用していますが、基本的に使用しなくても実現可能です。

なにはともあれ、ほとんど趣味的なカスタマイズなので参考になれば幸いです。

## 設定
### 追加スクリプトの設定
```html
<!-- head scripts -->
    <!-- mathjaxスクリプト注入 -->
    {% if page.mathjax %}{% include mathjax-v3.html %}{% endif %}
    <!-- highlight.jsスクリプト注入 -->
    {% include highlight-js.html %}
```
{: .hljs data-title="_includes/head-scripts.html"}

ページ内部で使用するスクリプトは`_includes/head-scripts.html`に追加します。  
直接scriptタグを書いてもいいのですが、見づらくなるのが嫌なので別ファイルにしてincludeで読み込むようにします。  
**MathJax**は結構重いので必要な時にのみ読み込むようにします。

ちなみにこの`_includes/head-scripts.html`の読み込み順序としては、**Bulmaフレームワーク**や**fontawesome**の後、\<body>読み込みの前になります。

#### MathJaxのスクリプト追加
```html
<!-- Begin mathjax-v3 -->
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<!-- End mathjax-v3 -->
```
{: .hljs data-title="_includes/mathjax-v3.html"}
[MathJax Docs](https://docs.mathjax.org/en/latest/web/start.html#using-mathjax-from-a-content-delivery-network-cdn){:data-ext="true"}にある通り、上記の様に\<script>タグを追加します。

**MathJax**を使用することで、数式をきれいにレンダリングできます。**MathJax**を使用するには、記事のフロントマターで`mathjax: true`を追加します。

$$
\begin{eqnarray}
\left\{
\begin{aligned}
\nabla \cdot \vec{\mathbf{B}} &= 0 \\
\nabla \cdot \vec{\mathbf{D}} &= \rho \\
\nabla \times \vec{\mathbf{H}} &= \vec{\mathbf{j}} + \frac{\partial\vec{\mathbf{D}}}{\partial t} \\
\nabla \times \vec{\mathbf{E}} &= -\frac{\partial\vec{\mathbf{B}}}{\partial t} \\
\end{aligned}
\right.
\end{eqnarray}
$$

こんなふうにマックスウェル方程式も余裕です。  
アインシュタインの重力方程式も『$$R_{\mu\nu}-\frac{1}{2}g_{\mu\nu}R+\Lambda g_{\mu\nu}=-\frac{8\pi G}{c^4}T_{\mu\nu}$$』こんな感じでインライン表示できます。

#### highlight.jsのスクリプト追加
```html
<!-- Begin highlight.js -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/highlightjs/cdn-release@11.6.0/build/styles/atom-one-dark-reasonable.min.css">
<script src="https://cdn.jsdelivr.net/gh/highlightjs/cdn-release@11.6.0/build/highlight.min.js"></script>
<script>hljs.highlightAll();</script>
<!-- End highlight.js -->
```
{: .hljs data-title="_includes/highlight-js.html"}
構文ハイライトライブラリ(ハイライター)は、デフォルトで`rouge`が使用されますが、ここでは軽量でかつ高速なライブラリである`highlight.js`を使用します。  
[highlight.js usage](https://highlightjs.org/usage/){:data-ext="true"}にある通り、上記の様に\<script>タグと\<link>タグを追加します。
使用するハイライトスタイルは\<link>タグで設定します。  

**rouge**ではJekyll側で構文ハイライト処理を行いますのでブラウザ側の負荷は少ないのですが、その反面色分けのための\<span>タグが、本体のコードより多く挿入され、結果としてファイル容量が大きくなります。(htmlソースコードのデバッグする時に凄い邪魔)

そんなこともあってブラウザの負荷が若干かかりますが、ブラウザ側で構文ハイライトを行う**highlight.js**を使用しています。

```yml
kramdown:
  syntax_highlighter_opts:
    disable: true   # rougeを無効化
```
{: .hljs data-title="_config.yml"}
デフォルトの**rouge**を無効化するには、上記のように`_config.yml`に設定する必要があります。

### コードブロックの微調整
```scss
pre {
    background-color: $body-background-color;
}

.content {
    pre {
        padding: 0.2em;
        &[data-title]::before{
            content: attr(data-title);
            margin: 0em 1.0em;
            padding: 0.4em 0.3em 0.2em;
            color: $white;
            background: $grey;
        }
    }
}
```
{: .hljs data-title="_sass/mystyle.scss"}
コードブロックのスタイルにちょっと手を加えます。  
技術系ブログサイトの`Qiita`では、コードブロックの頭にコードの説明をするタイトルをつけることができます。それと同じ様なものを実装してみました。

~~~html
```html
<div id="sample">
    <a href="/">sample link</a>
</div>
```
~~~
何も指定しないと、上のような普通のコードブロックになります。

~~~html
```html
<div id="sample">
    <a href="/">sample link</a>
</div>
```
{: data-title="コードブロックサンプル"}
~~~
{: data-title="コードブロックサンプル"}
コードブロックに`{: data-title="文字列"}`という属性をインライン付加すると、上の様なタブ表示になります。

~~~html
```html
<div id="sample">
    <a href="/">sample link</a>
</div>
```
{: .hljs data-title="コードブロックサンプル"}
~~~
{: .hljs data-title="コードブロックサンプル"}
属性に**highlight.js**のクラス`hljs`を追加してスタイルの色情報を継承することで、上の様な埋め込みタイトル風の表示にすることができます。

### キートップ風スタイル
```scss
kbd {
    display: inline-block;
    vertical-align: text-top;
    margin: 0;
    padding: 0.2em 0.5em 0.2em 0.5em;
    font-family: $family-monospace;
    font-size: 92%;
    line-height: 1.0;
    color: #202020;
    background-color: #E0E0E0;
    border: 1px solid #606060;
    border-radius: 3px;
    box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5), 1px 1px 1px rgba(255, 255, 255, 0.3) inset;
    white-space: pre-wrap;
}
```
{: .hljs data-title="_sass/mystyle.scss"}
キーボード操作のなどの説明の際用いられる\<kbd>タグにスタイルを追加します。

```html
Windowsでは<kbd>Ctrl</kbd> + <kbd>R</kbd>で、Macでは<kbd>&#x2318;cmd</kbd> + <kbd>R</kbd>でリロードします。
```
Windowsでは<kbd>Ctrl</kbd> + <kbd>R</kbd>で、Macでは<kbd>&#x2318;cmd</kbd> + <kbd>R</kbd>でリロードします。

文字列を\<kbd>タグで囲むだけで、上のようなキートップ風に表示させることができます。

### 貼り付けイメージに影追加
```scss
.content {
    img.dropshadow {
        -webkit-filter:drop-shadow(1px 3px 5px rgba(0, 0, 0, 0.4));
        -moz-filter:drop-shadow(1px 3px 5px rgba(0, 0, 0, 0.4));
        -ms-filter:drop-shadow(1px 3px 5px rgba(0, 0, 0, 0.4));
        filter:drop-shadow(1px 3px 5px rgba(0, 0, 0, 0.4));
    }      
}
```
{: .hljs data-title="_sass/mystyle.scss"}
デフォルトだと画像を貼るとベタ貼り感ハンパないので、影を落として柔らかくします。  
`box-shadow`じゃなく`drop-shadow`を使っているので、アルファで切り抜いた画像でも正しく影をつけられます。

使用する場合は、`{:.dropshadow}`という属性を次のようにインライン付加します。
```markdown
![影付ききつねさん](/img/20220904_favicon.png "影付ききつねさん"){:.dropshadow}
```
![普通のきつねさん](/img/20220904_favicon.png "普通のきつねさん")
![影付ききつねさん](/img/20220904_favicon.png "影付ききつねさん"){:.dropshadow}

左が影なし。右が影あり。

### 引用をモダンスタイルに
```scss
.content {
    blockquote{
        position: relative;
        padding-left: 45px;
        background-color: $white;
        border: 0px;
        box-shadow: 2px 2px 6px hsla(0, 0%, 80%, 1.00);
        &::before{
            content: url(/assets/img/quote.png);
            position: absolute;
            top: 10px;
            left: 10px;
        }
        cite{
            margin-top: 0.5em;
            text-align: right;
            display: block;
        }
   }
}
```
{: .hljs data-title="_sass/mystyle.scss"}
デフォルトだと左側にボーダーだけのレトロな引用スタイルなので、よく見かける引用符つきのスタイルに変更します。  
引用符はこちらの画像を使用しています。[引用符](https://www.ac-illust.com/main/detail.php?id=22539967&word=%E5%BC%95%E7%94%A8%E7%AC%A6%E3%81%BE%E3%81%9F%E3%81%AE%E5%90%8D%E3%82%92%E3%82%AF%E3%82%AA%E3%83%86%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3){:data-ext="true"} (但しアスペクト比を若干変更しています。)

引用符の画像は`blockquote`を基準に、絶対位置で指定しています。それに合わせて`blockquote`のパディングを調整しています。その他、引用元記述\<cite>を右寄せになる様に変更します。

```markdown
> 文頭に>を置くことで引用になります。複数行にまたがる場合には改行の都度>を置く必要があります。  
> 引用の中に引用を入れ子にすることができます。また、引用中で**Markdown**を使用することもできます。  
> 引用元を記載する場合は、引用元記述を<cite>タグで囲ってください。
> <cite>引用元：xxxxxxxxxxxx</cite>
```
上のmarkdown記述は次のようになります。↓
> 文頭に>を置くことで引用になります。複数行にまたがる場合には改行の都度>を置く必要があります。  
> 引用の中に引用を入れ子にすることができます。また、引用中で**Markdown**を使用することもできます。  
> 引用元を記載する場合は、引用元記述を\<cite>タグで囲ってください。
> <cite>引用元：xxxxxxxxxxxx</cite>

### 付箋風メッセージブロック
```scss
@mixin dl-mixin($char-color, $border-color, $bg-color, $content) {
    margin-left: 0.25em;
    margin-right: 0.25em;
    padding: 0.5em 1.5em 0.75em 1em;
    color: $char-color;
    border-left: 0.3em solid $border-color;
    background-color: $bg-color;
    border-radius: 3px;
    position: relative;
    box-shadow: 2px 2px 6px hsla(0, 0%, 80%, 1.00);
    > dt {
        font-weight: bold;
        font-style: normal;
        margin-top: inherit;
        padding-left: 1em;
        &::before {
            content: $content;
            font-size: 3em;
            font-weight: 900;
            font-family: 'Font Awesome 5 Free';
            color: $border-color;
            opacity: 0.3;
            position: absolute;
            margin-left: -0.5em;
            margin-top: -0.2em;
        }
    }
    > dd {
        margin-left: 1.5em;
        margin-bottom: 0;
        > ul {
            margin: 0 0 0 0.5em;    // リーダーを行頭ツライチに
            padding-left: 0.5em;
        }
        li + li {
            margin-top: 0em;        // リスト表示をコンパクトに
        }
        strong {
            color: inherit;
        }

    }
}

dl.note   { @include dl-mixin($grey-dark   , $grey   , $grey-lightest, '\f303'); }
dl.tip    { @include dl-mixin($success-dark, $success, $success-light, '\f058'); }
dl.info   { @include dl-mixin($info-dark   , $info   , $info-light   , '\f05a'); }
dl.warning{ @include dl-mixin($warning-dark, #ffbc6b , $warning-light, '\f071'); }
dl.caution{ @include dl-mixin($danger-dark , $danger , $danger-light , '\f05e'); }
```
{: .hljs data-title="_sass/mystyle.scss"}
補足事項などを目を引きたい場合、**付箋風メッセージブロック**で状況に合ったスタイルで表現することができます。  
定義リストをベースに、クラス属性ごとにスタイルシートで外観を装飾しています。

具体的には、親要素の`dl`を基準にして**FontAwesome**のアイコンを半透明化して絶対位置で貼り付けて、あとはそれに合わせて文字の位置をパディングやマージンで調整しています。  
それとメッセージブロック内でリスト表示を使用すると行間が開きすぎるので、コンパクトになるように調整します。

あとは、それぞれのクラスに合わせて配色とアイコンを設定しています。(**scss**の`@mixin`はこういう時に使うとすごく便利。)
```html
メッセージのタイトル
: 定義リストに属性`{:.note}`を付加することで、この様な付箋スタイルのメッセージブロックにすることができます。  
  行末に半角空白を２つの後改行することで、複数行に分けて書くこともできます。
{:.note}
```
↓こんな感じなります。

メッセージのタイトル
: 定義リストに属性`{:.note}`を付加することで、この様な付箋スタイルのメッセージブロックにすることができます。  
  行末に半角空白を２つの後改行することで、複数行に分けて書くこともできます。
{:.note}

確認
: 定義リストに属性`{:.tip}`を付加するとこうなります。
{:.tip}

情報
: 定義リストに属性`{:.info}`を付加するとこうなります。
{:.info}

注意
: 定義リストに属性`{:.warning}`を付加するとこうなります。
{:.warning}

警告
: 定義リストに属性`{:.caution}`を付加するとこうなります。
{:.caution}

### 記事の冒頭に投稿日と更新日を追加
```html
<div class="content">
    <p class="post-date">
      <!-- 投稿日、更新日フォーマット変更 -->
      投稿日: {{ page.date | date_to_xmlschema | date: "%Y年%m月%d日 %H:%M %Z" }}
      {%- if page.last_modified_at -%}
        <br>更新日: {{ page.last_modified_at | date_to_xmlschema | date: "%Y年%m月%d日 %H:%M %Z" }}
      {%- endif -%}
    </p>
    <!-- タグ表示位置変更 -->
    <div class="tags">
      {% for tag in page.tags %}
        {% include tag.html tag=tag style="is-info is-light is-medium"%}
      {% endfor %}
    </div>
  
    {% if page.series %}
        {% include series.html %}
    {% endif %}

    {{ content }}
</div>

{% include post-nav.html %}     <!-- 記事ナビゲーションボタンのためにこれを追加 -->

{% unless site.hide_share_buttons %}
{% include share-buttons.html %}
{% endunless %}

{% if site.disqus.shortname %}
  {% include disqus.html %}
{% endif %}
```
{: .hljs data-title="_layouts/post.html"}
投稿日だけでなく更新日も表示するようにします。それに合わせて日時のフォーマットを変更します。  
更新日を格納する変数はなんでもいいのですが、`last_modified_at`という変数は、プラグインの**jekyll-feed**や**jekyll-sitemap**でも更新日として参照されているのでそれに倣います。

日時フォーマットは`"%Y年%m月%d日 %H:%M %Z"`として、日本で標準的な並び順と末尾にタイムゾーンを略語表記で表示させます。しかし、なぜか更新日の方だけタイムゾーンがうまく表示できません。なので、一旦`date_to_xmlschema`でISO8601形式に変換してから、再度フォーマット変換することでこの問題を回避しています。

ついでにタグの位置を記事末尾から、投稿日／更新日の直後に変更しています。

あと次の項目で説明しますが、記事ナビゲーションボタンを記事本文直後に追加します。

```scss
.content{
    .post-date {
        font-family: $family-monospace;
        font-size: $small-font-size;
        line-height: 1.2em;
    }
}
```
{: .hljs data-title="_sass/mystyle.scss"}
投稿日／更新日をコンパクトに表示するためにスタイルを調整します。

### 記事ナビゲーションボタンの設置
```html
<div class="post-nav">
    <a class="pre-post {% unless page.previous %} is-invisible {% endunless %}" href="{{ page.previous.url }}">
        <div class="pre-arrow"><i class="fas fa-chevron-left"></i></div>
        <div class="pre-title">
            <div>Previous Post</div>
            <div><strong>{{ page.previous.title }}</strong></div>
        </div>
    </a>
    <a class="next-post {% unless page.next %} is-invisible {% endunless %}" href="{{ page.next.url }}">
        <div class="next-title">
            <div>Next Post</div>
            <div><strong>{{ page.next.title }}</strong></div>
        </div>
        <div class="next-arrow"><i class="fas fa-chevron-right"></i></div>
    </a>
</div>
```
{: .hljs data-title="_include/post-nav.html"}

```scss
div.post-nav {
    display: flex;
    justify-content: space-between;
    height: auto;
    flex-wrap: wrap;
    .pre-post, .next-post{
        width: 300px;
        height: 5em;
        margin: 0.3em;
        display: table;
        border-collapse: collapse;
        color: $primary-dark;
        .pre-arrow, .next-arrow{
            display: table-cell;
            background-color: $primary;
            vertical-align: middle;
            text-align: center;
            color: findColorInvert($primary);
            width: 1.5em;
        }
        .pre-arrow {
            border-radius: 5px 0 0 5px;
        }
        .next-arrow {
            border-radius: 0 5px 5px 0;
        }
        .pre-title, .next-title{
            display: table-cell;
            vertical-align: middle;
            background-color: lighten($primary, 35%);
            padding: 0 0.5em;
            strong {
                padding: 0.5em;
            }
        }
        .pre-title{
            text-align: left;
            border-radius: 0 5px 5px 0;
        }
        .next-title{
            text-align: right;
            border-radius: 5px 0 0 5px;
        }
    }
}
```
{: .hljs data-title="_scss/mystyle.scss"}
このテーマには「前の記事」「次の記事」のようなナビゲーションはついていないので自作しました。  
記述が長いだけで何も難しいことはしていません。基本的にボタンの配置と色の指定と位置決めをしているだけです。

`_include/post-nav.html`では、前記事／後記事それぞれのアンカータグを配置して、リンク先がない場合非表示になるようにしています。

`_scss/mystyle.scss`では、前述のアンカータグにボタン風のスタイル付けをしています。二つのボタンは`flexbox`を使って横並びに配置しています。ボタン自体は`display: table, table-cell`を用いて\<div>要素を一体化させています。

この**display: table**を使うと\<div>や\<p>などのブロック要素をテーブルセルとして扱うことができ、要素内のテキストは**text-align:**と**vertical-align:**で横および縦方向の揃え方を指定できて便利です。

どのように表示されるかは、この記事の一番下を見てください。↓

## IALについて
最後に、何度も度々出てくる`{: xxxx}`について説明します。

これは**IAL(Inline Attribute Lists)**と呼ばれるもので、Markdownレンダラーが生成する要素に新たな属性を付加することができます。

注意
: **IAL**は、**Jekyll**の標準Markdownレンダラーである**kramdown**の方言で、**PHP Markdown Extra**や**Maruku**の影響を強く受けたとされる機能です。なので他のレンダラーでは使用できない場合があるので注意してください。
{:.warning}

IALには大きく分けて二通りあり、一つは**インラインレベル要素に対してのIAL**と、もう一つは**ブロックレベル要素に対してのIAL**です。

### インラインレベル要素に対してのIAL
このようなmarkdown記述は、
```markdown
例えば**このように**{:style="color: red;"}直接スタイルを設定できます。
[link](sample.html){:target="_blank"}
```
レンダラーによって次のように解釈されます。
```html
例えば<strong style="color: red;">このように</strong>直接スタイルを設定できます。
<a href="sample.html" target="_blank">link</a>
```
この時注意が必要なのは、インラインレベル要素の**直後**にIALを記述する必要があります。  
空白や改行や文字を挟むことはできません。余分な文字列があるとIALとは解釈されず、IALがそのまま出力されてしまいます。

### ブロックレベル要素に対してのIAL
このようなmarkdown記述は、

~~~plaintext
> 引用はブロック要素になります。  
> IALで属性を付加することが可能です。
{:id="myid"}

```c
#include<stdio.h>
```
{:class="myclass"}
~~~

レンダラーによって次のように解釈されます。

```html
<blockquote id="myid">
    <p>引用はブロック要素になります。<br/>IALで属性を付加することが可能です。</p>
</blockquote>

<pre class="myclass">
    <code class="language-c">
        #include<stdio.h>
    </code>
</pre>
```

この時注意が必要なのは、ブロックレベル要素の**直前あるいは直後の行頭**にIALを記述する必要があります。  
また、前述の通りIALは**前置型**・**後置型**どちらでも使用可能ですが、**前置型**を使用した場合、IALの前にブロックレベル要素が来ないようにしてください。**後置型**が優先されるので前にあるブロック要素に属性が付加されてしまいます。

インラインレベル要素IAL、ブロックレベル要素IALどちらでも、以下のような省略表記が可能です。  
`{:class="myclass"}`は、`{:.myclass}`と省略可能です。また`{:id="myid"}`は、`{:#myid}`とすることもできます。  

***
