---
title: Windows の人は
isChild: true
anchor:  windows_setup
---

## Windows の人は {#windows_setup_title}

[windows.php.net/download][php-downloads] からバイナリをダウンロードしよう。それを展開したら、
PHPフォルダのルート (php.exe がある場所) に [PATH][windows-path] を通しておくといい。そうすれば、どこからでも PHP を実行できるようになる。

学習用にローカルで開発する場合は PHP 5.4 以降のビルトインウェブサーバーを使えばよいので、細かい設定を気にする必要はない。
もしウェブサーバーとかMySQLとかも含めた「全部入り」を使いたければ、[Web Platform Installer][wpi]や
[XAMPP][xampp]、[EasyPHP][easyphp]、そして[WAMP][wamp]などがお勧めだ。これらを使えば Windows 用の開発環境を手早く構築できる。
とはいうものの、これらのツールは実際の運用環境と微妙に異なる。なので、たとえば「Windows で開発して Linux にデプロイ」
とかいう場合は環境の違いに気をつける必要がある。

Windows 上でシステムを実運用する場合は、IIS 7 を使うとよい。これが一番安定しており、かつパフォーマンスも優れている。
[phpmanager][phpmanager] (IIS 7 用の GUI プラグイン) を使えば PHP の設定は管理をシンプルにできる。
IIS 7 には FastCGI が組み込まれており、すぐに使える。
単に PHP をハンドラとして設定するだけでよい。
その値の詳しい情報は、[dedicated area on iis.net][php-iis]に PHP 専用のエリアがある。

[php-downloads]: http://windows.php.net/download/
[windows-path]: http://www.windows-commandline.com/set-path-command-line/
[wpi]: http://www.microsoft.com/web/downloads/platform.aspx
[xampp]: http://www.apachefriends.org/en/xampp.html
[easyphp]: http://www.easyphp.org/
[wamp]: http://www.wampserver.com/en/
[phpmanager]: http://phpmanager.codeplex.com/
[php-iis]: http://php.iis.net/
