---
title: Mac の人は
isChild: true
anchor:  mac_setup
---

## Mac の人は  {#mac_setup_title}

OS X には PHP が最初からインストールされているけど、最新の安定版からは微妙に遅れている。
Mountain Lion についてくるのは PHP 5.3.10 だし Mavericks でも 5.4.17。Yosemite にしても 5.5.9 だ。
PHP 5.6 に比べると、十分だとはいえない。

PHP を OS X にインストールするには、いくつかの方法がある。

### Homebrew によるインストール

[Homebrew] は OS X 用の強力なパッケージ管理ツールで、
PHP やその拡張モジュールも簡単にインストールできる。
[Homebrew PHP] が、Homebrew 用の PHP 関連の "Formula" をまとめたリポジトリだ。
これを使えば PHP をインストールできる。

現時点では、`php53`、`php54`、`php55`、`php56` が `brew install` コマンドでインストールできる。
これらを切り替えるには、環境変数 `PATH` を設定すればいい。
[brew-php-switcher][brew-php-switcher] を使えば、そのへんを自動的にやってくれる。

### Macports によるインストール

[MacPorts] プロジェクトはオープンソースのコミュニティによる取り組みで、
OS X上のオープンソースソフトウェアのコンパイルやインストールそしてアップグレードを簡単にできるようにする仕組みだ。
コマンドラインのソフトからX11やAquaベースのソフトにまで対応している。

MacPorts はコンパイル済みのバイナリにも対応しているので、関連するライブラリなどを毎回ソースからコンパイルしなおす必要はない。
なので、まだ何もパッケージをインストールしていない状態でも、時間の心配をする必要はない。

現時点でインストールできるのは `php53`、`php54`、`php55`、`php56` のいずれかで、`port install` コマンドを使ってこのようにインストールする。

    sudo port install php54
    sudo port install php55

そして、`select` コマンドを使って、アクティブな PHP を切り替える。

    sudo port select --set php php55

### phpbrew によるインストール

[phpbrew] は、複数のバージョンの PHP をインストールして管理するためのツールだ。
使っているアプリケーションやプロジェクトによって、PHP のバージョンが異なる場合に特に便利で、
仮想マシンを用意する必要がなくなる。

### Liipのバイナリインストーラーによる PHP のインストール
[php-osx.liip.ch] は、たった一行で PHP をインストールできる方法だ。バージョン 5.3 から 5.6 に対応している。
Apple がインストールした php のバイナリは上書きせず、まったく別の場所 (/usr/local/php5) にすべてをインストールする。

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
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: http://php-osx.liip.ch/
[mac-compile]: http://php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[mamp-downloads]: http://www.mamp.info/en/downloads/
[xampp]: http://www.apachefriends.org/jp/xampp.html
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
