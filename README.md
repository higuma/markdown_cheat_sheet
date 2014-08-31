# Markdown cheat sheet (2nd edition)

> これはもともとMarkdown勉強時の個人的な備忘録として書いたものですが、気が付いたらもうチートシートではなく詳細解説になってしまいました。文法の細かい部分の確認にご利用下さい。
> 
> 全体の構成としては最初にコードを示し、次にその表示イメージを実際に示してどのように変換されるか目で分かるようにしています。表示結果は引用を使って他の部分と区別しています(特にheaderの説明時に混乱を避けるため)。
> 
> > 最初はテキスト文書として書き始めたものですが、途中からこの文章自体もMarkdownで書き直しました(それ自体とてもよい演習です)。どのように記述してあるかは本文のソースをRawモードで見れば分かります。

## リファレンス

Markdownリファレンス(Daring Fireball)

* 原文: <http://daringfireball.net/projects/markdown/syntax>
* 和訳: ~~<http://blog.2310.net/archives/6>~~ (デッドリンク)

GitHub Flavored Markdown (Github拡張)

* <https://help.github.com/articles/github-flavored-markdown>

Markdown記法 チートシート (Qiita拡張)

* <http://qiita.com/Qiita/items/c686397e4a0f4f11683d>

> 勉強開始時は文法確認にオンラインプレビューを使うとよい。探せば色々見つかるがおすすめは次のサイトで、左右で原文と変換結果を比較でき、出力もほぼgithubと同じようにフォーマットして表示してくれる。
> 
> <http://tmpvar.com/markdown.html>
> 
> > (追記) Qiitaの投稿プレビューはもっと便利で、(後で説明する)GithubやQiitaの拡張文法もすべて認識します。
> 
> またローカルファイルシステム上のMarkdown文章は次のサイトを使えばプレビューできる(これも便利)。ページにアクセスしてタイトル部分にファイルをドロップすればブラウザで閲覧できる。
> 
> <http://playground.k11i.biz/mp/>

以下Markdownリファレンスの章立ての順に説明する。

## Overview

### Inline HTML

Markdownは基本的にホームページを簡単に書くことを目的としており、HTMLでよく使われる書式をHTMLより容易に記述できる。Markdownで表現できない書式はHTMLタグを使えば表現できる。

ブロック要素(`<div>` `<table>` `<pre>` `<p>`など)は次のルールで記述すれば認識する。

* 前後に空行を置く
* タグは行頭に置く(手前にスペースやタブを入れない)

このように記述したブロック要素の内部ではMarkdownの記法が適用されない。次は(後で説明する)強調表示の例(これは通常のMarkdown)。

    This is *emphasized*

次はその変換結果(表示イメージ)。多くのブラウザは斜体で表示する。

> This is *emphasized*

しかし次の例では`<p>`はブロック要素なので内部では強調の`*...*`が適用されない。

    <p> This is *not emphasized*</p>

結果は次の通り。

> <p> This is *not emphasized*</p>

インライン要素は行頭に置く必要はなく、文章の途中にそのまま挿入できる。

    This is <span style="color:red">red</span>

> This is <span style="color:red">red</span>

> > 補足: これを赤文字で表示する処理系が実際にありますが、Qiitaでは`style`属性は認識されません。

インライン要素内ではMarkdown文法が有効になる。

    This is <span style="color:green">green and *emphasized*</span>

> This is <span style="color:green">green and *emphasized*</span>

> > 補足: Qiitaでは`style`は効きません。

### Automatic Escaping for Special Characters

HTMLでは`<`や`&`は特殊文字であり、文字実体参照を使って`&lt;`や`&amp;`などと書く必要がある。Markdownではこれらの特殊文字を自動変換して自然に処理する。

* `&copy;` =\> &copy; (コピーライトマークの文字実体参照)
* `AT&T` =\> AT&T (`AT&amp;T`に自動変換)
* `4 < 5` =\> 4 < 5 (`4 &lt; 5`に自動変換)

## Block Elements

### Paragraphs and Line Breaks

上下に空白行がある文章はひとつの段落として認識される。ここでの空白行は次の性質を持つ行(のまとまり)を指す。空白行の連続はひとつの段落区切りとして認識される。

* 空行
* 空白文字だけの行
* 上記2つの連続

空行でない行は同じ段落として認識される。次の例では一行目と二行目の間は改行されず、連続した文章として認識される。また途中の空行はいくつ連続してもひとつの段落区切りと認識される。

    This is a 1st paragraph.
    Rest of 1st Paragraph.


    Second Paragraph

> This is a 1st paragraph.
> Rest of 1st Paragraph.
> 
> 
> Second Paragraph

### Headers

ヘッダは2つの形式をサポートしている。最初の形式は次の通り。文章の直下の行が`=`または`-`の場合はそれぞれ`<h1>`と`<h2>`に対応する。

    This is an H1
    =============

    This is an H2
    -------------

変換結果は次の通り(以下は表示サンプル: 本物の見出しではないので注意)。

> This is an H1
> =============

> This is an H2
> -------------

===や---の長さは文字の長さと関係ない。次でも同じ結果になる。

    This is an H1
    =

    This is an H2
    ---------------------------

> This is an H1
> =

> This is an H2
> ---------------------------

どちらも次のHTMLに変換される。

>     <h1>This is an H1</h1>
>     <h2>This is an H2</h2>

2つ目の形式は先頭に#をレベルの数だけ置く。この形式は`<h3>`以上も表現できる。

    # h1

    #### h4

> # h1

> #### h4

タイトル文章の後に#を置いてもよい。これらは単に無視される。

    ## h2 ##

    ### item one #####
    ### item two #####
    ### item three ###

> ## h2 ##

> ### item one #####
> ### item two #####
> ### item three ###

### Blockquotes

メールと同じように先頭の`>`は引用(blockquote)として解釈される。次のように毎行の頭に`>`を書くと連続した引用として認識する。

    > This is a blockquote.
    >
    > foo
    > bar

> This is a blockquote.
>
> foo
> bar

連続した複数行に渡る文章は先頭に`>`を書けば全体をひとつの引用の段落と認識する。

    > This is a single
    blockquote paragraph.

> This is a single
blockquote paragraph.

引用は入れ子を認識する。

    > This is the first level of quoting.
    > 
    > > This is nested blockquote.
    > 
    > Back to the first level.

> This is the first level of quoting.
> 
> > This is nested blockquote.
> 
> Back to the first level.

引用の中でもMarkdown文法を使うことができる。

    > ## This is a header.
    >
    > 1. This is the first list item.
    > 2. This is the second list item.
    >
    > Here's some example code:
    >
    >     return shell_exec("echo $input | $markdown_script");

> ## This is a header.
>
> 1. This is the first list item.
> 2. This is the second list item.
>
> Here's some example code:
>
>     return shell_exec("echo $input | $markdown_script");

## Lists

順序なしリスト(`<ul>`)は次のように表現する。

    * Red
    * Green
    * Blue

> * Red
> * Green
> * Blue

`*`の代わりに`+`または`-`を使ってもよい。これらに区別はなく、次は最初の例と全く同じように表示する。

    - Red
    + Green
    * Blue

> - Red
> + Green
> * Blue

どちらも次のHTMLを生成する。

    <ul>
    <li>Red</li>
    <li>Green</li>
    <li>Blue</li>
    </ul>

(本文に書いてないが)入れ子はインデントで表現する。タブ1つまたは半角スペース4つで認識する。

    * 1st
        * 2nd
            * 3rd
        * 2nd
            * 3rd

> * 1st
>     * 2nd
>         * 3rd
>     * 2nd
>         * 3rd

---

