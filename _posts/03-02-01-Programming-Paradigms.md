---
title: プログラミングのパラダイム
isChild: true
anchor:  programming_paradigms
---

## プログラミングのパラダイム {#programming_paradigms_title}

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

PHP は、ファーストクラスの関数をサポートしている。
つまり、関数を変数に代入できるってことだ。
自分で定義した関数だろうがもともと組み込まれている関数だろうが、
変数で参照したり動的に実行したりできる。
何かの関数を別の関数の引数として渡すこと (高階関数っていう機能)
もできるし、関数の返り値を別の関数にすることもできる。

再帰 (ある関数の中から自分自身を呼ぶこと) も PHP の機能としてサポートしている。
しかし、たいていの PHP コードはそれよりも逐次処理を重視している。

新型の無名関数 (クロージャにも対応したもの) が使えるようになったのは、PHP 5.3 (2009 年) 以降だ。

PHP 5.4 からは、クロージャをオブジェクトのスコープにバインドできるようになった。
また callable のサポートも強化され、ほとんどの場合で無名関数と互換性を持つようになった。

* 詳しくは [PHP における関数型プログラミング](/pages/Functional-Programming.html) で
* [無名関数][anonymous-functions]
* [Closure クラス][closure-class]
* [クロージャの詳細を知りたければ、RFCを読めばいいよ][closures-rfc]
* [Callable][callables]
* [`call_user_func_array()`による動的な関数実行][call-user-func-array]

### メタプログラミング

PHP はいろんな形式のメタプログラミングに対応しており、リフレクション API やマジックメソッドが使える。
マジックメソッドには `__get()` や `__set()`、`__clone()`、`__toString()`、そして `__invoke()`
などがあり、これらを活用すればクラスの振る舞いをフックできる。
Ruby の人がよく「PHP には `method_missing` がなくてさあ」とか言うけど、ちゃんと
`__call()` とか `__callStatic()` があるよ。

* [マジックメソッド][magic-methods]
* [リフレクション][reflection]
* [オーバーロード][overloading]


[oop]: http://php.net/language.oop5
[traits]: http://php.net/language.oop5.traits
[anonymous-functions]: http://php.net/functions.anonymous
[closure-class]: http://php.net/class.closure
[closures-rfc]: https://wiki.php.net/rfc/closures
[callables]: http://php.net/language.types.callable
[call-user-func-array]: http://php.net/function.call-user-func-array
[magic-methods]: http://php.net/language.oop5.magic
[reflection]: http://php.net/intro.reflection
[overloading]: http://php.net/language.oop5.overloading

