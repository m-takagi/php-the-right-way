---
isChild: true
---

## プログラミングのパラダイム

PHP は柔軟性のある動的言語で、いろんなプログラミングテクニックに対応している。
長年の間に劇的に成長してきた。PHP 5.0 でのオブジェクト指向モデルの追加 (2004 年)、
PHP 5.3 での無名関数や名前空間の追加 (2009 年)、そして
PHP 5.4 でのトレイトの追加 (2012 年) などが特筆すべきところだろう。

### オブジェクト指向プログラミング

PHP には完全なオブジェクト指向プログラミングの機能が搭載されている。
クラスや抽象クラス、インターフェイス、継承、コンストラクタ、クローン、
例外などなどといった機能も、当然使える。

* [PHP のオブジェクト指向][oop]
* [トレイト][traits]

### 関数プログラミング

PHP 5.3 からは無名関数やクロージャが使えるようになった。

{% highlight php %}
<?php
$greet = function($name)
{
    print("Hello {$name}");
};

$greet('World');
{% endhighlight %}

PHP 5.4 からは、クロージャをオブジェクトのスコープにバインドできるようになった。
また callable のサポートも強化され、ほとんどの場合で無名関数と互換性を持つようになった。

* [無名関数][anonymous-functions]
* [Closure クラス][closure-class]
* [Callable][callables]
* [`call_user_func_array`による動的な関数実行][call-user-func-array]

### メタプログラミング

PHP はいろんな形式のメタプログラミングに対応しており、リフレクション API やマジックメソッドが使える。
マジックメソッドには `__get()` や `__set()`、`__clone()`、`__toString()`、そして `__invoke()`
などがあり、これらを活用すればクラスの振る舞いをフックできる。
Ruby の人がよく「PHP には `method_missing` がなくてさあ」とか言うけど、ちゃんと
`__call()` とか `__callStatic()` があるよ。

* [マジックメソッド][magic-methods]
* [リフレクション][reflection]

[namespaces]: http://php.net/manual/ja/language.namespaces.php
[overloading]: http://php.net/manual/ja/language.oop5.overloading.php
[oop]: http://www.php.net/manual/ja/language.oop5.php
[anonymous-functions]: http://www.php.net/manual/ja/functions.anonymous.php
[closure-class]: http://php.net/manual/ja/class.closure.php
[callables]: http://php.net/manual/ja/language.types.callable.php
[magic-methods]: http://php.net/manual/ja/language.oop5.magic.php
[reflection]: http://www.php.net/manual/ja/intro.reflection.php
[traits]: http://www.php.net/traits
[call-user-func-array]: http://php.net/manual/ja/function.call-user-func-array.php
