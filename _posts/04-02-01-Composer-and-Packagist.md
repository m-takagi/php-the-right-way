---
title: Composer と Packagist
isChild: true
---

## Composer と Packagist {#composer_and_packagist_title}

Composerは、PHP用の **すばらしい** 依存管理ツールだ。プロジェクト内の依存関係を
`composer.json` ファイルに書いてシンプルなコマンドを打ち込めば、
Composer が自動的にそれをダウンロードしてくれるだけでなく、オートロードの設定までしてくれるんだ。

Composer に対応したライブラリは既にいろいろ出回っていて、自分のプロジェクトですぐに使える。
そんなパッケージをまとめたのが [Packagist][1]。これは、Composer 対応の PHP ライブラリをまとめた公式リポジトリである。

### Composer のインストール

Composer はローカル (作業ディレクトリ) にインストールしてもよいし、グローバルに (/usr/local/bin などに)
インストールしてもよい。ただし、ローカルにインストールする方法は今は非推奨となっている。
ローカルにインストールするには、プロジェクトのルートディレクトリに移動して次のコマンドを実行する。

    curl -s https://getcomposer.org/installer | php

このコマンドは、 `composer.phar` (PHP バイナリアーカイブ)
をダウンロードする。これを `php` コマンドで実行すれば、そのプロジェクトの依存関係を管理できる。
<strong>注意:</strong> ダウンロードしたコードを直接パイプで実行する前に、
まずはオンラインでコードを確認して安全であることを確かめておこう。

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

    curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
    chmod +x $HOME/local/bin/composer

`$HOME/local/bin` (あるいは、その他あなたが指定した場所) にパスを通しておく必要がある。
これで、`composer` コマンドが使えるようになるだろう。

何かのドキュメントに「`php composer.phar install` で Composer を実行します」と書いてあれば、
その部分を次のように読み替えればいい。

    composer install

### 依存関係の定義とインストール

Composer keeps track of your project's dependencies in a file called `composer.json`. You can manage it by hand if you like, or use Composer itself. The `php composer.phar require` command adds a project dependency and if you don't have a `composer.json` file, one will be created. Here's an example that adds [Twig][2] as a dependency of your project. Run it in your project's root directory where you've downloaded `composer.phar`:

	php composer.phar require twig/twig:~1.8

Alternatively the `php composer.phar init` command will guide you through creating a full `composer.json` file for your project. Either way, once you've created your `composer.json` file you can tell Composer to download and install your dependencies into the `vendors/` directory. This also applies to projects you've downloaded that already provide a `composer.json` file:

    php composer.phar install

Next, add this line to your application's primary PHP file; this will tell PHP to use Composer's autoloader for your project dependencies.

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

これで、依存ライブラリが使えるようになった。実際に使う場面で、必要に応じて読み込まれる。

### Updating your dependencies

Composer creates a file called `composer.lock` which stores the exact version of each package it downloaded when you first ran `php composer.phar install`. If you share your project with other coders and the `composer.lock` file is part of your distribution, when they run `php composer.phar install` they'll get the same versions as you. To update your dependencies, run `php composer.phar update`.

This is most useful when you define your version requirements flexibly. For instance a version requirement of ~1.8  means "anything newer than 1.8.0, but less than 2.0.x-dev". You can also use the `*` wildcard as in `1.8.*`. Now Composer's `php composer.phar update` command will upgrade all your dependencies to the newest version that fits the restrictions you define.

### Checking your dependencies for security issues

The [Security Advisories Checker][3] is a web service and a command-line tool, both will examine your `composer.lock` file and tell you if you need to update any of your dependencies.

* [Composerとは][4]

[1]: http://packagist.org/
[2]: http://twig.sensiolabs.org
[3]: http://getcomposer.org/doc/00-intro.md
[4]: https://security.sensiolabs.org/
