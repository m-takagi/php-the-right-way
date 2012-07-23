---
title: Composer と Packagist
isChild: true
---

## Composer と Packagist

Composerは、PHP用の **すばらしい** 依存管理ツールだ。プロジェクト内の依存関係を
`composer.json` ファイルに書いてシンプルなコマンドを打ち込めば、
Composer が自動的にそれをダウンロードしてくれるだけでなく、オートロードの設定までしてくれるんだ。

Composer に対応したライブラリは既にいろいろ出回っていて、自分のプロジェクトですぐに使える。
そんなパッケージをまとめたのが [Packagist][1]。これは、Composer 対応の PHP ライブラリをまとめた公式リポジトリである。

### Composer のインストール

Composer はローカル (作業ディレクトリ) にインストールしてもよいし、グローバルに (/usr/local/bin などに)
インストールしてもよい。ただし、ローカルにインストールする方法は今は非推奨となっている。
ローカルにインストールするには、プロジェクトのルートディレクトリに移動して次のコマンドを実行する。

    curl -s http://getcomposer.org/installer | php

このコマンドは、 `composer.phar` (PHP バイナリアーカイブ)
をダウンロードする。これを `php` コマンドで実行すれば、そのプロジェクトの依存関係を管理できる。
<strong>注意:</strong> ダウンロードしたコードを直接パイプで実行する前に、
まずはオンラインでコードを確認して安全であることを確かめておこう。

### Composer の手動インストール

手動で composer をインストールするのは初心者にはおすすめできない。
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

    curl -s http://getcomposer.org/composer.phar -o $HOME/local/bin/composer
    chmod +x $HOME/local/bin/composer

`$HOME/local/bin` (あるいは、その他あなたが指定した場所) にパスを通しておく必要がある。
これで、`composer` コマンドが使えるようになるだろう。

何かのドキュメントに「`php composer.phar install` で Composer を実行します」と書いてあれば、
その部分を次のように読み替えればいい。

    composer install

### 依存関係の定義とインストール

まず、`composer.phar` と同じディレクトリに `composer.json` というファイルを作ろう。
ここに示す例は、あるプロジェクトで [Twig][2] を使うという依存関係を示したものだ。

	{
	    "require": {
	        "twig/twig": "1.8.*"
	    }
	}

次に、プロジェクトのルートディレクトリでこのコマンドを実行する。

    php composer.phar install

このコマンドは、依存するプロジェクトをダウンロードして `vendors/` ディレクトリにインストールする。
次に、自分のアプリケーションで最初に実行する PHP ファイルにこんな行を追加する。
これは、「Composer のオートローダーを使って依存ライブラリを読み込む」
という指示である。

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

これで、依存ライブラリが使えるようになった。実際に使う場面で、必要に応じて読み込まれる。

* [Composerとは][3]

[1]: http://packagist.org/
[2]: http://twig.sensiolabs.org
[3]: http://getcomposer.org/doc/00-intro.md
