---
isChild: true
title: 抽象化レイヤー
anchor: databases_abstraction_layers
---

## 抽象化レイヤー {#databases_abstraction_layers_title}

多くのフレームワークは、データベースの抽象化レイヤーを用意している。[PDO][1]を利用しているものもあれば、そうでないものもある。
あるデータベースには存在するけれども別のデータベースには存在しない機能などをエミュレートするために、
クエリーをPHPのメソッドでラップしたりするものだ。
PDOではデータベースへの接続の抽象化しかしないが、フレームワークの抽象化レイヤーは、それ以上のことをしてくれる。
もちろんそれなりのオーバーヘッドが発生するだろう。
でも、MySQLでもPostgreSQLでもSQLiteでも動くようなアプリケーションを書いているのなら、
多少のオーバーヘッドを割り引いてでも、すっきりしたコードを書けるほうがうれしいだろう。

以下の抽象化レイヤーは、 [PSR-0][psr0] や [PSR-4][psr4] で定められた標準名前空間に従っている。
これらはアプリケーションを問わずに利用できる。

* [Aura SQL][6]
* [Doctrine2 DBAL][2]
* [Propel][7]
* [Zend-db][4]


[1]: https://secure.php.net/book.pdo
[2]: https://www.doctrine-project.org/projects/dbal.html
[4]: https://packages.zendframework.com/docs/latest/manual/en/index.html#zendframework/zend-db
[6]: https://github.com/auraphp/Aura.Sql
[7]: http://propelorm.org/
[psr0]: https://www.php-fig.org/psr/psr-0/
[psr4]: https://www.php-fig.org/psr/psr-4/