>     <ul>
>         <li>1st
>         <ul>
>             <li>2nd
>             <ul>
>                 <li>3rd</li>
>             </ul>
>             </li>
>             <li>2nd
>             <ul>
>                 <li>3rd</li>
>             </ul>
>             </li>
>         </ul>
>         </li>
>     </ul>

---

> インデントはスペース4つ(またはタブ1つ)が基本(Web上で読んだ記憶はあるがどこのサイトか思い出せない)。ただし(本家のPerl実装を始め)可能な限りエラーにはしないので詳細仕様は実装により異なると考えるべき。4の倍数にしておけば確実。
> 
> > ここではgithubが用いている実装をリファレンスとする。

---

> 先頭記号として`*`以外に`+`と`-`も使えるようにした理由はテキスト状態でツリー表示した場合を考慮したものだろう。次のような例を考えること。
> 
>     - usr
>         + bin
>         - games
>             + chess
>             + go
>             + mahjong
>             + tetris
>         + include
>         + lib

番号付きリスト(`<ol>`)は次のように表現する。

    1. Bird
    2. McHale
    3. Parish

> 1. Bird
> 2. McHale
> 3. Parish

ただし数値に意味はない。次でも同じ結果になる。

    1001. Bird
    332. McHale
    883. Parish

> 1001. Bird
> 332. McHale
> 883. Parish

これらはどちらも次のHTMLに変換される。

    <ol>
    <li>Bird</li>
    <li>McHale</li>
    <li>Parish</li>
    </ol>

> 元のHTMLに番号を設定する機能がないため現状ではこうするよりない。ただし将来番号を設定する機能が付く可能性も考えられるので、1から連番にしておけば間違いない。

リストの正式なルールは次の通り。手前に3個までスペースを置いてよい(4つ以上あるとインデントとして認識する)。

    [0...3個のスペース](*,+,-,または数値とピリオド)[1個以上のスペース]文章

次のようにインデントを使って書いてもよい。これはテキストとしての見やすさを考えた仕様だが、2行目以降の空白は単に無視する(作業の負担だと思ったらやめていい)。

    *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
        Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
        viverra nec, fringilla in, laoreet vitae, risus.
    *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
        Suspendisse id sem consectetuer libero luctus adipiscing.

> *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
>     Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
>     viverra nec, fringilla in, laoreet vitae, risus.
> *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
>     Suspendisse id sem consectetuer libero luctus adipiscing.

空行は段落と認識されることを忘れてはいけない。

    * Bird

    * Magic


このように空行があると段落とみなされ次のように変換される。

> * Bird
>
> * Magic

---

>     <ul>
>     <li><p>Bird</p></li>
>     <li><p>Magic</p></li>
>     </ul>

ここで`<li>...</li>`の中に`<p>...</p>`が展開されることに注目。そのため次のように書くことができる。

    1.  This is a list item with two paragraphs. Lorem ipsum dolor
        sit amet, consectetuer adipiscing elit. Aliquam hendrerit
        mi posuere lectus.

        Vestibulum enim wisi, viverra nec, fringilla in, laoreet
        vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
        sit amet velit.

    2.  Suspendisse id sem consectetuer libero luctus adipiscing.

> 1.  This is a list item with two paragraphs. Lorem ipsum dolor
>     sit amet, consectetuer adipiscing elit. Aliquam hendrerit
>     mi posuere lectus.
> 
>     Vestibulum enim wisi, viverra nec, fringilla in, laoreet
>     vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
>     sit amet velit.
> 
> 2.  Suspendisse id sem consectetuer libero luctus adipiscing.

中に引用を入れる場合は手前に4つスペース(またはタブ1つ)が必要になる。

    *   A list item with a blockquote:

        > This is a blockquote
        > inside a list item.

> *   A list item with a blockquote:
> 
>     > This is a blockquote
>     > inside a list item.

引用の内部にコードを挿入するにはスペース8つ(またはタブ2つ)が必要。

    *   A list item with a code block:

            <code goes here>

> *   A list item with a code block:
> 
>         <code goes here>

最後は次のケースに注意。普通に書いたつもりの文章が偶然リストと同じ形式になってしまうことがある。

    1986. What a great season.

> 1986. What a great season.

こういう場合はバックスラッシュを使う。

    1986\. What a great season.

> 1986\. What a great season.

### Code Blocks

コードブロックはスペース4つ(またはタブ1つ)を先頭に付ける。

    This is a normal paragraph:

        This is a code block.

> This is a normal paragraph:
> 
>     This is a code block.

変換結果は次の通り。先頭の空白(スペース4つまたはタブ一つ)は除去する。また`<code>`はインライン要素なのでブロック要素の`<pre>`を併用して出力する。

    <p>This is a normal paragraph:</p>

    <pre><code>This is a code block.
    </code></pre>

コードブロック内では`<` `>` `&`を自動的に`&lt;` `&gt;` `&amp;`に変換する。これは特にHTMLソースを挿入する場合に便利で、ほとんどの場合はそのままペーストしてインデントだけ追加すれば認識する。

        <div class="footer">
            &copy; 2004 Foo Corporation
        </div>

次のように変換される。

>     <pre><code>&lt;div class="footer"&gt;
>         &amp;copy; 2004 Foo Corporation
>     &lt;/div&gt;
>     </code></pre>

### Horizontal Rules

3つ以上の`-`,`*`,`_`だけで構成される行は`<hr/>`と認識される。間に半角スペースがあってもよい。

    * * *

    *****

    - - -

    -----------------------

    _ _ _

> * * *
> 
> *****
> 
> - - -
> 
> -----------------------
> 
> _ _ _

## Span Elements

### Links

リンクの`<a>`は`[内部の文字](URL)`で表現する。マウスをリンク上においた時に表示されるポップアップ文字列を設定する場合はURLの後に"..."を書く。

    This is [an example](http://example.com/ "Title") inline link.

> This is [an example](http://example.com/ "Title") inline link.

次のように変換される。

>     <p>This is <a href="http://example.com/" title="Title">
>     an example</a> inline link.</p>

これは単純な変換なので相対リンク(同一サーバ上)でもよい。

    See my [About](/about/) page for details.

>     <p>See my <a href="/about/">About</a> page for details.

MarkdownにはURLを間接的に表現する便利な機能がある。外部参照を用いたリンク(本文の表現ではreference-style link)は`[文字列][リンクID]`と書く。間に半角スペースがひとつあってもよい。

    This is [an example][id] reference-style link.

    This is [an example] [id] reference-style link.

[ID]: #

> This is [an example][id] reference-style link.
> 
> This is [an example] [id] reference-style link.

IDとそのURLを定義する側は文章のどこかに次の書式で記述する。URLは見やすくするため<...>で囲ってもよい。

    [ID]:(一つ以上のスペースまたはタブ)URL "もし必要ならタイトル"

例を示す。これらは変換したWebページには表示されない。

    [id]: http://example.com/  "Optional Title Here"
    [id]: <http://example.com/>  "Optional Title Here"

IDは数字、スペース、記号が入っていてもよい。また大文字と小文字は区別しない。次の場合はどちらも同じIDのAを参照する。

    [link text][a]
    [link text][A]

さらに省略機能がある。IDを(特殊文字を除く)任意の文字列としたのには理由があり、リンク対象文字列をそのままIDとして使うことができる。例えばGoogleという単語全部にリンクを付けるには次のように記述する。

    [Google][]

そして文書のどこかで次のようにGoogleをIDとして定義する。

    [Google]: http://google.com/

これで`[Google][]`は`<a href="http://google.com/">Google</a>`に変換される。

IDは空白を含んでよいため、次のようなこともできる。

    Visit [Daring Fireball][] for more information.

> Visit [Daring Fireball][] for more information.

このリンクの定義部は次のように記述すればよい。

    [Daring Fireball]: http://daringfireball.net/

[Daring Fireball]: http://daringfireball.net/

リンク定義は文書のどこにあってもよい。以下は最後にまとめて書いた場合の例。

    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"

> I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

[1]: http://google.com/        "Google"
[2]: http://search.yahoo.com/  "Yahoo Search"
[3]: http://search.msn.com/    "MSN Search"

名称そのものをIDにできるため次のように書いてもよい。以下の例では大文字と小文字を区別してないことに注意。

    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].

    [google]: http://google.com/        "Google"
    [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
    [msn]:    http://search.msn.com/    "MSN Search"

> I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

[google]: http://google.com/        "Google"
[yahoo]:  http://search.yahoo.com/  "Yahoo Search"
[msn]:    http://search.msn.com/    "MSN Search"

どちらも次のように変換される。

>     <p>I get 10 times more traffic from <a href="http://google.com/"
>     title="Google">Google</a> than from
>     <a href="http://search.yahoo.com/" title="Yahoo Search">Yahoo</a>
>     or <a href="http://search.msn.com/" title="MSN Search">MSN</a>.</p>

この参照リンクの機能は書きやすさよりもむしろ見やすさを考慮したもので、次のように直接書いた場合と比較すれば効果がよく分かる。

    I get 10 times more traffic from [Google](http://google.com/ "Google")
    than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
    [MSN](http://search.msn.com/ "MSN Search").

### Emphasis

`*文章*`や`_文章_`は`<em>文章</em>`に、`**文章**`や`__文章__`は`<strong>文章</strong>`に変換される(多くのブラウザで`<em>`は斜体、`<strong>`は太文字で表示する)。

    *single asterisks* -> <em>single asterisks</em>
    _single underscores_ -> <em>single underscores</em>
    **double asterisks** -> <strong>double asterisks</strong>
    __double underscores__ -> <strong>double underscores</strong>
    un*fucking*believable -> un<em>fucking</em>believable

> *single asterisks* -> <em>single asterisks</em>
> 
> _single underscores_ -> <em>single underscores</em>
> 
> **double asterisks** -> <strong>double asterisks</strong>
> 
> __double underscores__ -> <strong>double underscores</strong>
> 
> un*fucking*believable -> un<em>fucking</em>believable

前後に半角スペースがある場合はこれに該当しない(そうでないと色々と不便)。またアスタリスクやアンダーライン文字をそのまま記述する場合はバックスラッシュでエスケープする。

次の場合は前後のスペースがあるのでそのままでOK。

    y = 2 * x + 1

> y = 2 * x + 1

次の例は前後のスペースがないのでバックスラッシュでエスケープする。

    You're f\*\*king crazy.

> You're f\*\*king crazy.

ソースコードを(ブロックでなく)インラインで引用する場合は`` `...` ``で表現する。

    Use the `printf()` function.

> Use the `printf()` function.

>     <p>Use the <code>printf()</code> function.</p>

インラインコードの中に`` ` ``が含まれる場合は、2重バッククォート(``` `` ```)で囲む。

    ``files = `ls`.split``

> ``files = `ls`.split``
>
>     <p><code>files = `ls`.split</code></p>

最初または最後が`の場合は間にスペースをひとつ入れる(ないと認識しない)。この空白は除去される。

    `` `ps` ``

> `` `ps` ``

>     <p><code>`ps`</code></p>

さらにインラインコードの中にn重のバッククォートがある場合は(n+1)重のバッククォートで囲む。

    ``` ``double-back-quote`` ```

> ``` ``double-back-quote`` ```
> 
>     <p><code>``doubled back-quote``</code></p>

コードブロックの場合と同じようにインラインコードでも`<` `>` `&`は自動的に文字実体に変換される。

    Please don't use any `<blink>` tags.

> Please don't use any `<blink>` tags.

>     <p>Please don't use any <code>&lt;blink&gt;</code> tags.</p>

### Images

画像の`<img>`も表現できるが、`<a>`と同じように直接と参照の両方の方法がある。直接の場合の書式は次の通り。先頭の`!`で画像の`<img>`と認識する。

    ![代替文字列](URL)
    ![代替文字列](URL "タイトル")

実際に使った例を示す。

    ![Github mascot Octcat](https://raw.github.com/github/media/master/octocats/octocat.png "Octcat")

> ![Github mascot Octcat](https://raw.github.com/github/media/master/octocats/octocat.png "Octcat")

参照の場合は次の通り。先頭の`!`以外はリンクと同じなので書式だけ示す。

    ![Alt text][id]

定義側はリンクのIDと同じ書式を用いる。

    [id]: url/to/image  "Optional title attribute"

ただし大きさなどの設定はできないので、必要な場合は`<img>`要素を使うこと。

    <img src="https://raw.github.com/github/media/master/octocats/octocat.png"
     alt="Github mascot Octcat" title="Octcat" width="200" height="200" />

> <img src="https://raw.github.com/github/media/master/octocats/octocat.png"
   alt="Github mascot Octcat" title="Octcat" width="200" height="200" />

## Miscellaneous

### Automatic Links

URLの自動リンクはURLを`<...>`で囲む。

    <http://example.com/>

> <http://example.com/>

>     <a href="http://example.com/">http://example.com/</a>

メールアドレスでも同じ書式を使う。

    <address@example.com>

この場合はスパム対策のため全文字を文字実体参照に変換する(簡単な対策だが多くのロボットに対してはこれで防げることが多い)。

> <a href="&#x6D;&#x61;i&#x6C;&#x74;&#x6F;:&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;">&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;</a>

>     <a href="&#x6D;&#x61;i&#x6C;&#x74;&#x6F;:&#x61;&#x64;&#x64;&#x72;&#x65;
>     &#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;
>     &#109;">&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;
>     &#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;</a>

### Backslash Escapes

Markdownで使う特殊文字をまとめて示す。

    \   backslash
    `   backtick
    *   asterisk
    _   underscore
    {}  curly braces
    []  square brackets
    ()  parentheses
    #   hash mark
    +   plus sign
    -   minus sign (hyphen)
    .   dot
    !   exclamation mark

これらをそのままテキストとして書く場合はバックスラッシュでエスケープする。

    \*literal asterisks\*

> \*literal asterisks\*

## GitHub Flavored Markdown

Githubでは標準Markdown(Standard Markdown:以下SM)を拡張したGitHub Flavored Markdown(以下GFM)を用いている。正式な仕様書はなく、次が事実上のリファレンスになっている。

<https://help.github.com/articles/github-flavored-markdown>

### Multiple underscores in words

SMは英数字とアンダースコアの連続があるとそれを強調表示と認識しようとする。しかしこの仕様はsnake\_caseを多用する言語(特にRuby)と相性が悪く、例えば`each_with_index`の中央部(with)が強調と(誤?)認識される。

GFMは(連続した)語に複数回出現するアンダースコアを強調と認識しない(無視する)。以下はどちらもRubyでよく出てくる例。

    items.each_with_index |item, index|

    module_function :parse_text

> items.each_with_index |item, index|
> 
> module_function :parse_text

### URL autolinking

GFMはURLを自動認識するため、自動リンクは`<...>`で囲まなくてもよい。

    https://help.github.com/articles/github-flavored-markdown

> https://help.github.com/articles/github-flavored-markdown

### Strikethrough

GFMは取り消し線を`~~...~~`で表現できる。

    ~~これは誤り~~

> ~~これは誤り~~

### Fenced code blocks

SMはコードブロックをスペース4つのインデントで判別するが、GFMは\`\`\`で囲んだブロックもコードブロックと認識する(インデント不要)。

    ```
    npm install -g coffee
    ```

> ```
> npm install -g coffee
> ```

### Syntax highlighting

さらに\`\`\` (言語名) の形式で言語別の文法を認識しハイライト表示する。いくつか自分の知っている言語を使って確認する。

    ``` ruby
    require 'open-uri'
    open 'https://github.com/higuma' {|f| puts f.read }
    ```

> ``` ruby
> require 'open-uri'
> open 'https://github.com/higuma' {|f| puts f.read }
> ```

    ``` python
    if __name__ == '__main__':
        for noun in ('sax', 'ash', 'cry', 'day'): print(plural(noun))
    ```

> ``` python
> if __name__ == '__main__':
>     for noun in ('sax', 'ash', 'cry', 'day'): print(plural(noun))
> ```

    ``` javascript
    var dc = document.getElementById('canvas').getContext('2d');
    ```

> ``` javascript
> var dc = document.getElementById('canvas').getContext('2d');
> ```

(注意)言語名は全て小文字で記述すること(`JavaScript`では認識しない)。


### Tables

> Tableは最近追加された(2014年4月確認)。この文法はすでに多くのサイトで採用されている(Qiitaは2年前から採用)。なおオリジナルはPHP Markdown Extraらしい。

> <http://michelf.ca/projects/php-markdown/extra/>

`-`と`|`を使った簡易`<table>`作成機能がある。

```
 TH  |  TH
---- | ----
 TD  |  TD
 TD  |  TD
```

>  TH  |  TH
> ---- | ----
>  TD  |  TD
>  TD  |  TD

両端に`|`があってもよい(見やすさのため)。次の例は最初と同じ結果になる。

```
|  TH  |  TH  |
| ---- | ---- |
|  TD  |  TD  |
|  TD  |  TD  |
```

> |  TH  |  TH  |
> | ---- | ---- |
> |  TD  |  TD  |
> |  TD  |  TD  |

`-`の個数は横幅を合わせなくてもよい。

```
 TH | TH
-------|---------
TD | TD
 TD   | TD
```

>  TH | TH
> -------|---------
> TD | TD
> TD   | TD

アラインメントは次のように記述できる。

```
 left | center | right
:---- |:------:| -----:
  L   |   C    |   R
```

>  left | center | right
> :---- |:------:| -----:
>   L   |   C    |   R

## Qiita拡張

Qiita (<http://qiita.com/>)ではGitHub Flavored Markdownをベースとしてさらに拡張した方言が用いられる。文法は次を参照。 

<http://qiita.com/Qiita/items/c686397e4a0f4f11683d>

### コード

GFMと同じトリプルバッククォートを使える。さらに先頭部に`language:filename`の形式で言語とファイル名の両方を記述できる。これで文法ハイライトが有効になり、さらにコードブロックの左上にファイル名を表示する。

    ``` coffeescript:test.coffee
    for x in items
      console.log x
    ```

> ``` coffeescript:test.coffee
> for x in items
>   console.log x
> ```

### 数式(TeX形式)

トリプルバッククォート形式コードブロックで言語をmathとするとTeX記法による数式を記述できる。

    ``` math
    F(s)=\int_{0}^{\infty}f(t)e^{-st}dt
    ```

> ``` math
> F(s)=\int_{0}^{\infty}f(t)e^{-st}dt

> ```

インラインコードブロックと同じように`$formula$`と書くとインラインで数式を挿入できる。

    Qiitaではインライン数式$E=mc^2$が使えます。

> Qiitaではインライン数式$E=mc^2$が使えます。

ただしこの`$...$`の書式は時々副作用を起こす。次の文章はたまたま同じ行の中に$で囲まれた部分があるため数式と誤認識される。

    $.get(url)は$.ajax({url: url})と同じ

> $.get(url)は$.ajax({url: url})と同じ

このケースを考慮して、Qiita拡張では`$`文字もバックスラッシュによるエスケープの対象となる。次の例はQiitaでは`\$`をエスケープと認識するが、別の処理系ではバックスラッシュがそのまま表示される。

    \$.get(url)は\$.ajax({url: url})と同じ

> \$.get(url)は\$.ajax({url: url})と同じ




