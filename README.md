# obarads.github.io
## About
3Dと点群、教師なし手法、その他についてまとめもしくは翻訳をします。論文と見比べて怪しい部分がある場合は該当する論文のIssueにコメントをしていただけると幸いです。Githubのスターを押していただけるとやる気が出ます。

PapersやComplementaryの内容は[https://obarads.github.io/](https://obarads.github.io/)で正しく表示されます。ブラウザはGoogle Chromeにしか対応しておらず、その他は表示が崩れる可能性が有ります。  
比較的最近(1年くらい)の論文に関する記事更新情報は[Twitterの@obarads](https://twitter.com/obarads)で公開しています。  
サイトの方には[Markdown_and_MathJax](https://github.com/Obarads/Markdown_and_MathJax)を使っています。

## Link & Directory
各ページ(ディレクトリ)の説明をする。なお、基本的に定義式は画像形式で示します。理由は、書くのに時間がかかるのと、スマホで見たときに画面外にはみ出すことがあるからです。

### [Papers](./papers)
各論文のmdファイルが入っている。基本的に[arXivTimesのIssue](https://github.com/arXivTimes/arXivTimes)と[落合フォーマット](https://www.slideshare.net/Ochyai/1-ftma15?ref=http://lafrenze.hatenablog.com/entry/2015/08/04/120205)に従う形式となっている。  

#### status
ページ内の進行具合を示すものである。以下4つに分かれる。
- 更新済: 完成したもの、今後更新されるとしたら間違いの修正もしくは詳細の追記が行われる。
- 省略: 内容を一部省略したもの、とりあえず完成したものであり省略した部分は更新される可能性がある。
- 参照: 他のページを参照するようにリンクを貼っているもの
- 未完: 更新中もしくは更新を放棄しているもの
- 修正: ちゃんと理解する必要して修正する必要がある。

### [Complementary](./complementary)
各論文で出てくる単語やジャンルへの補足説明を行う。

## Issue page in Github
Issueの方に各論文のmdファイルの内容を載せています。もし間違いだと思う部分がある場合は、とりあえずissueの方にcommentしてください。


## Use this repository in local
### About
本来、このレポジトリはgithub.ioでもネットワークが無い環境でも論文をまとめられる様にするために作られた。

通常のmarkdownの記述と違い、Latexの数式記述をMathJax.jsで反映する(プログラムコードは[1]を利用している)。サーバー側に必要なのはHTMLとjavascriptだけでOK(編集する環境のみファイルをリスト化するのにpythonが必要)。

### Things to prepare
- **visual studio code(vscode)**  
    Windows, Mac, Linuxのどれでも使えるエディッタ。今回は簡単にローカルサーバーで動かすための拡張機能、Live Serverを使うためにもインストールする。

- **anacondaなどのpython環境(動作保証はver.3.7)**  
    正確に言うと絶対必要ではない。/index.htmlでmdファイルへのリンクを一覧として表示する際に、リンク先をjavascriptの配列でまとめ、それをreturnする関数を持つファイルが必要になる(ただし現在では予めまとめられる情報はそのファイルにまとめるようにしている、/js/list.js参照)。手動で書いてもいいし、他のツールを使ってもいいし、そもそも/index.htmlを使わないのであればこれ自体必要ない。
    - なぜリスト用のjsファイルが必要なのか  
        javascriptとhtmlだけでは、フォルダ内の探索を行えないからである。これらの言語は、(たしか)セキュリティーの問題上、これらの権限を持つことができない。

- **MathJaxのライブラリ(動作保証はver.2.7.5)**  
    インダーネットに繋げれないローカルサーバーで動作させる際に必要、いつでもインターネットに繋げられるのであれば必要なし(cdnjsを経由して取得している)。もし必要であれば、MathJaxライブラリを/js内に置くこと。

### How to use
1. **リンクや情報諸々をまとめたjsファイルの作成**  
    mdを/papersへ入れた後に、listing.pyをpythonで動かす。すると、jsフォルダにlist.jsが作られる。
    - **文中にある情報をまとめる際のトリガーワード(listing.pyによる処理前に行うこと)**  
        - \#\# key-words  
        この表記の一行下にkey-wordを置くことで、/index.htmlにそのkey-wordを並べてくれる(書き方は/papersの中のmdファイルを参考に)。
        - \#\#\# 投稿日付(yyyy/MM/dd)  
        この表記の一行下にその内容の投稿日や公開年月日を書くと/index.htmlで表示してくれる(書き方は/papersの中のmdファイルを参考に)。

1. **Live serverによるローカルサーバー化**  
    vscodeにこのディレクトリを追加後、Live serverを起動し、このディレクトリを選択する。webブラウザが起動して/index.htmlが表示される。あとは好きにする。

### Reference
1. [markdown+mathjaxをブラウザで](https://qiita.com/legokichi/items/27b7b865a0ab28b5d530)
2. [ButTaiwan/gensen-font: A free font family derived from Source Han Sans.](https://github.com/ButTaiwan/gensen-font/tree/master/JP)
3. [資料－Web色見本](http://www.geocities.co.jp/HeartLand/8819/webjpcol.html)
4. [MathJax で利用可能な TeX コマンド（非公式）](http://memopad.bitter.jp/web/mathjax/TeXSyntax.html)
5. [CSSで作る！押したくなるボタンデザイン100（Web用）](https://saruwakakun.com/html-css/reference/buttons)
6. [inputイベントでフォーム入力値をリアルタイム取得できるよ。（あとjQuery例。） | Ginpen.com](https://ginpen.com/2018/01/30/realtime-form-values/)
7. [テーブルのキーワードにマッチする行だけ表示する at softelメモ](https://www.softel.co.jp/blogs/tech/archives/4330)
8. [jQuery tablesorter 2.0](https://mottie.github.io/tablesorter/docs/)
9. [MathJax | Beautiful math in all browsers.](https://www.mathjax.org/)