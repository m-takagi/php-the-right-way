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

* [PHP: MySQL 用の API の選択肢][mysql_api]
* [MySQL開発者用のPDOチュートリアル][pdo4mysql_devs]

[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysql_removed]: http://php.net/manual/ja/migration70.removed-exts-sapis.php
[mysqli]: http://php.net/mysqli
[pdo]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers
