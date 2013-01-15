---
layout: page
title: Design Patterns
---

# デザインパターン

ウェブアプリケーションのコードやプロジェクトを作っていくにはいろんなやりかたがあって、
どんなふうに作るか考え抜くのもありだし適当に作るのもありだ。
でも普通は、一般的なパターンに従うほうがいい。そのほうがコードを管理しやすいし、
他の人にもそのコードを理解してもらいやすくなるからである。

* [Architectural pattern (Wikipedia)](https://en.wikipedia.org/wiki/Architectural_pattern)
* [デザインパターン (Wikipedia)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))

## ファクトリー

最も多用されているデザインパターンのひとつが、ファクトリーパターンだ。このパターンでは、
使いたいオブジェクトを生成するだけのクラスを用意する。ファクトリーパターンの例として、
こんなコードを考えてみよう。

{% highlight php %}
<?php
class Automobile
{
    private $vehicle_make;
    private $vehicle_model;

    public function __construct($make, $model)
    {
        $this->vehicle_make = $make;
        $this->vehicle_model = $model;
    }

    public function get_make_and_model()
    {
        return $this->vehicle_make . ' ' . $this->vehicle_model;
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

print_r($veyron->get_make_and_model()); // 出力は "ブガッティ ヴェイロン"
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
    static $instance;

    private function __construct()
    {
    }

    public static function getInstance()
    {
        if (!isset(self::$instance)) {
            self::$instance = new self();
        }

        return self::$instance;
    }
}

$instance1 = Singleton::getInstance();
$instance2 = Singleton::getInstance();

echo $instance1 === $instance2; // 出力は 1
{% endhighlight %}

このコードは、静的スコープの変数と`getInstance()`メソッドを使ってシングルトンパターンを実装している。
コンストラクタがprivate宣言されていることに注目。つまり、外部から`new`でインスタンスを作ることはできないってことだ。

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
