---
layout: page
title: Functional Programming in PHP
---

# PHP における関数型プログラミング

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

高階関数の最もよくある使いかたは、ストラテジーパターンの実装だ。
組み込みの `array_filter` 関数は、入力の配列 (データ)
と関数 (ストラテジーあるいはコールバック) を受け取って、
配列の各要素に対してその関数をフィルターとして適用する。

{% highlight php %}
<?php
$input = array(1, 2, 3, 4, 5, 6);

// 新たな無名関数を作って、変数に代入する
$filter_even = function($item) {
    return ($item % 2) == 0;
};

// array_filter は、データと関数を受け取る
$output = array_filter($input, $filter_even);

// 関数をいったん変数に代入せずに、直接このように書いてもかまわない
$output = array_filter($input, function($item) {
    return ($item % 2) == 0;
});

print_r($output);
{% endhighlight %}

クロージャは無名関数の一種で、外部のスコープからインポートした変数に対してグローバル変数を介さずにアクセスできる。
理論的には、クロージャというのは関数の一種であり、定義時に一部の引数を環境に閉じた (固定した) ものである。
クロージャを使えば、変数のスコープの制約をすっきりとした方法で回避できる。

次の例では、クロージャを使って関数を定義する。この関数はフィルター関数群の中から
`array_filter` 用のフィルター関数を一つ返すものだ。

{% highlight php %}
<?php
/**
 * フィルター用の無名関数 (items > $min を通すもの) を作る
 *
 * 「n より大きい」フィルター群の中からひとつのフィルターを返す
 */
function criteria_greater_than($min)
{
    return function($item) use ($min) {
        return $item > $min;
    };
}

$input = array(1, 2, 3, 4, 5, 6);

// array_filter を使い、入力値とフィルター関数を指定する
$output = array_filter($input, criteria_greater_than(3));

print_r($output); // items > 3
{% endhighlight %}

フィルター関数群の各関数は、何らかの最小値を上回る要素だけを通過させる。
`criteria_greater_than` が返すフィルターはクロージャであり、
`$min` 引数の値 (`criteria_greater_than` の呼び出し時に引数として渡したもの)
がスコープ内で束縛されている。

デフォルトでは、`$min` 変数の値を関数にインポートするときに早期束縛を使う。
遅延束縛を使った真のクロージャを使いたい場合は、インポートの際に参照を使わないといけない。
テンプレート、あるいは入力バリデーションライブラリを考えてみよう。
こんな場合、クロージャを定義してスコープ内の変数を捕捉しておいて、
あとで無名関数が評価されたときにそれにアクセスすることになる。

* [無名関数][anonymous-functions]
* [クロージャの詳細を知りたければ、RFCを読めばいいよ][closures-rfc]
* [`call_user_func_array`による動的な関数実行][call-user-func-array]

[anonymous-functions]: http://www.php.net/manual/ja/functions.anonymous.php
[call-user-func-array]: http://php.net/manual/ja/function.call-user-func-array.php
[closures-rfc]: https://wiki.php.net/rfc/closures
