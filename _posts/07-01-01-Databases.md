---
title: データベース
anchor: databases
---

# データベース {#databases_title}

PHP のコードを書いていると、情報を保存するためにデータベースを使うことが多くなる。
データベースを操作するには、いくつかの方法がある。
**PHP 5.1.0 の時代までは** 、おすすめの方法は [mysqli]、[pgsql]、[mssql]
などのネイティブドライバを使うことだった。

このデータベースしか使わないよ！っていうのならネイティブドライバもいい。
でもたとえば、MySQL を使っているけど一部は MSSQL であるとか、
Oracle にもつなぐことがあるとか、そんな場合は同じドライバでは対応できない。
そして、データベースが変わるたびに新しい API を覚えないといけないことになる。
ばかげた話だ。


[mysqli]: https://secure.php.net/mysqli
[pgsql]: https://secure.php.net/pgsql
[mssql]: https://secure.php.net/mssql
