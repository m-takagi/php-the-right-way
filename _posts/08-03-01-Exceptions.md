---
title: 例外処理
isChild: true
---

## 例外処理 {#exceptions_title}

最近はやりのプログラミング言語には、たいてい例外処理の仕組みがある。でも PHP の人は見落としがちだ。
たとえば Ruby なんかは例外処理を使いまくっている。
HTTP リクエストに失敗したときもデータベースへのクエリが失敗したときも、画像ファイルが見つからなかったときでさえも、
Ruby のプログラム (あるいはその中で使っている gem) は例外を投げる。
なので、画面を見ればすぐに「ああ、何か間違えたんだな」とわかる。

PHP はそのあたりが極めて緩くて、たとえば `file_get_contents()` が失敗しても単に
`FALSE` を返して警告を出すだけだ。古いフレームワークの多くもそんな感じだ。
たとえば CodeIgniter の場合は、単に false を返してログファイルにメッセージを書き出すだけ。
原因を知りたければ `$this->upload->get_error()` みたいなメソッドを実行することになる。
何が問題かというと、まずエラーが発生したのかどうかを自分で調べないといけないこと。
そして次に、エラー情報を取得する方法をドキュメントで調べないといけないこと。
もっとはっきりわかるようにしてくれたらいいのに。

別の問題もある。何かのクラスがエラーを画面に投げっぱなしにしてそのまま終わるような場合だ。
そんなことをすれば、エラーがあったときにプログラム中で動的に対応することができなくなってしまう。
そんな場合は、エラーではなく例外を発生させないといけない。
例外にしておけば開発者がエラーに気づけるし、プログラムの中で対応できるようになる。
たとえばこんな感じだ。

{% highlight php %}
<?php
$email = new Fuel\Email;
$email->subject('タイトル');
$email->body('ごきげんいかが？');
$email->to('guy@example.com', '誰かさん');

try
{
    $email->send();
}
catch(Fuel\Email\ValidationFailedException $e)
{
    // 検証に失敗した
}
catch(Fuel\Email\SendingFailedException $e)
{
    // ドライバがメールを送れなかった
}
finally
{
    // 例外が発生してもしなくても、ここは必ず実行される
}
{% endhighlight %}

### SPL の例外

汎用的な `Exception` クラスには、開発者がデバッグするためのコンテキスト情報がほとんど含まれていない。
これを改善するには、特化型の `Exception` を作ればいい。つまり、`Exception` クラスのサブクラスを作るってことだ。

{% highlight php %}
<?php
class ValidationException extends Exception {}
{% endhighlight %}

こんなふうにすれば、catch ブロックを複数用意してそれぞれの例外で別の処理をできるようになる。
その結果、自作の例外クラスが <em>大量に</em> できあがってしまうかもしれないが、
[SPL 拡張モジュール][splext] が用意する例外クラスを使えば少しはましになるだろう。

たとえば、マジックメソッド `__call()` を使っているときに、無効なメソッドを要求されたとしよう。
標準の Exception クラスを使うのは曖昧すぎるし、そのためだけに専用の例外クラスを作るのも何だし、
という場合には単に `throw new BadFunctionCallException;` とすればよい。

* [例外について][exceptions]
* [SPL 拡張モジュール][splexe]
* [PHP での例外のネスト][nesting-exceptions-in-php]
* [PHP 5.3での例外処理のベストプラクティス][exception-best-practices53]

[exceptions]: http://php.net/manual/ja/language.exceptions.php
[splexe]: http://php.net/manual/ja/spl.exceptions.php
[splext]: /#standard_php_library
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/
