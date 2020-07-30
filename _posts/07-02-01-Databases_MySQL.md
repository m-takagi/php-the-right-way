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

いま使っているモジュールがどれなのかを知りたいなら、わざわざ `php.ini` の設定を調べるまでもない。
お好みのエディターで `mysql_*` を検索してみればいい。
`mysql_connect()` とか `mysql_query()` みたいな関数がヒットしたら、 `mysql` モジュールを使ってるってことだ。

当面は PHP 7.x を使うつもりがないのだとしても、今ちゃんと考えておかないと、いざというときに大変なことになる。
いちばんいいのは、通常の開発スケジュールの中で、mysql モジュールを使っている部分を
[mysqli] や [PDO] に徐々に置き換えていくことだ。
そうすれば、後になってあせらずにすむ。

** [mysql] から [mysqli] への移行について、単に「`mysql_*` を `mysqli_*` に置換すればOK」などと書いているような記事には用心すること。話を単純化しすぎているだけではなく、mysqli ならではの利点（パラメータのバインドなど。これは [PDO][pdo] でも用意されている）の活用ができなくなってしまう。 **

* [MySQLi Prepared Statements][mysqli_prepared_statements]
* [PHP: MySQL 用の API の選択肢][mysql_api]

[mysql]: https://secure.php.net/mysqli
[mysql_deprecated]: https://secure.php.net/migration55.deprecated
[mysql_removed]: https://secure.php.net/manual/migration70.removed-exts-sapis.php
[mysqli]: https://secure.php.net/mysqli
[pdo]: https://secure.php.net/pdo
[mysql_api]: https://secure.php.net/mysqlinfo.api.choosing
[mysqli_prepared_statements]: https://websitebeaver.com/prepared-statements-in-php-mysqli-to-prevent-sql-injection
