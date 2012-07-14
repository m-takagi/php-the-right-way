---
isChild: true
---

## コマンドラインインターフェイス

PHP はもともとウェブアプリケーションを書くために作られたものだが、
コマンドラインインターフェイス (CLI) のプログラムを書くのにも便利だ。
コマンドラインのプログラムを書けば、テストやデプロイといった
よくある作業を自動化する助けとなる。

CLI の PHP プログラムが便利なのは、アプリケーションのコードを使うときに
わざわざウェブの UI を用意せずに済むところだ。
ただ、CLI の PHP スクリプトをウェブサーバーの公開ディレクトリに置くことは絶対禁止！

PHP をコマンドラインで実行してみよう。

{% highlight bash %}
> php -i
{% endhighlight %}

`-i` は、PHP の設定情報を [`phpinfo`][phpinfo] 関数みたいに表示するオプションだ。

`-a` オプションで対話シェルを使えるようになる。ruby の IRB とか、Python の対話シェルと同じようなものだ。
それ以外にも、いろんな [コマンドラインオプション][cli-options] がある。

じゃあ、シンプルな "Hello, $name" プログラムを書いてみよう。`hello.php` というファイルを作って、
こんな内容にする。

{% highlight php %}
<?php
if($argc != 2) {
    die("Usage: php hello.php [name].\n");
}
$name = $argv[1];
echo "Hello, $name\n";
{% endhighlight %}

PHP のスクリプトを実行すると、コマンドラインの引数に関する変数がふたつ設定される。
[`$argc`][argc] は整数値で、引数の *数* を表し、
[`$argv`][argv] は配列で、各引数の *値* を含む。
最初の引数は、常に PHP スクリプトのファイル名となる。今回の場合なら `hello.php` だ。

このスクリプトをコマンドラインから実行すると、次のようになる。

{% highlight bash %}
> php hello.php
Usage: php hello.php [name]
> php hello.php world
Hello, world
{% endhighlight %}


 * [PHP をコマンドラインから実行する方法][php-cli]
 * [Windows でもコマンドラインから PHP を実行したい！][php-cli-windows]

[phpinfo]: http://php.net/manual/ja/function.phpinfo.php
[cli-options]: http://www.php.net/manual/ja/features.commandline.options.php
[argc]: http://php.net/manual/ja/reserved.variables.argc.php
[argv]: http://php.net/manual/ja/reserved.variables.argv.php
[php-cli]: http://php.net/manual/ja/features.commandline.php
[php-cli-windows]: http://www.php.net/manual/ja/install.windows.commandline.php
