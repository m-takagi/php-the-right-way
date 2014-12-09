---
title: 基本的な概念
isChild: true
anchor:  basic_concept
---

## 基本的な概念 {#basic_concept_title}

考えかたを説明するために、シンプルな例を示そう。細かいところは手を抜いているけどね。

ここに `Database` クラスがある。こいつがデータベースとやりとりするためには、何らかのアダプターが必要だ。
そこで、コンストラクタの中でアダプターのインスタンスを作っている。アダプターの名前を直接指定してね。
こんな書きかただと `Database` クラスを単体でテストするのが難しくなるし、
このクラスがアダプターと密接に結びついてしまう。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

このコードを書き換えて依存性の注入を使うようにすれば、この依存関係を緩やかにできる。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

これで、依存関係を外部から `Database` クラスに渡せるようになった。このクラス自身に作らせる必要がなくなったんだ。
コンストラクタで指定する以外にも、依存関係を引数で受け取ってそれを設定するようなメソッドを新たに作ってもいいし、
あるいは `$adapter` という `public` プロパティを作って、依存関係を直接設定できるようにしてもいい。
