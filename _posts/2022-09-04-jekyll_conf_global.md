---
layout: post
title: jekyllのカスタマイズ(グローバル編)
description: 
date: 2022-09-04 04:30:00 +0900
last_modified_at: 2022-09-30 17:00:00 +0900
tags: [jekyll, GitHub Pages]
comments: true
image: "/img/20220904_top.PNG"
excerpt: "Jekyllの説明とカスタマイズ方法を数回に分けて説明していきます。"
summary: サイト全般に関係する設定を紹介します
---
やっとサイトの公開まで漕ぎ着くことができました。  
ということでサイト公開にあたり、それまでにやってきたカスタマイズについて何回かに分けて、公開していきたいと思います。

## 始める前に
環境は「GitHub Pages」上で「jekyll」を使用することを想定しています。そのうえでjekyllのテーマには「bulma-clean-theme」を使用し、これをカスタマイズしていきます。

ローカルシステムへの開発環境の構築やJekyllのインストールについては、当サイトでは説明しません。詳しく説明されている方が、たくさんいらっしゃいますので、そちらの方をご覧ください。

当サイトでは他のサイトでは書かれていないことを中心に説明します。

### GitHub Pagesとは
[GitHub社](https://github.com/){:data-ext="true"}がユーザーに対して提供している**ウェブホスティングサービス**です。  
これによって、GitHubに登録してるプロジェクトの**ドキュメント**やその開発状況を記した**ブログ**を公開することができます。しかも無料で。  

### GitHubとは
それじゃ「GitHub」ってなんだ？

端的に言えばソフトウェア開発のための**ソースコードホスティングサービス**です。  
これによって、インターネットを通じ複数のソフトウェア開発者で協調しながら開発を行うことができます。

まぁこれ以上詳しいことはググってください。

### Jekyllとは
**[Jekyll(ジキル)](http://jekyllrb-ja.github.io/){:data-ext="true"}**とは、いわゆる**静的サイトジェネレーター**と呼ばれるものです。  
**Jekyll**では、ブログの記事をmarkdownやHTMLなどで記述したのち、Jekyllを用いて表示用のHTMLに変換するという形をとっています。  
何でこんなまどろっこしい事しているのかと言うと、このページもそうですが、実際は色々なパーツが組み合わされてできています。例えば、
* 最上部のナビゲーションバー
* その下のタイトルスペース
* ブログ記事本体
* その横の最新投稿カード
* 等々…

これらをいちいち最初から書いていくこともできますが、それよりもパーツ群をレイアウトした雛形を事前につくっておき、必要に応じて雛形に文章を流し込む方が効率的です。  
で、それをやってくれるものが「ブログツール」で、雛形に当たるのが「テーマ」または「テンプレート」となります。

この様なブログツールは一般的に、ページ生成はサイト閲覧者がリンクをクリックするたびに動的に行われるのが殆どで、**WordPress**や**Drupal**に代表されるシステムで採用されています。しかし、柔軟性や汎用性は高いものの、レスポンスが悪くなる傾向があり、Webサーバー側には高いパフォーマンスが要求されます。

一方、事前にサイトを生成しWebサーバーにアップロードしておく方法が、**Jekyll**や**Hugo**に代表されるシステムで採用されています。これにより柔軟性が犠牲になりますが、レスポンスが高く、Webサーバーの負荷も軽いものとなります。

結論的に、なぜJekyllを使うのか？  
それは、GitHubの創業者の一人がJekyllの開発者であり、GitHub Pagesにバックエンドエンジンとして統合されており、GitHubとは非常に親和性が高いからです。  
(ローカルPCにJekyllをインストールして、ローカルで生成したサイトをアップロードする使い方も可能)
### bulma-clean-themeとは
bulma-clean-themeとは、前述した**パーツをレイアウトしたテンプレート「テーマ」**の一つになります。

[Jekyll Themes](http://jekyllthemes.org/){:data-ext="true"}を見てわかる通り、テーマは数百ほどのあります。その中でもシンプル過ぎずかつアレンジしやすいテーマということで[C.S. Rhymes](https://www.csrhymes.com/){:data-ext="true"}氏作成の[bulma-clean-theme](https://github.com/chrisrhymes/bulma-clean-theme){:data-ext="true"}を使わせていただきました。

名前にもある通り、**bulma**と呼ばれるCSSフレームワークが使用されています。

### bulmaとは
**[bulma(ブルマ)](https://bulma.io/){:data-ext="true"}**とは前述の通りCSSフレームワークの一種です。  
**bulma**は同様のフレームワーク**bootstrap**にかなり影響を受けていますが、bulmaはbootstrapと比べると…
* JavaScriptが不要であるため、他のJSフレームワークとぶつからない
* CSS100%なので比較的高速
* フレキシブル・レスポンシブルデザイン対応
* Sassを使うことでカスタマイズが非常に容易
* シンプルで見通しがいいので、覚えやすい

…と、CSSフレームワーク初心者にも扱いやすいものとなっています。ハッキリ言いましょう、**好きです[^bloomer]**。

[^bloomer]:「ぶるまスキスキ」とか言うと変態扱いされそう。ちなみに女性向け運動用半ズボンのことは「bloomer」と綴ります。名前の由来はアメリカ人のAmelia Bloomerさんが広めたことから来ているそうです。(Good Job!!)

## 設定
### _config.ymlの設定
```yml
# Site Settings
lang: ja                            # 言語を日本語に設定(デフォルトでは"en")
favicon: /assets/img/favicon.png    # faviconを設定
timezone: Asia/Tokyo                # タイムゾーンを東京に設定
# 以下省略...
```
{: .hljs data-title="_config.yml"}
「lang:」で日本語を設定。  
デフォルトで"en"になってしまうので設定しておいた方が良いでしょう。

「timezone:」で東京を設定。  
デフォルトでは"UTC"になるのでそのままだと投稿日時とかの表示がズレてしまいます。

favicon(ファビコン)とは、ブラウザのブックマークやタブのサイト名の横に表示されるアレのことです。  
当サイトのfaviconは、フリー素材のものを、当方で着色しものを使用しています。  
![favicon](/img/20220904_favicon.png "きつねさん"){: .dropshadow}  
かわえぇ:two_hearts:  
元画像はこちらになります。[きつねさん](https://www.silhouette-ac.com/detail.html?id=123789&sw=%E7%8B%90){: data-ext="true"}、[葉っぱ](https://www.silhouette-ac.com/detail.html?id=121050&sw=%E8%91%89){: data-ext="true"}


```yml
# リモートテーマを設定
remote_theme: chrisrhymes/bulma-clean-theme

plugins:
  - jekyll-sitemap
  - jekyll-paginate
  - jekyll-feed
  - jekyll-seo-tag
  - kramdown-parser-gfm
  - jekyll-redirect-from
  - jekyll-remote-theme   # リモートテーマプラグイン追加
  - jemoji                # 絵文字プラグイン追加

# 以下省略...
```
{: .hljs data-title="_config.yml"}
次に、サイトの見た目を決める「テーマ」です。

テーマを構成するファイルをゴッソリ持ってくる方法もあるのですが、当サイトでは**リモートテーマ**機能を用いて取得しています。こうした方がリポジトリ内もスッキリするし、サイト構築するたびに最新のテーマファイルを使用することが出来ると言う利点があります。
GitHub Pages自体リモートテーマには対応はしていますが、デフォルトでは有効になっていないので、設定でプラグインを有効にします。

ついでに絵文字のプラグインも有効にしておきます。

:fox_face:<-こういう絵文字を`:fox_face:`という感じの**ショートコード**で追加できるようになります。
ショートコードについては、こちらの[emoji cheat sheet](https://www.webfx.com/tools/emoji-cheat-sheet/){: data-ext="true"}が参考になります。

ローカルPCで開発環境を作っている場合は、リモートテーマと絵文字のプラグインは入っていないので`Gemfile`に追加してインストールしてください。

### サイトアイコンの追加
![top画面](/img/20220904_top.PNG){: .dropshadow}

デフォルトだとページ最上部のナビゲーションバーがサイト名だけでなんか寂しい。  
というわけでサイトアイコンを追加してみました。
```html
    <div class="navbar-brand">{% raw %}
        <a href="{{ site.baseurl }}/" class="navbar-item">
            <img class="pr-2" src="{{ '/assets/img/favicon.png' | relative_url }}"> <!-- これを追加 -->
            {{ site.title }}
        </a>
        以下省略...
    </div>{% endraw %}
```
{: .hljs data-title="_includes/header.html"}
ナビゲーションバー内のレイアウトは`_include/header.html`のファイルにあるのでそれを編集します。

`navbar-brand`クラスの\<div>ブロックの中に、\<a>タグでサイト名を設定しているところがあるので、サイト名の前に\<img>タグを追加します。アイコンはfaviconと同じものを使用しています。  
\<img>タグの`pr-2`クラスは、アイコンとサイト名の間のパッディングを追加するためのモノでbulmaのヘルパークラスです。

### ナビゲーションバーに各種ＳＮＳボタンを追加
```html
<div class="navbar-end">{% raw %}
        {% if site.twitter_username %}
        <a class="navbar-item" href="https://twitter.com/{{ site.twitter_username }}">
            <span class="icon"><i class="fab fa-lg fa-twitter"></i></span>
        </a>
        {% endif %}
        {% if site.github_username %}
        <a class="navbar-item" href="https://github.com/{{ site.github_username }}">
            <span class="icon"><i class="fab fa-lg fa-github"></i></span>
        </a>
        {% endif %}
        <a class="navbar-item" href="{{ site.baseurl }}/feed.xml">
            <span class="icon"><i class="fas fa-lg fa-rss-square"></i></span>
        </a>
    </div>{% endraw %}
    以下省略...
```
{: .hljs data-title="_includes/header.html"}

デフォルトで設定ではナビゲーションバー右側にGitHub Sponsorへのリンクがあり、これは`_config.yml`内の`gh_sponsor`をコメントアウトすれば消せますが、折角なんで各種ＳＮＳボタンに置き替えます。

`navbar-end`クラスの\<div>ブロックの中がそれにあたります。
\<a>タグには`:hover`等のスタイルを継承させるために`navbar-item`クラスを追加します。

リンク主体のＳＮＳボタンには**fontawesome**のフォントを使用しています。bulmaにはfontawesomeが元から組み込まれていますので改めて追加する必要はありません。

ここではbulmaのヘルパークラスの`icon`クラスを使用していますが、これはアイコンフォント読み込み中に、アイコンの表示されるべき領域が潰れないように、スペースをあらかじめ確保しておくことで、いわゆる**Jumping[^jumpping]**を防ぐためのものです。このレイアウトなら有っても無くてもどちらでも構いません。

[^jumpping]: Jumping(ジャンピング)とは例えになりますが、広告枠とかが時間差で表示されて、それに伴って突然レイアウトが変わって、クリックしたい位置がズレてイラッとするアレのことを言います。(この名称って一般的なの？)

アイコン名やアイコンイメージ等は、[Search v5 Icons](https://fontawesome.com/v5/search?o=r&m=free){: data-ext="true"}から確認できます。  
一つ注意点としてbulmaのFontAwesomeはVer5のfree版なので、中には表示できないアイコンもあります。

```yml
twitter_username: lazyfox1974
github_username:  lazyfox1974
```
{: .hljs data-title="_config.yml"}
それそれのアカウント名は変更しやすいように変数で記述して、実体は_config.ymlで設定するようにしています。

***
