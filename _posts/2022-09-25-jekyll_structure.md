---
layout: post
title: jekyllの変数のデータ構造
description: jekyllのテーマなどを弄る際、変数(オブジェクト)のデータ構造を知る必要があるので、それについて説明します。
date: 2022-09-25 12:00:00 +0900
last_modified_at: 2022-12-31 00:00:00 +0900
tags: jekyll 解析してみた
image: "/img/20220925_structure.png"
comments: true
summary: jekyllの変数のデータ構造を紹介します
---
`liquid`で`jekyll`の変数を色々弄るのに、どんなデータ構造の構造体になっているのか気になったので調べてみました。

## 前提
`GitHub Pages`で使用されているバージョン`jekyll 3.9.2`を前提に説明します。が、`jekyll 4.0`以降でもあまり変わりがないようです。(キャッシュの設定が追加されただけ？)

データ構造についてはJavascriptライクな似非言語で書いています。自分が理解するためだけに書いたので適当です。  
大文字で書かれている変数名やオブジェクト名は、説明のため当方で勝手に付けた名称ですので注意してください。

調べるにあたって、以下のjekyllの公式ドキュメントサイトを参考にしています。
* [変数一覧](http://jekyllrb-ja.github.io/docs/variables/){:data-ext="true"}
* [デフォルト設定](http://jekyllrb-ja.github.io/docs/configuration/default/){:data-ext="true"}
* [オプション設定](http://jekyllrb-ja.github.io/docs/configuration/options/){:data-ext="true"}
* [Markdownオプション](http://jekyllrb-ja.github.io/docs/configuration/markdown/){:data-ext="true"}
* [Liquidオプション](http://jekyllrb-ja.github.io/docs/configuration/liquid/){:data-ext="true"}
* [Front Matter](http://jekyllrb-ja.github.io/docs/front-matter/){:data-ext="true"}
* [コレクション](http://jekyllrb-ja.github.io/docs/collections/){:data-ext="true"}
* [静的ファイル](http://jekyllrb-ja.github.io/docs/static-files/){:data-ext="true"}
* [ページネーター](http://jekyllrb-ja.github.io/docs/pagination/){:data-ext="true"}

実際に動作を確認しながら書いていますが、誤りがあるかもしれません。その点はご了承ください。

## データ構造一覧
全てのオブジェクトはグローバルの`layout`、`page`、`paginator`、`site`のいずれかにぶら下がる構成となっています。
```javascript
GLOBAL = {
  "content"           : <string>,
  "layout"            : LAYOUT<object>,
  "page"              : PAGE<object>,
  "paginator"         : PAGINATOR<object>,
  "site"              : SITE<object>
};

LAYOUT = {
  "layout"            : <string>
  // other custom parameter start (ex.)
    ...etc (custom parameter)
  // other custom parameter end
}

PAGE = {
  "content"           : <string>,
  "output"            : <string>,
  "next"              : PAGE<object>
  "previous"          : PAGE<object>
  "id"                : <string>,
  "relative_path"     : <string>,
  "collection"        : <string>,
  "categories"        : <array> , // => [CATEGORY_NAME<string>, ...];
  "draft"             : <bool>  ,
  "tags"              : <array> , // => [TAG_NAME<string>, ...];
  "slug"              : <string>,
  "ext"               : <string>,
  "dir"               : <string>,
  "name"              : <string>,
  "url"               : <string>, 
  "path"              : <string>,
  "title"             : <string>,
  "layout"            : <string>,
  "date"              : <string>,
  "excerpt"           : <string>,
  // front matter start (ex.)
  "image"             : <string>,
  "author"            : <string>,
  "description"       : <string>,
  "summary"           : <string>,
    ...etc
  // front matter end
}

PAGINATOR = {
  "page"              : <integer>,
  "per_page"          : <integer>,
  "posts"             : <array>  , // => [PAGE<object>, ...];
  "total_posts"       : <integer>,
  "total_pages"       : <integer>,
  "previous_page"     : <integer>,
  "previous_page_path": <string>,
  "next_page"         : <integer>,
  "next_page_path"    : <string>
}

SITE = {
  "tags"              : <array> , // => [TAG<array>, ...];  TAG => [TAG_NAME<string>, PAGES<array>]; PAGES => [PAGE<object>, ...];
  "time"              : <string>,
  "pages"             : <array> , // => [PAGE<object>, ...];
  "posts"             : <array> , // => [PAGE<object>, ...];
  "documents"         : <array> , // => [PAGE<object> or STATIC_FILE<object>, ...];
  "related_posts"     : <array> , // => [PAGE<object>, ...];
  "html_pages"        : <array> , // => [PAGE<object>, ...];
  "static_files"      : <array> , // => [STATIC_FILE<object>, ...];
  "categories"        : CATEGORIES<object> , 
  "collections"       : <array> , // => [COLLECTION<object>, ...];
  "data"              : DATA<object>,
  "source"            : <string>,
  "destination"       : <string>,
  "collections_dir"   : <string>,
  "plugins_dir"       : <string>,
  "layouts_dir"       : <string>,
  "data_dir"          : <string>,
  "includes_dir"      : <string>,
  "safe"              : <bool>  ,
  "include"           : <array> , // => [FILE_DIR_NAME<string>, ...];
  "exclude"           : <array> , // => [FILE_DIR_NAME<string>, ...];
  "keep_files"        : <array> , // => [FILE_DIR_NAME<string>, ...];
  "encoding"          : <string>,
  "markdown_ext"      : <string>,
  "strict_front_matter": <bool> ,
  "show_drafts"       : <bool>  ,
  "limit_posts"       : <integer>,
  "future"            : <bool>  ,
  "unpublished"       : <bool>  ,
  "whitelist"         : <array> , // => [WITELIST_NAME<string>, ...];
  "plugins"           : <array> , // => [PLUGINS_NAME<string>, ...];
  "markdown"          : <string>,
  "highlighter"       : <string>,
  "lsi"               : <bool>  ,
  "excerpt_separator" : <string>,
  "incremental"       : <bool>  ,
  "detach"            : <bool>  ,
  "port"              : <string>,
  "host"              : <string>,
  "baseurl"           : <string>,
  "show_dir_listing"  : <bool>  ,
  "permalink"         : <string>,
  "paginate_path"     : <string>,
  "timezone"          : <string>,
  "quiet"             : <bool>  ,
  "verbose"           : <bool>  ,
  "defaults"          : <array>,  // => [YAML<object>, ...];
  "liquid"            : LIQUID<object>,
  "kramdown"          : KRAMDOWN<object>,
  "livereload_port"   : <integer>,
  "serving"           : <bool>  ,
  "watch"             : <bool>  ,
  "theme"             : <string>,
  "url"               : <string>,
  // _config.yml other custom parameter start (ex.)
  "lang"              : <string>,
  "title"             : <string>,
  "tagline"           : <string>,
  "description"       : <string>,
  "domain"            : <string>,
  "favicon"           : <string>,
  "theme_color"       : <string>,
  "paginate"          : <integer>,
    ...etc (custom parameter)
  // _config.yml other custom parameter end
};

CATEGORIES = {
  CATEGORY_NAME       : <array>, // => [PAGE<object>, ...];
  ...
};

DATA = {
  YAML_FILE_NAME      : YAML_OBJECT<object>,
  ...
};
YAML_OBJECT = #parse_from_file_to_object(YAML_FILE_NAME.yml);

COLLECTIONS = {
  "output"            : <bool>  ,
  "label"             : <string>,
  "directory"         : <string>,
  "docs"              : <array> ,// => [DOC<strings>, ...];
  "relative_directory": <string>,
  "files"             : <array> ,// => [STATIC_FILE<object>, ...];
};

STATIC_FILE = {
  "modified_time"     : <string>,
  "name"              : <string>,
  "basename"          : <string>,
  "extname"           : <string>,
  "collection"        : <string>,
  "path"              : <string>
}

LIQUID = {
  "error_mode"        : <string>,
  "strict_filters"    : <bool>  ,
  "strict_variables"  : <bool>  ,
  ...
}

KRAMDOWN = {
  "auto_ids"          : <bool>,
  "entity_output"     : <string>,
  "toc_levels"        : <string>,
  "smart_quotes"      : <string>,
  "input"             : <string>,
  "hard_wrap"         : <bool>  ,
  "footnote_nr"       : <integer>,
  "show_warnings"     : <bool>  ,
  ...
}
```

## 初期値と詳細説明
### グローバル変数

|キー          |型      |初期値     |説明           |
| ----------- |:------:|:--------:| ------------ |
|**content**  |`string`| -        |現在のページの本文|
|**layout**   |`object`| -        |**LAYOUTオブジェクト**<br>現在のページのレイアウトに関するオブジェクト|
|**page**     |`object`| -        |**PAGEオブジェクト**<br>現在のページに関するオブジェクト|
|**paginator**|`object`| _null_   |**PAGINATORオブジェクト**<br>現在のページのページネーターに関するオブジェクト。ページ分けしていなければ`null`|
|**site**     |`object`| -        |**SITEオブジェクト**<br>サイトの設定に関するオブジェクト|
{: .table .is-narrow}

### LAYOUTオブジェクト

|キー           |型      |初期値     |説明           |
| ------------ |:------:|:--------:| ------------ |
|**layout**    |`string`| -        |layoutの再帰的参照で、一番最後にたどり着いたレイアウト名|
|_custom param_|        |          |レイアウトファイル内で宣言された変数の集合|
|　**custom**  |`string`| -        |(例)|
|　　...etc     |        |          |        |
|_end_         |        |          |        |
{: .table .is-narrow}

### PAGEオブジェクト

|キー               |型      |初期値  |説明           |
| ---------------- |:------:|:-----:| ------------ |
|**content**       |`string`| -     |現在のページの本文|
|**output**        |`string`| _null_|\<html>を含む全ての内容<br>【postを含むコレクションに属するページのみ】|
|**next**          |`object`| _null_|**PAGEオブジェクト**<br>`site.posts`順で次の記事のオブジェクト。存在しない場合は`null`<br>【postを含むコレクションに属するページのみ】|
|**previous**      |`object`| _null_|**PAGEオブジェクト**<br>`site.posts`順で前の記事のオブジェクト。存在しない場合は`null`<br>【postを含むコレクションに属するページのみ】|
|**id**            |`string`| -     |各ページを識別する一意なID<br>【postを含むコレクションに属するページのみ】|
|**relative_path** |`string`| -     |現在のブログ記事(post)の元ファイルへの相対パス<br>【postを含むコレクションに属するページのみ】|
|**collection**    |`string`| -     |現在のページが属するコレクション名。ブログ記事は`posts`となる<br>【postを含むコレクションに属するページのみ】|
|**categories**    |`array` | _[ ]_ |**CATEGORY_ARRAY** => [CATEGORY_NAME\<string>, ...];<br>現在のブログ記事(post)が属するカテゴリ名のリスト<br>【postを含むコレクションに属するページのみ】|
|**draft**         |`bool`  |_false_|草稿記事かどうかのフラグ。`_drafts`内の記事にはフラグが立つ<br>【postを含むコレクションに属するページのみ】|
|**tags**          |`array` | _[ ]_ |**TAG_ARRAY** => [TAG_NAME\<string>, ...];<br>現在のブログ記事(post)が属するタグ名のリスト<br>【postを含むコレクションに属するページのみ】|
|**slug**          |`string`| -     |現在のブログ記事(post)の元ファイルの拡張子を除いたファイル名<br>【postを含むコレクションに属するページのみ】|
|**ext**           |`string`| -     |現在のブログ記事(post)の元ファイルの拡張子<br>【postを含むコレクションに属するページのみ】|
|**dir**           |`string`| -     |現在のページの元ファイルがあるディレクトリへのパス<br>【コレクションに属さない静的ページのみ】|
|**name**          |`string`| -     |現在のページの元ファイルのファイル名<br>【コレクションに属さない静的ページのみ】|
|**url**           |`string`| -     |現在のページのドメインを含まないURL|
|**path**          |`string`| -     |現在のページの元ファイルへのパス|
|**title**         |`string`| -     |(front matterで設定)現在のページのタイトル名|
|**layout**        |`string`| -     |(front matterで設定)現在のページのレイアウト名|
|**date**          |`string`| -     |(front matterで設定)現在のページに割り付けられた日時|
|**excerpt**       |`string`| -     |(front matterで設定)現在のページの抜粋|
|_front matter_    |        |       |現在のページのfront matterで宣言された変数の集合|
|　**image**       |`string`| -     |(例)        |
|　**author**      |`string`| -     |(例)        |
|　**description** |`string`| -     |(例)        |
|　**summary**     |`string`| -     |(例)        |
|　　...etc        |        |       |        |
|_end_             |        |      |        |
{: .table .is-narrow}

### PAGINATORオブジェクト

|キー                    |型       |初期値    |説明           |
| --------------------- |:-------:|:-------:| ------------ |
|**page**               |`integer`| -       |現在のページのページ番号|
|**per_page**           |`integer`| -       |１ページに表示される記事数|
|**posts**              |`array`  | -       |**PAGE_ARRAY** => [PAGE\<object>, ...];<br>現在のページに表示されている記事のPAGEオブジェクトのリスト|
|**total_posts**        |`integer`| -       |記事の総数|
|**total_pages**        |`integer`| -       |ページ分けされたページの総数|
|**previous_page**      |`integer`| _null_  |前のページのページ番号|
|**previous_page_path** |`string` | _null_  |前のページへのパス|
|**next_page**          |`integer`| _null_  |次のページのページ番号|
|**next_page_path**     |`string` | _null_  |次のページへのパス|
{: .table .is-narrow}

### SITEオブジェクト

|キー                |型       |初期値         |説明           |
| ----------------- |:-------:|:------------:| ------------ |
|**tags**           |`array`  | -            |**TAGS_ARRAY** => [TAG\<array>, ...];<br>TAG => [TAG_NAME\<string>, PAGES\<array>];<br>PAGES => [PAGE\<object>, ...];<br>`page.tags`とは異なるので注意|
|**time**           |`string` | -            |`jekyll`を起動した日時|
|**pages**          |`array`  | -            |**PAGE_ARRAY** => [PAGE\<object>, ...];<br>投稿記事以外の閲覧可能な静的なページとCSSやsitemapなどの機械可読なページのオブジェクトのリスト<br>(jekyllのドキュメントサイトの説明と違う？)|
|**posts**          |`array`  | -            |**PAGE_ARRAY** => [PAGE\<object>, ...];<br>全投稿記事のページのオブジェクトのリスト(新しい順)|
|**documents**      |`array`  | -            |**PAGE_ARRAY** => [PAGE\<object> or STATIC_FILE\<object>, ...];<br>postを含む全てのコレクションのドキュメントのオブジェクトのリスト|
|**related_posts**  |`array`  | -            |**PAGE_ARRAY** => [PAGE\<object>, ...];<br>関連するページのオブジェクトのリスト<br>変数を解釈しているベージが投稿記事(post)の場合のみ有効。それ以外は`null`|
|**html_pages**     |`array`  | -            |**PAGE_ARRAY** => [PAGE\<object>, ...];<br>投稿記事以外の閲覧可能な静的なページのオブジェクトのリスト|
|**static_files**   |`array`  | -            |**STATIC_FILES_ARRAY** => [STATIC_FILE\<object>, ...];<br>`jekyll`や`liquid`で処理されない画像ファイルやスクリプトなどのオブジェクトのリスト|
|**categories**     |`object` | -            |**CATEGORIESオブジェクト**<br>カテゴリー別ページ一覧のオブジェクト<br>`page.categories`とは異なるので注意|
|**collections**    |`array`  | -            |**COLLECTIONS_ARRAY** => [COLLECTION\<object>, ...];<br>postを含むコレクションのオブジェクトのリスト|
|**data**           |`object` | -            |**DATAオブジェクト**<br>`_data`フォルダ内のymlファイルの内容が入る|
|**source**         |`string` | _"."_        |`jekyll`が読み込みを行うルートディレクトリ。`_config.yml`があるディレクトリ？|
|**destination**    |`string` | _"./_site"_  |`jekyll`が書き出しを行うルートディレクトリ|
|**collections_dir**|`string` | _"."_        |コレクションがまとめられているディレクトリが存在しているディレクトリのパス|
|**plugins_dir**    |`string` | _"_plugins"_ |プラグインファイルが存在するディレクトリの名称|
|**layouts_dir**    |`string` | _"_layouts"_ |レイアウトファイルが存在するディレクトリの名称|
|**data_dir**       |`string` | _"_data"_    |`site.data`で参照されるファイルが存在するディレクトリの名称|
|**includes_dir**   |`string` | _"_includes"_|`include`コマンドで用いられるファイルが存在するディレクトリの名称|
|**safe**           |`bool`   | _false_      |`true`で`whitelist`にないカスタムプラグインを無効化|
|**include**        |`array`  | =>           |_[".htaccess"]_<br>`jekyll`の実行時に強制的に変換に含めるファイルやディレクトリのリスト|
|**exclude**        |`array`  | =>           |_["Gemfile", "Gemfile.lock", "node_modules", "vendor/bundle/", "vendor/cache/", "vendor/gems/", "vendor/ruby/"]_<br>`jekyll`の実行時に変換から除外するファイルやディレクトリのリスト<br>ドットファイルはこの設定如何によらず基本除外される|
|**keep_files**     |`array`  | =>           |_[".git", ".svn"]_<br>サイト生成時残しておきたいファイルやディレクトリのリスト<br>サイト生成のたび`_site`の内容は消去されるので、消去から除外する設定|
|**encoding**       |`string` | _"utf-8"_    |ファイルの文字エンコーディングの設定|
|**markdown_ext**   |`string` | =>           |_"markdown,mkdown,mkdn,mkd,md"_<br>マークダウン記述ファイルであることを認識させる拡張子一覧|
|**strict_front_matter**|`bool`| _false_     |`front matter`にYMAL構文エラーがある場合ビルドを停止させるかどうかのフラグ|
|**show_drafts**    |`bool`   | _false_      |草稿(draft)を出力するかのフラグ|
|**limit_posts**    |`integer`| _0_          |解析および公開する投稿数の制限。"0"に設定すると無制限？|
|**future**         |`bool`   | _false_      |未来の日付のページも出力するかのフラグ|
|**unpublished**    |`bool`   | _false_      |`front matter`内で、`published: false`とした記事でも出力するかのフラグ|
|**whitelist**      |`array`  | _[ ]_        |**WHITELIST_ARRAY** => [WITELIST_NAME\<string>, ...];<br>ホワイトリストとして登録したプラグインのリスト|
|**plugins**        |`array`  | _[ ]_        |**PLUGINS_ARRAY** => [PLUGINS_NAME\<string>, ...];<br>プラグインのリスト|
|**markdown**       |`string` | _"kramdown"_ |`jekyll`で使用するMarkdownレンダラー|
|**highlighter**    |`string` | _"rouge"_    |`jekyll`で使用する構文ハイライト|
|**lsi**            |`bool`   | _false_      |関連記事のインデックスを作成するかのフラグ<br>`classifier-rebornプラグイン`が必要。GitHub Pagesでは非対応|
|**excerpt_separator**|`string`| _"\n\n"_    |デフォルトでは本文冒頭から最初の段落`\n\n`があるところまでを抜粋と設定|
|**incremental**    |`bool`   | _false_      |【実験的機能】インクリメンタルビルド機能を有効化するかどうかのフラグ<br>変更が加えられたファイルに関連するページのみを再ビルドする機能|
|**detach**         |`bool`   | _false_      |端末からサーバーを切り離すかどうかのフラグ<br>`ture`で`jekyll`のプロセスをバックグランドで実行させる<br>【`jekyll server`時のみ有効】|
|**port**           |`string` | _"4000"_     |内蔵webサーバーのポート番号<br>【`jekyll server`時のみ有効】|
|**host**           |`string` | _"127.0.0.1"_|内蔵webサーバーのホスト名<br>【`jekyll server`時のみ有効】|
|**baseurl**        |`string` | _""_         |生成したファイルを公開サーバーのどこのディレクトリに配置するかを設定<br>`baseurl: "sample"`とした場合、`http://domain/sample/`となる|
|**show_dir_listing**|`bool`  | _false_      |ディレクトリリストを表示させるかどうかのフラグ<br>`true`とすると`http://domain/`にアクセスした場合`http://domain/index.html`の代わりにディレクトリリストを表示する<br>【`jekyll server`時のみ有効】|
|**permalink**      |`string` | _"date"_     |`front matter`で設定されなかった場合のパーマリンクの設定|
|**paginate_path**  |`string` | _"/page:num"_|ページ分けされたページの出力先|
|**timezone**       |`string` | _null_       |タイムゾーンの設定。設定がない場合、環境変数`TZ`を継承|
|**quiet**          |`bool`   | _false_      |ビルド時のログを出力させないかどうかのフラグ|
|**verbose**        |`bool`   | _false_      |ビルド時に詳細なログを出力するかどうかのフラグ|
|**defaults**       |`array`  | _[ ]_        |**DEFAULTS_ARRAY** => [YAML\<object>, ...];<br>`front matter`で設定しなかった場合のデフォルト値の設定<br>`_config.yml`内の`defaults:`のyamlオブジェクトの内容が格納されている|
|**liquid**         |`object` | -            |**LIQUIDオブジェクト**<br>`liquid`の動作に関する設定のオブジェクト|
|**kramdown**       |`object` | -            |**KRAMDOWNオブジェクト**<br>`kramdown`の動作に関する設定のオブジェクト|
|**livereload_port**|`integer`| _35729_      |livereloadを通知するポートの設定<br>【`jekyll server`時のみ有効】|
|**serving**        |`bool`   | -            |`jekyll server`で起動されているかのフラグ|
|**watch**          |`bool`   | _true_       |ファイルに変更が加えられた時に自動再生成を行うかのフラグ<br>【`jekyll server`時のみ有効】|
|**theme**          |`string` | -            |現在使用しているテーマの名称|
|**url**            |`string` | -            |サイトを公開するドメインのurl<br>`jekyll server`で実行された場合`site.host`と`site.port`で設定された値が入る|
|*_config.yml*      |         |              | _config.ymlで宣言された変数の集合 |
|　**lang**          |`string`| -            |(例)        |
|　**title**         |`string`| -            |(例)        |
|　**tagline**       |`string`| -            |(例)        |
|　**description**   |`string`| -            |(例)        |
|　**domain**        |`string`| -            |(例)        |
|　**favicon**       |`string`| -            |(例)        |
|　**theme_color**   |`string`| -            |(例)        |
|　**paginate**      |`integer`| -           |(例)        |
|　　...etc          |        |              ||
|_end_               |       |               ||
{: .table .is-narrow .is-fullwidth}

### COLLECTIONオブジェクト

|キー           |型      |初期値    |説明           |
| ------------ |:------:|:-------:| ------------ |
|**output**    |`bool`  | _false_ |コレクションに属するページを個別ファイルとして出力するかのフラグ<br>`label="posts"`はデフォルトで`true`|
|**label**     |`string`| -       |コレクションの名称|
|**directory** |`string`| -       |コレクションがまとめられているディレクトリへのフルパス|
|**docs**      |`array` | _[ ]_   |**DOCS_ARRAY** => [DOC\<strings>, ...];<br>コレクションに属するページの本文のリスト |
|**relative_directory**|`string`| -|コレクションがまとめられているディレクトリへのサイトからの相対パス|
|**files**     |`array` | _[ ]_   |**FILES_ARRAY** => [STATIC_FILE\<object>, ...];<br>コレクションに属する静的ファイルのオブジェクトのリスト|
{: .table .is-narrow}

### STATIC_FILEオブジェクト

|キー              |型      |初期値  |説明          |
| --------------- |:------:|:-----:| ------------ |
|**modified_time**|`string`| -     |ファイルの更新日時のタイムスタンプ|
|**name**         |`string`| -     |ファイルのファイル名|
|**basename**     |`string`| -     |ファイルの拡張子を含まないファイル名|
|**extname**      |`string`| -     |ファイルの拡張子|
|**collection**   |`string`|_null_ |ファイルが属するコレクション名|
|**path**         |`string`| -     |ファイルへのパス|
{: .table .is-narrow}

### LIQUIDオブジェクト

|キー                |型       |初期値    |説明          |
| ------------------ |:------:|:-------:| ------------ |
|**error_mode**      |`string`| _"warn"_|liquid解釈中に例外をキャッチした場合の反応の設定<br>デフォルトでは警告を出力し解釈は継続|
|**strict_filters**  |`bool`  | _false_ |存在しないフィルターが呼ばれた場合の反応の設定<br>`true`で例外をthrowする|
|**strict_variables**|`bool`  | _false_ |定義されていない変数が参照された場合の反応の設定<br>`true`で例外をthrowする|
|　...etc          |         |             ||
{: .table .is-narrow}

### KRAMDOWNオブジェクト

|キー              |型       |初期値        |説明          |
| --------------- |:-------:|:-----------:| ------------ |
|**auto_ids**     |`bool`   | _true_      |\<h1>, \<h2>などの見出しタグの自動的にIDを生成するかどうかのフラグ|
|**entity_output**|`string` | _"as_char"_ |デフォルトではhtmlエンティティは可能であれば文字として出力される|
|**toc_levels**   |`string` | _"1..6"_    |どのレベルまで目次に表示させるかを設定<br>"1..3"であれば第１階層から第３階層まで表示し、それ以下は省略する|
|**smart_quotes** |`string` | =>          |_"lsquo,rsquo,ldquo,rdquo"_<br>スマートクォートとして出力させるhtmlエンティティを設定|
|**input**        |`string` | _"GFM"_     |デフォルトでは`GitHub Flavored Markdown`をサポート<br>GFMを外したい場合`input: Kramdown`と設定|
|**hard_wrap**    |`bool`   | _false_     |改行を改行として解釈するかどうかのフラグ|
|**footnote_nr**  |`integer`| _1_         |最初の注釈番号の設定|
|**show_warnings**|`bool`   | _false_     |???|
|　...etc          |         |             ||
{: .table .is-narrow}

***