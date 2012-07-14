---
title: Databases
---

# データベース

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
PHP 用の mysql 拡張モジュールの開発はすでに終了しており、PHP 5.4.0 の時点での公式発表によると
「長期的には廃止予定」だ。つまり、近い将来に削除されるということであり、
PHP 5.6 (になるかどうかわからないけど、5.5 の次にくるやつ) の頃には使えなくなるだろう。
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
こんな書き方ではなく、PDO のバインド変数で ID を受け取らないといけない。

{% highlight php %}
<?php
$pdo = new PDO('sqlite:users.db');
$stmt = $pdo->prepare('SELECT name FROM users WHERE id = :id');
$stmt->bindParam(':id', filter_input(INPUT_GET, 'id', FILTER_SANITIZE_NUMBER_INT), PDO::PARAM_INT);
$stmt->execute();
{% endhighlight %}

これが正しい方法だ。この例では、PDO ステートメントでバインド変数を使っている。
外部からの入力である ID がエスケープされてからデータベースに渡るので、
SQL インジェクション攻撃を受けることがなくなる。

* [PDOについて調べる][1]

## 抽象化レイヤー

多くのフレームワークが自前の抽象化レイヤーを用意している。
PDO をベースとしたものもあれば、そうでないものもある。
こういった抽象化レイヤーでは、あるデータベースには存在するけれども
別のデータベースには存在しない機能を、PHP のメソッドでエミュレートして
使えるようにしていることが多い。真の意味でのデータベースの抽象化をしているわけだ。
そのぶんオーバーヘッドはある。
ただ、MySQL でも PostgreSQL でも SQLite でも動かせるアプリケーションを作る必要があるという場合は、
コードがぐちゃぐちゃになることを思えば多少のオーバーヘッドは我慢できる。

これらの抽象化レイヤーは PSR-0 の標準規格に従った名前空間を使って作られているので、
いろんなアプリケーションに導入できる。

* [Doctrine2 DBAL][2]
* [ZF2 Db][4]
* [ZF1 Db][3]

[1]: http://www.php.net/manual/ja/book.pdo.php
[2]: http://www.doctrine-project.org/projects/dbal.html
[3]: http://framework.zend.com/manual/ja/zend.db.html
[4]: http://packages.zendframework.com/docs/latest/manual/ja/zend.db.html

[mysql]: http://php.net/mysql
[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
