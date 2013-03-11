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

Composer は、プロジェクトの依存関係を `composer.json` というファイルで管理する。
このファイルを手で書き換えてもいいし、Composer を使って編集してもいい。
`php composer.phar require` を実行すると、プロジェクトの依存関係を追加する。
もしまだ `composer.json` がなければ、新しいファイルを作る。
この例は、プロジェクトの依存関係に [Twig][2] を追加するものだ。
プロジェクトのルートディレクトリに `composer.phar` をダウンロードして、このコマンドを実行しよう。

	php composer.phar require twig/twig:~1.8

あるいは、 `php composer.phar init` コマンドを実行して、
自分のプロジェクト用の完全な `composer.json` ファイルを作ることもできる。
どちらの方法にせよ、一度 `composer.json` ファイルを作ってしまえば、
あとは Composer がすべての依存ライブラリをダウンロードして `vendors/` にインストールしてくれる。
次のコマンドは、すでに `composer.json` ファイルを含むプロジェクトをダウンロードした場合にも使える。

    php composer.phar install

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
たとえば、バージョンに ~1.8 と書いた場合は「1.8.0 以降のバージョン。ただし 2.0.x-dev は含まない」と指定したことになる。
ワイルドカード `*` を使って `1.8.*` にように指定してもいい。
これで、Composer で `php composer.phar update` を実行したときに、
定義した制約の範囲での最新版に依存ライブラリを更新してくれる。

### 依存ライブラリのセキュリティ問題のチェック

[Security Advisories Checker][3] は、Webサービスとコマンドラインツールとして提供されている。
`composer.lock` ファイルを調べて、もし依存関係に更新が必要なら教えてくれるものだ。

* [Composerとは][4]

[1]: http://packagist.org/
[2]: http://twig.sensiolabs.org
[3]: http://getcomposer.org/doc/00-intro.md
[4]: https://security.sensiolabs.org/
