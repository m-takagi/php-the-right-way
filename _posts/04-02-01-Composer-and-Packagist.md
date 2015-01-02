---
title: Composer と Packagist
isChild: true
anchor:  composer_and_packagist
---

## Composer と Packagist {#composer_and_packagist_title}

Composerは、PHP用の **すばらしい** 依存管理ツールだ。プロジェクト内の依存関係を
`composer.json` ファイルに書いてシンプルなコマンドを打ち込めば、
Composer が自動的にそれをダウンロードしてくれるだけでなく、オートロードの設定までしてくれるんだ。

Composer に対応したライブラリは既にいろいろ出回っていて、自分のプロジェクトですぐに使える。
そんなパッケージをまとめたのが [Packagist]。これは、Composer 対応の PHP ライブラリをまとめた公式リポジトリである。

### Composer のインストール

Composer はローカル (作業ディレクトリ) にインストールしてもよいし、グローバルに (/usr/local/bin などに)
インストールしてもよい。ただし、ローカルにインストールする方法は今は非推奨となっている。
ローカルにインストールするには、プロジェクトのルートディレクトリに移動して次のコマンドを実行する。

{% highlight console %}
curl -s https://getcomposer.org/installer | php
{% endhighlight %}

このコマンドは、 `composer.phar` (PHP バイナリアーカイブ)
をダウンロードする。これを `php` コマンドで実行すれば、そのプロジェクトの依存関係を管理できる。
<strong>注意:</strong> ダウンロードしたコードを直接パイプで実行する前に、
まずはオンラインでコードを確認して安全であることを確かめておこう。

#### Windows でのインストール

Windowsの場合、一番簡単なのは [ComposerSetup][6] インストーラーを使う方法だ。
これは、すべてのユーザーで使えるようにインストールしたうえで `$PATH` も設定してくれるので、
あとはコマンドラインから `composer` を呼ぶだけで使えるようになる。

### Composer の手動インストール

手動で Composer をインストールするのは初心者にはおすすめできない。
でもなぜか、さっきのインストール方法よりも手作業でのインストールをしたがる人もいるらしい。
さっきの対話的なインストールでは何をしていたのかというと、こんなことを確かめていたんだ。

- 適切なバージョンの PHP が入っている
- `.phar` ファイルを正しく実行できる
- ディレクトリのパーミッションが適切に設定されている
- 問題のある特定の拡張モジュールがロードされていない
- `php.ini` の設定が適切にされている

手作業でのインストールではこういったチェックは一切行わない。
それでもいいの？

それでもいいという人向けに、手動での Composer のインストール方法を示す。

{% highlight console %}
curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
chmod +x $HOME/local/bin/composer
{% endhighlight %}

`$HOME/local/bin` (あるいは、その他あなたが指定した場所) にパスを通しておく必要がある。
これで、`composer` コマンドが使えるようになるだろう。

何かのドキュメントに「`php composer.phar install` で Composer を実行します」と書いてあれば、
その部分を次のように読み替えればいい。

{% highlight console %}
composer install
{% endhighlight %}
    
このセクションでは、composer をグローバルにインストールしたものとして説明する。

### 依存関係の定義とインストール

Composer は、プロジェクトの依存関係を `composer.json` というファイルで管理する。
このファイルを手で書き換えてもいいし、Composer を使って編集してもいい。

`composer require` を実行すると、プロジェクトの依存関係を追加する。
もしまだ `composer.json` がなければ、新しいファイルを作る。
この例は、プロジェクトの依存関係に [Twig] を追加するものだ。

{% highlight console %}
composer require twig/twig:~1.8
{% endhighlight %}

あるいは、 `composer init` コマンドを実行して、
自分のプロジェクト用の完全な `composer.json` ファイルを作ることもできる。
どちらの方法にせよ、一度 `composer.json` ファイルを作ってしまえば、
あとは Composer がすべての依存ライブラリをダウンロードして `vendor/` にインストールしてくれる。
次のコマンドは、すでに `composer.json` ファイルを含むプロジェクトをダウンロードした場合にも使える。

{% highlight console %}
composer install
{% endhighlight %}

次に、アプリケーションで最初に呼ばれる PHP ファイルにこんな行を追加する。
これは、Composer のオートローダーを使ってプロジェクトの依存ライブラリを読むよう指示している。

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

これで、依存ライブラリが使えるようになった。実際に使う場面で、必要に応じて読み込まれる。

### 依存関係の更新

Composer は `composer.lock` というファイルを作る。
これは、最初に `php composer.phar install`
を実行したときにダウンロードした、各パッケージの正確なバージョンを記録しておくものだ。
他の開発者とプロジェクトを共有するときに `composer.lock` も一緒に配布しておくと、
他の人が `php composer.phar install` を実行したときにもまったく同じバージョンがインストールされるようになる。
依存関係を更新するには、 `php composer.phar update` を実行しよう。

これは、バージョンの要件を柔軟に定義できるので便利だ。
たとえば、バージョンに `~1.8` と書いた場合は「`1.8.0` 以降のバージョン。ただし `2.0.x-dev` は含まない」と指定したことになる。
ワイルドカード `*` を使って `1.8.*` のように指定してもいい。
これで、Composer で `php composer.phar update` を実行したときに、
定義した制約の範囲での最新版に依存ライブラリを更新してくれる。

### 更新通知

新バージョンのリリースの通知を受け取りたければ [VersionEye] にサインアップするといい。
このサービスは、自分の GitHub アカウントや BitBucket アカウントにある
`composer.json` の内容を監視して、パッケージの新しいリリースがあればメールで教えてくれるものだ。

### 依存ライブラリのセキュリティ問題のチェック

[Security Advisories Checker] は、Webサービスとコマンドラインツールとして提供されている。
`composer.lock` ファイルを調べて、もし依存関係に更新が必要なら教えてくれるものだ。

### Handling global dependencies with Composer

Composer can also handle global dependencies and their binaries. Usage is straight-forward, all you need
to do is prefix your command with `global`. If per example you wanted to install PHPUnit and have it 
available globally, you'd run the following command:

{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}

This will create a `~/.composer` folder where your global dependencies reside. To have the installed
packages' binaries available everywhere, you'd then add the `~/.composer/vendor/bin` folder to your 
`$PATH` variable.

* [Composerとは]

[Packagist]: http://packagist.org/
[Twig]: http://twig.sensiolabs.org
[VersionEye]: https://www.versioneye.com/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Composerとは]: http://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe
