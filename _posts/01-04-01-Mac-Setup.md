---
title: Mac の人は
isChild: true
anchor:  mac_setup
---

## Mac の人は  {#mac_setup_title}

OS X には PHP が最初からインストールされているけど、最新の安定版からは微妙に遅れている。
最新の PHP を OS X にインストールするには、いくつかの方法がある。

### Homebrew によるインストール

[Homebrew] は OS X 用の強力なパッケージ管理ツールで、
PHP やその拡張モジュールも簡単にインストールできる。
Homebrew のコアリポジトリで、PHP 5.6、7.0、7.1、7.2、7.3、7.4 用の "Formula" が公開されている。

最新版の PHP をインストールするには、こんなコマンドを実行すればいい。

```
brew install php@7.4
```

Homebrew の PHP のバージョンを切り替えるには、環境変数 `PATH` を設定すればいい。
[brew-php-switcher][brew-php-switcher] を使えば、そのへんを自動的にやってくれる。

### Macports によるインストール

[MacPorts] プロジェクトはオープンソースのコミュニティによる取り組みで、
OS X上のオープンソースソフトウェアのコンパイルやインストールそしてアップグレードを簡単にできるようにする仕組みだ。
コマンドラインのソフトからX11やAquaベースのソフトにまで対応している。

MacPorts はコンパイル済みのバイナリにも対応しているので、関連するライブラリなどを毎回ソースからコンパイルしなおす必要はない。
なので、まだ何もパッケージをインストールしていない状態でも、時間の心配をする必要はない。

現時点でインストールできるのは `php54`、`php55`、`php56`、`php70`、`php71`、`php72`、`php73`、`php74` のいずれかで、`port install` コマンドを使ってこのようにインストールする。

    sudo port install php56
    sudo port install php74

そして、`select` コマンドを使って、アクティブな PHP を切り替える。

    sudo port select --set php php74

### phpbrew によるインストール

[phpbrew] は、複数のバージョンの PHP をインストールして管理するためのツールだ。
使っているアプリケーションやプロジェクトによって、PHP のバージョンが異なる場合に特に便利で、
仮想マシンを用意する必要がなくなる。

### Liipのバイナリインストーラーによる PHP のインストール

[php-osx.liip.ch] は、たった一行で PHP をインストールできる方法だ。バージョン 5.3 から 7.4 に対応している。
Apple がインストールした php のバイナリは上書きせず、まったく別の場所 (/usr/local/php5) にすべてをインストールする。

### ソースからのコンパイル

もうひとつの選択肢がある。 [自分でコンパイル][mac-compile]
することで、この方法なら PHP の設定を完全にコントロールできる。
この場合は、Xcode あるいはその代用ツール ["Command Line Tools for XCode"]
をインストールする必要がある。これらは、 Apple の Mac Developer Center からダウンロードできる。

### 全部入りのインストーラー

ここまでで説明した方法は PHP そのものに関するものばかりで、それ以外のたとえば [Apache][apache]、[Nginx][nginx]、SQL server などは扱っていない。
[MAMP][mamp-downloads] や [XAMPP][xampp] みたいな「全部入り」のインストーラーを使えば、このあたりのソフトも一緒に入れられるし設定もしてくれる。かんたん。
でも、インストーラーに縛られてしまって自由にいじれなくなるという弱点もある。

[Homebrew]: https://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: https://php-osx.liip.ch/
[mac-compile]: https://secure.php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[apache]: https://httpd.apache.org/
[nginx]: https://www.nginx.com/
[mamp-downloads]: https://www.mamp.info/en/downloads/
[xampp]: https://www.apachefriends.org/jp/index.html
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
