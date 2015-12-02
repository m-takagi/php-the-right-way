---
isChild: true
title:   MySQL 用の拡張モジュール
anchor:  mysql_extension
---

## MySQL 用の拡張モジュール {#mysql_extension_title}

PHP 用の [mysql] 拡張モジュールは既に思いっきり古くなっていて、これらの拡張モジュールがその後継になっている。

- [mysqli]
- [pdo]

[mysql] 拡張モジュールの開発は大昔に終了していて、 [PHP 5.5.0で「廃止予定」となった][mysql_deprecated]。
そして、 **[PHP 7.0.0で公式に削除された][mysql_removed]** 。

To save digging into your `php.ini` settings to see which module you are using, one option is to search for `mysql_*` 
in your editor of choice. If any functions such as `mysql_connect()` and `mysql_query()` show up, then `mysql` is 
in use.

Even if you are not using PHP 7.0 yet, failing to consider this upgrade as soon as possible will lead to greater 
hardship when the PHP 7.0 upgrade does come about. The best option is to replace mysql usage with [mysqli] or [PDO] in 
your applications within your own development schedules so you won't be rushed later on.

**If you are upgrading from [mysql] to [mysqli], beware lazy upgrade guides that suggest you can simply find and replace `mysql_*` with `mysqli_*`. Not only is that a gross oversimplification, it misses out on the advantages that mysqli provides, such as parameter binding, which is also offered in [PDO][pdo].**

* [PHP: MySQL 用の API の選択肢][mysql_api]
* [MySQL開発者用のPDOチュートリアル][pdo4mysql_devs]

[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysql_removed]: http://php.net/manual/ja/migration70.removed-exts-sapis.php
[mysqli]: http://php.net/mysqli
[pdo]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers
