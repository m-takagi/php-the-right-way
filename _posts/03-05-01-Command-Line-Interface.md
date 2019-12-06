---
title: コマンドラインインターフェイス
isChild: true
anchor:  command_line_interface
---

## コマンドラインインターフェイス {#command_line_interface_title}

PHP はもともとウェブアプリケーションを書くために作られたものだが、
コマンドラインインターフェイス (CLI) のプログラムを書くのにも便利だ。
コマンドラインのプログラムを書けば、テストやデプロイといった
よくある作業を自動化する助けとなる。

CLI の PHP プログラムが便利なのは、アプリケーションのコードを使うときに
わざわざウェブの UI を用意せずに済むところだ。
ただ、CLI の PHP スクリプトをウェブサーバーの公開ディレクトリに置くことは **絶対禁止** ！

PHP をコマンドラインで実行してみよう。

{% highlight console %}
> php -i
{% endhighlight %}

`-i` は、PHP の設定情報を [`phpinfo`][phpinfo] 関数みたいに表示するオプションだ。

`-a` オプションで対話シェルを使えるようになる。ruby の IRB とか、Python の対話シェルと同じようなものだ。
それ以外にも、いろんな [コマンドラインオプション][cli-options] がある。

じゃあ、シンプルな "Hello, $name" プログラムを書いてみよう。`hello.php` というファイルを作って、
こんな内容にする。

{% highlight php %}
<?php
if ($argc !== 2) {
    echo "Usage: php hello.php <name>.\n";
    exit(1);
}
$name = $argv[1];
echo "Hello, $name\n";
{% endhighlight %}

PHP のスクリプトを実行すると、コマンドラインの引数に関する変数がふたつ設定される。
[`$argc`][argc] は整数値で、引数の *数* を表し、
[`$argv`][argv] は配列で、各引数の *値* を含む。
最初の引数は、常に PHP スクリプトのファイル名となる。今回の場合なら `hello.php` だ。

`exit()` でゼロ以外の数値を返すと、コマンドが失敗したことをシェルに伝えることができる。
よく使われる終了コードは [ここ][exit-codes] で調べよう。

このスクリプトをコマンドラインから実行すると、次のようになる。

{% highlight console %}
> php hello.php
Usage: php hello.php <name>
> php hello.php world
Hello, world
{% endhighlight %}


 * [PHP をコマンドラインから実行する方法][php-cli]

[phpinfo]: https://secure.php.net/function.phpinfo
[cli-options]: https://secure.php.net/features.commandline.options
[argc]: https://secure.php.net/reserved.variables.argc
[argv]: https://secure.php.net/reserved.variables.argv
[exit-codes]: https://www.gsp.com/cgi-bin/man.cgi?section=3&amp;topic=sysexits
[php-cli]: https://secure.php.net/features.commandline.options
