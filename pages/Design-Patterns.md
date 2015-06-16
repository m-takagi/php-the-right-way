---
layout: page
title:  Design Patterns
sitemap: true
---

# デザインパターン

ウェブアプリケーションのコードやプロジェクトを作っていくにはいろんなやりかたがあって、
どんなふうに作るか考え抜くのもありだし適当に作るのもありだ。
でも普通は、一般的なパターンに従うほうがいい。そのほうがコードを管理しやすいし、
他の人にもそのコードを理解してもらいやすくなるからである。

* [Architectural pattern (Wikipedia)](https://en.wikipedia.org/wiki/Architectural_pattern)
* [デザインパターン (Wikipedia)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))
* [実装サンプル集](https://github.com/domnikl/DesignPatternsPHP)

## ファクトリー

最も多用されているデザインパターンのひとつが、ファクトリーパターンだ。このパターンでは、
使いたいオブジェクトを生成するだけのクラスを用意する。ファクトリーパターンの例として、
こんなコードを考えてみよう。

{% highlight php %}
<?php
class Automobile
{
    private $vehicleMake;
    private $vehicleModel;

    public function __construct($make, $model)
    {
        $this->vehicleMake = $make;
        $this->vehicleModel = $model;
    }

    public function getMakeAndModel()
    {
        return $this->vehicleMake . ' ' . $this->vehicleModel;
    }
}

class AutomobileFactory
{
    public static function create($make, $model)
    {
        return new Automobile($make, $model);
    }
}

// ファクトリーを使って Automobile オブジェクトを作る
$veyron = AutomobileFactory::create('ブガッティ', 'ヴェイロン');

print_r($veyron->getMakeAndModel()); // 出力は "ブガッティ ヴェイロン"
{% endhighlight %}

このコードは、ファクトリーパターンを使って Automobile オブジェクトを作る。
こんなふうにするメリットは二つある。
まず、もし後で Automobile クラスに手を入れたり名前を変更したり別のものに入れ替えたりすることになっても簡単にできるということ。
単にファクトリーの中のコードを変更すれば済むわけで、
コードの中で Automobile クラスを使っているところをひとつひとつ修正するとかいうことはしなくてもよい。
二番目のメリットは、仮にオブジェクトの生成が複雑な作業になってしまっても、
そのすべてのファクトリーに閉じ込めてしまえること。
新しいインスタンスを作るたびに毎回同じようなことを繰り返さずに済む。

なにがなんでもファクトリーパターンを使えばいいってわけではない。
この例のコードはとてもシンプルだし、この程度だとファクトリーパターンのおかげで
無駄に複雑になったように見えるかもしれない。
でも、それなりに大規模で込み入ったプロジェクトにかかわる場合は、
変なトラブルに巻き込まれないためにもファクトリーを使っておいたほうがいいだろう。

* [ファクトリーパターン (Wikipedia)](https://ja.wikipedia.org/wiki/Factory_Method_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)

## シングルトン

ウェブアプリケーションを設計するときには、概念的そして構造的に、
特定のクラスのたったひとつのインスタンスにだけアクセスさせるようにしたいということがよくある。
そんなときに使えるのがシングルトンパターンだ。

{% highlight php %}
<?php
class Singleton
{
    /**
     * @var Singleton The reference to *Singleton* instance of this class
     */
    private static $instance;
    
    /**
     * Returns the *Singleton* instance of this class.
     *
     * @return Singleton The *Singleton* instance.
     */
    public static function getInstance()
    {
        if (null === static::$instance) {
            static::$instance = new static();
        }
        
        return static::$instance;
    }

    /**
     * Protected constructor to prevent creating a new instance of the
     * *Singleton* via the `new` operator from outside of this class.
     */
    protected function __construct()
    {
    }

    /**
     * Private clone method to prevent cloning of the instance of the
     * *Singleton* instance.
     *
     * @return void
     */
    private function __clone()
    {
    }

    /**
     * Private unserialize method to prevent unserializing of the *Singleton*
     * instance.
     *
     * @return void
     */
    private function __wakeup()
    {
    }
}

class SingletonChild extends Singleton
{
}

$obj = Singleton::getInstance();
var_dump($obj === Singleton::getInstance());             // bool(true)

$anotherObj = SingletonChild::getInstance();
var_dump($anotherObj === Singleton::getInstance());      // bool(false)

var_dump($anotherObj === SingletonChild::getInstance()); // bool(true)
{% endhighlight %}

このコードは、[*静的* な変数](http://php.net/language.variables.scope#language.variables.scope.static)
と`getInstance()`メソッドを使ってシングルトンパターンを実装している。
これらのことに注目しよう。

* コンストラクタ [`__construct`](http://php.net/language.oop5.decon#object.construct) が protected 宣言されている。これで、このクラスの外部からは `new` 演算子で新しいインスタンスを作れなくなる。
* マジックメソッド [`__clone`](http://php.net/language.oop5.cloning#object.clone) が private 宣言されている。これで、[`clone`](http://php.net/language.oop5.cloning) 演算子でインスタンスをクローンしようとしてもできなくなる。
* マジックメソッド [`__wakeup`](http://php.net/language.oop5.magic#object.wakeup) が private 宣言されている。これで、グローバル関数 [`unserialize()`](http://php.net/function.unserialize) でのインスタンスのアンシリアライズができなくなる。
* 新しいインスタンスを作るには、静的な作成用メソッド `getInstance()` による [遅延静的束縛](http://php.net/language.oop5.late-static-bindings) を使う。このメソッドの中ではキーワード `static` が使われていて、サンプルのように `Singleton` のサブクラスを作れるようになる。

シングルトンパターンが有用なのは、たとえばウェブアプリケーションのリクエスト全体で、
たったひとつのインスタンスだけしかないことを保証しないといけない場合だ。
Configurationクラスみたいなグローバルオブジェクトを使っていたり、
イベントキューみたいな共有リソースを使っていたりする場合に活用できる。

ただ、シングルトンを使うときには注意が必要だ。その性質上、シングルトンパターンを使うと
アプリケーションにグローバルな状態を導入することになってしまい、テストがしにくくなる。
シングルトンを使いたいという場面では、たいていの場合は依存性の注入が代わりに使える
(し、むしろそっちを使うべきだ)。依存性の注入を使えば、不要な結合を
アプリケーションの設計から取り除ける。というのも、共有リソースやグローバルリソースを使う
オブジェクトが具象クラスについて知らなくてもよくなるからだ。

* [シングルトンパターン (Wikipedia)](https://en.wikipedia.org/wiki/Singleton_pattern)

## ストラテジー

ストラテジーパターンを使うと、一連のアルゴリズム群をカプセル化して、
クライアントクラス側から特定のアルゴリズムのインスタンスを作れるようになる。
このときに、実際の実装に関する知識はなくてもかまわない。
このパターンにはいくつかのバリエーションがあるけれど、ここでは一番シンプルなものを解説する。

最初に示すのは、アルゴリズム群を表すコード片だ。
シリアライズした配列、JSON、あるいは単なるデータの配列が欲しいとしよう。
{% highlight php %}
<?php

interface OutputInterface
{
    public function load();
}

class SerializedArrayOutput implements OutputInterface
{
    public function load()
    {
        return serialize($arrayOfData);
    }
}

class JsonStringOutput implements OutputInterface
{
    public function load()
    {
        return json_encode($arrayOfData);
    }
}

class ArrayOutput implements OutputInterface
{
    public function load()
    {
        return $arrayOfData;
    }
}
{% endhighlight %}

このようにアルゴリズム群をカプセル化しておけば、使う側のコードからもうまい具合に使えるようになる。
また、他の開発者が新たな出力形式を追加したとしても、使う側のコードは変更する必要がない。

個々の具象「出力」クラスがOutputInterfaceを実装していることに気づくだろう。
その目的は二つ。まずは、具象実装が従うべきシンプルな規約をあてはめること。
そしてもう一つは、このように共通のインターフェイスを実装することで、
次のセクションで説明する
[タイプヒンティング](http://php.net/language.oop5.typehinting)
が使え、正しい型（この場合は'OutputInterface'）を使っていることを保証できるということだ。

次のコードは、呼び出し側のクライアントクラスが実際に特定のアルゴリズムを使ったり、
必要な振る舞いを実行時に設定したりするものだ。
<?php
class SomeClient
{
    private $output;

    public function setOutput(OutputInterface $outputType)
    {
        $this->output = $outputType;
    }

    public function loadOutput()
    {
        return $this->output->load();
    }
}
{% endhighlight %}

このクライアントクラスにはprivateなプロパティが用意されている。
実行時には、必ず'OutputInterface'型を設定しておく必要がある。
このプロパティを設定すれば、loadOutput()を呼んだときに、
指定した型の具象クラスのload()メソッドが呼ばれることになる。
{% highlight php %}
<?php
$client = new SomeClient();

// 配列が使いたければ
$client->setOutput(new ArrayOutput());
$data = $client->loadOutput();

// JSONが使いたければ
$client->setOutput(new JsonStringOutput());
$data = $client->loadOutput();

{% endhighlight %}

* [ストラテジーパターン (Wikipedia)](http://en.wikipedia.org/wiki/Strategy_pattern)

## フロントコントローラ

フロントコントローラパターンは、ウェブアプリケーションのエントリポイントをひとつだけ (例: index.php) にして
そこですべてのリクエストを処理するというパターンだ。このエントリポイントが、
依存情報を読み込んだりリクエストを処理したりレスポンスをブラウザに返したりといった責務を負う。
フロントコントローラを利用すると、コードのモジュール化が進めやすくなる。
また、すべてのリクエストに対して実行したいコード
(入力のチェックなど) をフックとして組み込みやすくなる。

* [フロントコントローラパターン (Wikipedia)](https://en.wikipedia.org/wiki/Front_Controller_pattern)

## Model-View-Controller

モデル=ビュー=コントローラ (MVC) パターン、そしてその関連パターンである HMVC や MVVM
を使うと、コードを論理的に分解してそれぞれ特定の役割を担わせることができる。
モデルはデータアクセス層を扱い、データを取得してそれをアプリケーションで使いやすい形式で返す。
コントローラはリクエストを扱い、モデルから受け取ったデータを処理してビューを読み込み、
それをレスポンスとして返す。
ビューはテンプレート (マークアップや xml など) を扱い、これをレスポンスとしてウェブブラウザに返す。

MVC は最も一般的なアーキテクチャパターンで、主要な [PHP フレームワーク](https://github.com/codeguy/php-the-right-way/wiki/Frameworks)
でも採用されている。

MVC やその関連パターンについてさらに知りたければ、これらを参考にしよう。

* [MVC](https://ja.wikipedia.org/wiki/Model_View_Controller)
* [HMVC](https://en.wikipedia.org/wiki/Hierarchical_model%E2%80%93view%E2%80%93controller)
* [MVVM](https://ja.wikipedia.org/wiki/Model_View_ViewModel)
