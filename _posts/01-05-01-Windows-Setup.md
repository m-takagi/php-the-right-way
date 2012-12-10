---
title: Windows の人は
isChild: true
---

## Windows の人は {#windows_setup_title}

Windows で PHP を使うには、いくつかの方法がある。[バイナリをダウンロード][php-downloads]することもできるし、
つい最近までは'.msi'形式のインストーラも使えた。でも PHP 5.3.0 からは、インストーラ形式をサポートしなくなった。

学習用にローカルで開発する場合は PHP 5.4 のビルトインウェブサーバーを使えばよいので、細かい設定を気にする必要はない。
もしウェブサーバーとかMySQLとかも含めた「全部入り」を使いたければ、[Web Platform Installer][wpi]や
[Zend Server CE][zsce]、[XAMPP][xampp]、そして[WAMP][wamp]などがお勧めだ。これらを使えば Windows 用の開発環境を手早く構築できる。
とはいうものの、これらのツールは実際の運用環境と微妙に異なる。なので、たとえば「Windows で開発して Linux にデプロイ」
とかいう場合は環境の違いに気をつける必要がある。

Windows 上でシステムを実運用する場合は、IIS 7 を使うとよい。これが一番安定しており、かつパフォーマンスも優れている。
[phpmanager][phpmanager] (IIS 7 用の GUI プラグイン) を使えば PHP の設定は管理をシンプルにできる。
IIS 7 には FastCGI が組み込まれており、すぐに使える。
単に PHP をハンドラとして設定するだけでよい。
その値の詳しい情報は、[dedicated area on iis.net][php-iis]に PHP 専用のエリアがある。

[php-downloads]: http://windows.php.net
[phpmanager]: http://phpmanager.codeplex.com/
[wpi]: http://www.microsoft.com/web/downloads/platform.aspx
[zsce]: http://www.zend.com/en/products/server-ce/
[xampp]: http://www.apachefriends.org/en/xampp.html
[wamp]: http://www.wampserver.com/
[php-iis]: http://php.iis.net/
