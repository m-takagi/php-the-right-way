---
isChild: true
---

## プログラミングのパラダイム

PHP は柔軟性のある動的言語で、いろんなプログラミングテクニックに対応している。
長年の間に劇的に成長してきた。PHP 5.0 でのオブジェクト指向モデルの追加 (2004 年)、
PHP 5.3 での無名関数や名前空間の追加 (2009 年)、そして
PHP 5.4 でのトレイトの追加 (2012 年) などが特筆すべきところだろう。

### オブジェクト指向プログラミング

* [PHP のオブジェクト指向][oop]
* [トレイト][traits]

### 関数プログラミング

PHP 5.3 からは無名関数が使えるようになった。

{% highlight php %}
<?php
$greet = function($name)
{
    print("Hello {$name}");
};

$greet('World');
{% endhighlight %}

* [無名関数][anonymous-functions]
* [`call_user_func_array`による動的な関数実行][call-user-func-array]

### メタプログラミング

Ruby の人がよく「PHP には `method_missing` ってないよね」とか言うんだけど、
違うよ。`__call()` があるよ。それ以外にもいろんなマジックメソッドがある。
`__get()` とか `__set()` とか `__clone()` とか `__toString()` とかね。

* [マジックメソッド][magic-methods]
* [リフレクション][reflection]

[namespaces]: http://php.net/manual/ja/language.namespaces.php
[overloading]: http://php.net/manual/ja/language.oop5.overloading.php
[oop]: http://www.php.net/manual/ja/language.oop5.php
[anonymous-functions]: http://www.php.net/manual/ja/functions.anonymous.php
[magic-methods]: http://php.net/manual/ja/language.oop5.magic.php
[reflection]: http://www.php.net/manual/ja/intro.reflection.php
[traits]: http://www.php.net/traits
[call-user-func-array]: http://php.net/manual/ja/function.call-user-func-array.php
