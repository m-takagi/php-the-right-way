---
title: Mac の人は
isChild: true
anchor: mac_setup
---

## Mac の人は  {#mac_setup_title}

OS X には PHP が最初からインストールされているけど、最新の安定版からは微妙に遅れている。
Mountain Lion についてくるのは PHP 5.3.10 だし Mavericks でも 5.4.17。Yosemite にしても 5.5.9 だ。
PHP 5.6 に比べると、十分だとはいえない。

PHP を OS X にインストールするには、いくつかの方法がある。

### Homebrew によるインストール

[Homebrew](http://brew.sh/) は OS X 用の強力なパッケージ管理ツールで、
PHP やその拡張モジュールも簡単にインストールできる。
[Homebrew PHP] が、Homebrew 用の PHP 関連の "Formula" をまとめたリポジトリだ。
これを使えば PHP をインストールできる。

現時点では、`php53`、`php54`、`php55`、`php56` が `brew install` コマンドでインストールできる。
これらを切り替えるには、環境変数 `PATH` を設定すればいい。

### phpbrew によるインストール

[phpbrew] は、複数のバージョンの PHP をインストールして管理するためのツールだ。
使っているアプリケーションやプロジェクトによって、PHP のバージョンが異なる場合に特に便利で、
仮想マシンを用意する必要がなくなる。

### ソースからのコンパイル

もうひとつの選択肢がある。 [自分でコンパイル][mac-compile]
することで、この方法なら PHP の設定を完全にコントロールできる。
この場合は、Xcode あるいはその代用ツール ["Command Line Tools for XCode"]
をインストールする必要がある。これらは、 Apple の Mac Developer Center からダウンロードできる。

### 全部入りのインストーラー

ここまでの方法は主に PHP 本体だけを扱うもので、たとえば Apache や Nginx、そしてデータベースサーバーなどは用意していない。
いわゆる「全部入り」のソリューションである [MAMP][mamp-downloads] や [XAMPP][xampp] を使えば、
これらのソフトウェアもまとめてインストールしてくれる。
でも、簡単にインストールできるぶん、柔軟性に欠けるという弱点もある。

[Homebrew]: http://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[mac-compile]: http://www.php.net/manual/en/install.macosx.compile.php
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer

["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[mamp-downloads]: http://www.mamp.info/en/downloads/
[phpbrew]: https://github.com/phpbrew/phpbrew
[xampp]: http://www.apachefriends.org/jp/xampp.html
