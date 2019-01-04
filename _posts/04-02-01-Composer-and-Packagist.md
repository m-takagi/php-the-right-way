---
title: Composer と Packagist
isChild: true
anchor:  composer_and_packagist
---

## Composer と Packagist {#composer_and_packagist_title}

Composerは、PHP用としておすすめの依存管理ツールだ。プロジェクト内の依存関係を
`composer.json` ファイルに書いてシンプルなコマンドを打ち込めば、
Composer が自動的にそれをダウンロードしてくれるだけでなく、オートロードの設定までしてくれるんだ。
Composer は、node.js の NPM や Ruby の Bundler みたいなものだ。

Composer に対応したライブラリは既にいろいろ出回っていて、自分のプロジェクトですぐに使える。
そんなパッケージをまとめたのが [Packagist]。これは、Composer 対応の PHP ライブラリをまとめた公式リポジトリである。

### Composer のインストール

composer をダウンロードするいちばん安全な方法は、[公式サイトの指示に従うこと](https://getcomposer.org/download/)。
この方法だと、インストーラが壊れていたり改ざんされていたりしないかを確かめられる。
インストーラは、`composer.phar` のバイナリを _カレントディレクトリ_ にインストールする。

お勧めは、*グローバルにインストールする* (要するに、`/usr/local/bin` にだけ置く) 方式だ。
そのためには、次のコマンドを実行すればいい。

{% highlight console %}
mv composer.phar /usr/local/bin/composer
{% endhighlight %}

**注意:** パーミッションのエラーでこのコマンドが失敗する場合は、頭に `sudo` をつけて実行してみよう。

ローカルにインストールした Composer を実行するときには `php composer.phar`
とする。グローバルにインストールしたのなら、単に `composer` と打つだけだ。

#### Windows でのインストール

Windowsの場合、一番簡単なのは [ComposerSetup][6] インストーラーを使う方法だ。
これは、すべてのユーザーで使えるようにインストールしたうえで `$PATH` も設定してくれるので、
あとはコマンドラインから `composer` を呼ぶだけで使えるようになる。

### 依存関係の定義とインストール

Composer は、プロジェクトの依存関係を `composer.json` というファイルで管理する。
このファイルを手で書き換えてもいいし、Composer を使って編集してもいい。
`composer require` を実行すると、プロジェクトの依存関係を追加する。
もしまだ `composer.json` がなければ、新しいファイルを作る。
この例は、プロジェクトの依存関係に [Twig] を追加するものだ。

{% highlight console %}
composer require twig/twig:^2.0
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
これは、最初に `composer install`
を実行したときにダウンロードした、各パッケージの正確なバージョンを記録しておくものだ。
他の開発者とプロジェクトを共有するときに `composer.lock` も一緒に配布しておくと、
他の人が `composer install` を実行したときにもまったく同じバージョンがインストールされるようになる。
依存関係を更新するには、 `composer update` を実行しよう。
デプロイのときには `composer update` を使ってはいけない。必ず `composer install` を使うこと。
そうしないと、開発環境と運用環境で違うバージョンのパッケージを使ってしまうことになる。

これは、バージョンの要件を柔軟に定義できるので便利だ。
たとえば、バージョンに `~1.8` と書いた場合は「`1.8.0` 以降のバージョン。ただし `2.0.x-dev` は含まない」と指定したことになる。
ワイルドカード `*` を使って `1.8.*` のように指定してもいい。
これで、Composer で `php composer.phar update` を実行したときに、
定義した制約の範囲での最新版に依存ライブラリを更新してくれる。

### 更新通知

新バージョンのリリースの通知を受け取りたければ [libraries.io] にサインアップするといい。
このサービスは、依存ライブラリを監視して、更新があれば通知してくれるものだ。

### 依存ライブラリのセキュリティ問題のチェック

[Security Advisories Checker] は、Webサービスとコマンドラインツールとして提供されている。
`composer.lock` ファイルを調べて、もし依存関係に更新が必要なら教えてくれるものだ。

### Composerでのグローバルな依存関係の扱い

Composer は、グローバルな依存関係やそのバイナリを扱うこともできる。
使いかたはとても簡単で、単にコマンドの前に `global` をつけるだけでいい。
たとえば、PHPUnit をグローバルに使えるようインストールしたければ、こんなコマンドを実行する。

{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}

このコマンドは、 `~/.composer` ディレクトリを作って、グローバルな依存関係をそこに置く。
インストールされたパッケージのバイナリを全体で使えるようにするには、 `~/.composer/vendor/bin`
ディレクトリを環境変数 `$PATH` に追加すればいい。

* [Composerとは]

[Packagist]: https://packagist.org/
[Twig]: https://twig.symfony.com/
[libraries.io]: https://libraries.io/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Composerとは]: https://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe
