---
title: Mac の人は
isChild: true
anchor: mac_setup
---

## Mac の人は  {#mac_setup_title}

OS&nbsp;X には PHP が最初からインストールされているけど、最新の安定版からは微妙に遅れている。
Lion についてくるのは PHP 5.3.6 だし Mountain Lion でも 5.3.10。Mavericks にしても 5.4.17 だ。

OS&nbsp;X の PHP をアップデートするには、Mac 用の [パッケージ管理ツール][mac-package-managers]を使えばいい。
あと、[php-osx by Liip][php-osx-downloads]もおすすめだ。

あるいは、[自分でコンパイル][mac-compile]してもいい。ただその場合は、
Xcode あるいは ["Command Line Tools for Xcode"][apple-developer] をインストールしないといけない。
これは Apple の Mac Developer Center でダウンロードできる。

PHP だけじゃなくて Apache や MySQL とかも入っていて、さらに GUI
の管理ツールもついてる「全部入り」のパッケージもある。それが [MAMP][mamp-downloads] や [XAMPP][xampp] だ。

[mac-package-managers]: http://www.php.net/manual/en/install.macosx.packages.php
[mac-compile]: http://www.php.net/manual/en/install.macosx.compile.php
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
[apple-developer]: https://developer.apple.com/downloads
[mamp-downloads]: http://www.mamp.info/en/downloads/
[php-osx-downloads]: http://php-osx.liip.ch/
[xampp]: http://www.apachefriends.org/jp/xampp.html
