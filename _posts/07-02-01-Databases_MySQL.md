---
isChild: true
title:   MySQL 用の拡張モジュール
anchor:  mysql_extension
---

## MySQL 用の拡張モジュール {#mysql_extension_title}

PHP 用の [mysql] 拡張モジュールの開発はすでに終了しており、 [PHP 5.5.0で正式に「廃止予定」となった][mysql_deprecated]。
つまり、近い将来に削除されるということだ。
もし未だに `mysql_connect()` とか `mysql_query()` のような `mysql_*` 系の関数を使っているなら、
いずれ書き直さざるを得なくなる。mysql を使っているプログラムがあれば、
今のうちに [mysqli] か [PDO] を使うように書き直しておこう。
そうすれば、後になって焦らずに済む。

**今から新しく書き始めるっていうのなら、[mysql] 拡張モジュールを使うっていう選択肢はナシだ。
[MySQLi 拡張モジュール][mysqli] か [PDO] を使うこと。**

* [PHP: MySQL 用の API の選択肢][mysql_api]
* [MySQL開発者用のPDOチュートリアル][pdo4mysql_devs]


[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysqli]: http://php.net/mysqli
[pdo]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers
