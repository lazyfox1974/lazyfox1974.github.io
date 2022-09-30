---
layout: post
title: jekyllのカスタマイズ(その他)
description: 
date: 2022-09-21 23:45:00 +0900
last_modified_at: 2022-09-30 17:00:00 +0900
tags: jekyll
image: "/img/20220921_file-not-found.png"
comments: true
summary: その他の設定を紹介します
---
今回はタグラベル関連とその他の設定について説明します。  
このテーマではタグラベルはあるのですが、**タグ別ポスト一覧ページ**や**タグ一覧**は実装されていないので自作する必要があります。

## 設定
### タグラベルにページへのリンクを追加
```html
{% raw %}<span class="tag {% if include.style %} {{ include.style }} {% else %} is-primary {% endif %}">
    <a href="{{ site.tag_path }}/#{{ include.tag | slugify }}">     <!-- このアンカータグを追加 -->
        {{ include.tag }}
    </a>
</span>{% endraw %}
```
{: .hljs data-title="_includes/tag.html"}
まず、サイト内の各所で使用されているタグラベルのパーツにちょっと手を加えます。  

タグラベルはこのテーマではパーツ`_includes/tag.html`として用意されているのでそれを弄ります。  
タグラベル自体は**bulmaフレームワーク**標準のクラスが使用されていますが、これはただのラベルにすぎないのでリンクを張れるようにアンカータグを仕込みます。  
リンク先は後で作成する**タグ別ポスト一覧ページ**`tag/index.html`にリンクされています。あとページ内リンク用にタグ名(slug化済)でIDを追加しています。  
(変数`tag_path`は`_config.yml`内で定義しています。)
### タグ別ポスト一覧ページを追加
```yml
{% raw %}---
layout: tag-list
title: tag
subtitle: タグ一覧
show_sidebar: false
---{% endraw %}
```
{: .hljs data-title="tag/index.html"}
上記の**タグ別ポスト一覧ページ**を`tag/index.html`のパスに作ります。  
内容についてはタイトルなどの設定をするだけに留め、パーツの配置等はレイアウトファイル`tag_list.html`に分離します。  
但しこのページに文言を加えたい場合は本文に追記してください。

```html
{% raw %}---
layout: default
---
{{ content }}
<div class="content post-list">
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    <h3 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h3>
    <ul>
      {% assign sorted_posts = tag[1] | sort: "date" %}
      {% for post in sorted_posts %}
        <li><a href="{{ post.url }}">{{ post.title }}</a><span> - {{ post.date | date_to_xmlschema | date: "%Y/%m/%d" }}</span></li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>{% endraw %}
```
{: .hljs data-title="_layouts/tag_list.html"}
新たにレイアウトファイル`tag_list.html`を作ります。タグ別ポスト一覧は、公式サイトの
[これ](http://jekyllrb-ja.github.io/docs/posts/#%E3%82%BF%E3%82%B0){:data-ext="true"}
を参考にしました。  
変更点はタグをアルファベット順にソートしたのと、それぞれの投稿記事を古い順から並ぶようにソートしました。あとはタグの見出しにサイト内リンク用の`id`属性を付与しています。

本文冒頭にある{% raw %}`{{ content }}`{% endraw %}は、このレイアウトファイルの参照元`tag/index.html`の本文に書いた内容がここに展開されます。
```scss
.post-list {
    li{
        list-style-type: none;
        a{
            font-weight: bold;
        }
        span{
            color: $grey;
        }
    }
}
```
{: .hljs data-title="_scss/mystyle.scss"}
仕上げに見栄えを良くするため簡単にスタイル付けをしています。

### タグ一覧カード追加
```html
{% raw %}<div class="card">
    <header class="card-header">
        <a class="card-header-title" href="{{ site.tag_path }}/">タグ一覧</a>
    </header>
    <div class="card-content">
        <div class="content tags">
            {% assign sorted_tags = site.tags | sort %}
            {% for tag in sorted_tags %}
            {% assign tagname=tag[0] %}
                {% include tag.html tag=tagname style="is-info is-light is-medium"%}
            {% endfor %}
        </div>
    </div>
</div>{% endraw %}
```
{: .hljs data-title="_includes/tag-card.html"}
ブログ記事の右側にある**サイドバー**に**タグ一覧**を作ります。  
`_includes`ディレクトリに`tag-card.html`というファイルを作ります。(サイトのパーツなのでfront matterは不要です)  
今回はbulmaフレームワークのクラスの`Cardコンポーネント`と`Tagsコンテナ`を使用します。デザイン済みのコンポーネントなので、マージンやパディングなど細かいことは気にせずにforループでタグラベルを貼りつけていきます。

```html
…省略
{% raw %}        <div class="column {{ content_width }}">
            {% include tabs.html %}
            {% include showcase.html %}
            {% include sponsors.html %}
            {% include gallery.html %}
            {{ content }}
        </div>
        {% if site.posts and page.show_sidebar %}
        <div class="column is-4-desktop is-4-tablet">
            {% include latest-posts.html %}
            {% include tag-card.html %}     <!-- タグ一覧カードをここに追加 -->
        </div>
        {% endif %}{% endraw %}
以降省略…
```
{: .hljs data-title="_layouts/default.html"}
あとは、デフォルトレイアウトにサイドバー内に表示する内容を設定するところがあるので`latest-posts.html`の後にインクルードします。

### 404ページの追加
```yml
---
title: 404
subtitle:  File not found???
layout: page
show_sidebar: false
permalink: /404.html
---
```
{: .hljs data-title="404.html"}
最後にオリジナルの404ページを作りました。デフォルトだと**GitHub Pages**のものが表示され、テーマの統一性がなくなってしまうため自作しました。あとSEO的にも有利だとか…  
作り方は簡単、**GitHub Pages**ではルートのディレクトリに`404.html`という名前のファイルを置くだけです。

front matterで単一ページ用のレイアウトの`page`を設定。所定の場所にファイルを配置するために`permalink: /404.html`とします。  
あとは、本文を適当に作って…

![NotfoundPage](/img/20220921_file-not-found.png "404ページ"){:width="50%" .dropshadow}

↑こんな感じになりました。

今回で設定の説明は終了です。本当はもっと細かい所も弄っているんですが、その辺は直接GitHubの[リポジトリ](https://github.com/lazyfox1974/lazyfox1974.github.io){:data-ext=true}の方を見てください。

***
