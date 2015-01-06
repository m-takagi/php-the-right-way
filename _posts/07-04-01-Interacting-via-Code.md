---
isChild: true
title: データベースとのやりとり
anchor: databases_interacting
---

## データベースとのやりとり {#databases_interacting_title}

PHPを勉強し始めたばかりの開発者がやってしまいがちなのが、データベースとのやりとりと画面表示のロジックをごちゃまぜにしてしまうことだ。
たとえば、こんなコードになる。

{% highlight php %}
<ul>
<?php
foreach ($db->query('SELECT * FROM table') as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>";
}
?>
</ul>
{% endhighlight %}

これは、あらゆる意味でよろしくない。
まず何と言っても、デバッグしづらいし、テストもしづらいし、読みづらい。
あと、何も制限をかけていないので、大量のフィールドを出力してしまうことになる。

同じことをもっとすっきり行う方法はいろいろある。[OOP](/#object-oriented-programming)が好きな人向けのやりかたもあれば
[関数型プログラミング](/#functional-programming)が好きな人向けのやりかたもある。
が、まずは、分離することからはじめよう。

これが第一歩だ。

{% highlight php %}
<?php
function getAllFoos($db) {
    return $db->query('SELECT * FROM table');
}

foreach (getAllFoos($db) as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>"; // BAD!!
}
{% endhighlight %}

少しはマシになった。この二つを別々のファイルに分けてしまえば、きれいに分離できるだろう。

次に、このメソッドを保持するクラスを用意する。「モデル」だ。
そして、シンプルな `.php` ファイルをもうひとつ作って、そこに画面表示ロジックを入れる。「ビュー」だ。
これで、何となく [MVC] っぽくなった。これは、多くの [フレームワーク](/#frameworks_title) で使われている、OOPのアーキテクチャだ。

**foo.php**

{% highlight php %}
<?php
$db = new PDO('mysql:host=localhost;dbname=testdb;charset=utf8', 'username', 'password');

// モデルを読み込む
include 'models/FooModel.php';

// インスタンスを作る
$fooList = new FooModel($db);

// ビューを表示する
include 'views/foo-list.php';
{% endhighlight %}


**models/FooModel.php**

{% highlight php %}
<?php
class FooModel()
{
    protected $db;

    public function __construct(PDO $db)
    {
        $this->db = $db;
    }

    public function getAllFoos() {
        return $this->db->query('SELECT * FROM table');
    }
}
{% endhighlight %}

**views/foo-list.php**

{% highlight php %}
<?php foreach ($fooList as $row): ?>
    <?= $row['field1'] ?> - <?= $row['field1'] ?>
<?php endforeach ?>
{% endhighlight %}

本質的にこれは、今どきのフレームワークがやっていることと、ほぼ同じだ。それを手作業でやってみた。
毎回こんなことをする必要はないかもしれないが、画面表示とデータベース操作を混在させすぎると、
[ユニットテスト](/#unit-testing) をしたくなったときにやっかいな問題が発生してしまう。

[PHPBridge] に、 [Creating a Data Class] という記事が公開されている。
ここで扱ったのと同じ話題をとりあげていて、データベース操作に慣れた人にとって最適の記事だ。

[MVC]: http://code.tutsplus.com/tutorials/mvc-for-noobs--net-10488
[PHPBridge]: http://phpbridge.org/
[Creating a Data Class]: http://phpbridge.org/intro-to-php/creating_a_data_class
