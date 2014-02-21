---
title: データベース
---

# データベース {#databases_title}

PHP のコードを書いていると、情報を保存するためにデータベースを使うことが多くなる。
データベースを操作するには、いくつかの方法がある。
_PHP 5.1.0 の時代までは_ 、おすすめの方法は [mysql][mysql]、[mysqli][mysqli]、[pgsql][pgsql]
などのネイティブドライバを使うことだった。

このデータベースしか使わないよ！っていうのならネイティブドライバもいい。
でもたとえば、MySQL を使っているけど一部は MSSQL であるとか、
Oracle にもつなぐことがあるとか、そんな場合は同じドライバでは対応できない。
そして、データベースが変わるたびに新しい API を覚えないといけないことになる。
ばかげた話だ。

ネイティブドライバについては、もうひとつ注意すべきことがある。
PHP 用の mysql 拡張モジュールの開発はすでに終了しており、PHP 5.5 で正式に
「廃止予定」となった。つまり、近い将来に削除されるということだ。
もし未だに `mysql_connect()` とか `mysql_query()` を使っているなら、
いずれ書き直さざるを得なくなる。mysql を使っているプログラムがあれば、
今のうちに mysqli か PDO を使うように書き直しておこう。
そうすれば、後になって焦らずに済む。
_今から新しく書き始めるっていうのなら、mysql 拡張モジュールを使うっていう選択肢はナシだ。
[MySQLi 拡張モジュール][mysqli] か PDO を使うこと。_

* [PHP: MySQL 用の API の選択肢](http://php.net/manual/ja/mysqlinfo.api.choosing.php)

## PDO

PDO はデータベースとの接続を抽象化するライブラリだ。PHP 5.1.0 以降に組み込まれていて、
いろんなデータベースを同じインターフェイスで扱える。
PDO は、SQL のクエリをデータベースにあわせて変換するものではないし、
もともと存在しない機能をエミュレートするものでもない。
純粋に、いろんなデータベースに同じ API で接続するためのものだ。

もっと大切なことは、`PDO` を使えば、外部からの入力 (ID など)
を安全に SQL クエリに埋め込めるということだ。データベースへの
SQL インジェクション攻撃を心配しなくてもよくなる。
そのためには、PDO ステートメントとバインド変数を使えばよい。

数値の ID をクエリ文字列として受け取る PHP スクリプトを考えてみよう。
渡された ID を使って、データベースからユーザー情報を取り出す。
最初に示すのは `悪い方法` だ。

{% highlight php %}
<?php
$pdo = new PDO('sqlite:users.db');
$pdo->query("SELECT name FROM users WHERE id = " . $_GET['id']); // <-- ダメ、ゼッタイ！
{% endhighlight %}

こんな恐ろしいコードを書いちゃいけない。
これは、クエリ文字列のパラメータを SQL に直に埋め込んでいることになる。
あっという間に攻撃を食らうだろう。
どこかのずるがしこい攻撃者が `id` に渡す内容をひと工夫して
`http://domain.com/?id=1%3BDELETE+FROM+users` みたいな URL を呼んだとしよう。
このとき変数 `$_GET['id']` の内容は `1;DELETE FROM users` となり、全ユーザーが消えてしまうことになる！
こんな書き方ではなく、PDO のバインド変数で ID を受け取らないといけない。

{% highlight php %}
<?php
$pdo = new PDO('sqlite:users.db');
$stmt = $pdo->prepare('SELECT name FROM users WHERE id = :id');
$stmt->bindParam(':id', $_GET['id'], PDO::PARAM_INT); // <-- PDOが自動的にエスケープ処理をする
$stmt->execute();
{% endhighlight %}

これが正しい方法だ。この例では、PDO ステートメントでバインド変数を使っている。
外部からの入力である ID がエスケープされてからデータベースに渡るので、
SQL インジェクション攻撃を受けることがなくなる。

* [PDOについて調べる][1]

あと、データベースのコネクションはリソースを使うということにも気をつけよう。
コネクションを明示的に閉じることを忘れたせいでリソースを食いつぶしてしまうなんて話は珍しくない。
とはいえ、これは別にPHPに限った話でもないけどね。
PDOを使っている場合は、オブジェクトへの参照をすべて削除して(Nullを代入するなどして)
オブジェクトを破棄してしまえば、暗黙のうちにコネクションを閉じることが保証される。
オブジェクトを明示的に破棄しない場合は、スクリプトの実行が終わった時点でPHPが自動的に接続を閉じる。
もちろん、持続的接続を使っている場合は別だ。

* [PDOの接続について調べる][5]

## 抽象化レイヤー

多くのフレームワークが自前の抽象化レイヤーを用意している。
PDO をベースとしたものもあれば、そうでないものもある。
こういった抽象化レイヤーでは、あるデータベースには存在するけれども
別のデータベースには存在しない機能を、PHP のメソッドでエミュレートして
使えるようにしていることが多い。真の意味でのデータベースの抽象化をしているわけだ。
そのぶんオーバーヘッドはある。
ただ、MySQL でも PostgreSQL でも SQLite でも動かせるアプリケーションを作る必要があるという場合は、
コードがぐちゃぐちゃになることを思えば多少のオーバーヘッドは我慢できる。

これらの抽象化レイヤーは [PSR-0][psr0] または [PSR-4][psr4] の標準規格に従った名前空間を使って作られているので、
いろんなアプリケーションに導入できる。

* [Aura SQL][6]
* [Doctrine2 DBAL][2]
* [Propel][7]
* [ZF2 Db][4]
* [ZF1 Db][3]

[1]: http://www.php.net/manual/ja/book.pdo.php
[2]: http://www.doctrine-project.org/projects/dbal.html
[3]: http://framework.zend.com/manual/ja/zend.db.html
[4]: http://packages.zendframework.com/docs/latest/manual/ja/index.html#zend-db
[5]: http://php.net/manual/ja/pdo.connections.php
[6]: https://github.com/auraphp/Aura.Sql
[7]: http://propelorm.org/Propel/

[mysql]: http://php.net/mysql
[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md
