---
title: コーディングスタイル
---

# コーディングスタイル

PHP のコミュニティはとてもでっかくて、いろんな人たちがいる。
そして、数え切れないほどのライブラリやフレームワークそしてコンポーネントが存在する。
そんな中からいくつか選んで、それを組み合わせてひとつのプロジェクトで使うっていうのもよくあることだ。
大切なのは、PHP のコードを書くときに、(できるだけ) 標準的なスタイルに従うことだ。
そうすれば、いろんなライブラリを組み合わせて使うのも簡単になる。

[Framework Interop Group][fig] っていうところ (元は 'PHP Standards Group' という名前だった)
が、おすすめのスタイルを提案している。
[PSR-0][psr0]や[PSR-1][psr1]、そして[PSR-2][psr2]がそれだ。
変な名前のせいでちょっと戸惑うかもしれないけど、これって単に
Drupal や Zend、CakePHP、phpBB、AWS SDK、FuelPHP、Lithium
などのプロジェクトの規約をまとめただけのものなんだ。
自分のプロジェクトでこれを使ってもいいし、今までの自分のスタイルを使い続けてもいい。

理想を言えば、PHP のコードを書くときにはこれらの標準規約に従っておいたほうが無難だ。
そうすれば、他の人にもコードを読んでもらいやすくなるし、手助けも得やすくなるだろう。
この一連のスタイルは、すべて以前のものに対する付け足しの形式になっている。
なので、たとえば PSR-1 を使うなら PSR-0 に従うのが必須となるが、
PSR-2 には従う必要がない。

* [PSR-0 とは][psr0]
* [PSR-1 とは][psr1]
* [PSR-2 とは][psr2]

[PHP_CodeSniffer][phpcs]を使えば、
自分のコードがこれらの標準に準拠しているかどうかを確認できる。
Fabien Potencierが作った[PHP Coding Standards Fixer][phpcsfixer]を使えば、
これらの標準に準拠するようにコードを自動的に修正してくれる。
いちいち手作業で修正する手間を省けるってわけだ。

[fig]: http://www.php-fig.org/
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md
[psr2]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md
[phpcs]: http://pear.php.net/package/PHP_CodeSniffer/
[phpcs-psr]: https://github.com/klaussilveira/phpcs-psr
[phpcsfixer]: http://cs.sensiolabs.org/
