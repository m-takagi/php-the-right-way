---
title: コーディングスタイル
---

# コーディングスタイル  {#code_style_guide_title}

PHP のコミュニティはとてもでっかくて、いろんな人たちがいる。
そして、数え切れないほどのライブラリやフレームワークそしてコンポーネントが存在する。
そんな中からいくつか選んで、それを組み合わせてひとつのプロジェクトで使うっていうのもよくあることだ。
大切なのは、PHP のコードを書くときに、(できるだけ) 標準的なスタイルに従うことだ。
そうすれば、いろんなライブラリを組み合わせて使うのも簡単になる。

[Framework Interop Group][fig] っていうところ
が、おすすめのスタイルを提案している。
[PSR-0][psr0]や[PSR-1][psr1]、、そして[PSR-2][psr2]がそれだ。
変な名前のせいでちょっと戸惑うかもしれないけど、これって単に
Drupal や Zend、Symfony、CakePHP、phpBB、AWS SDK、FuelPHP、Lithium
などのプロジェクトの規約をまとめただけのものなんだ。
自分のプロジェクトでこれを使ってもいいし、今までの自分のスタイルを使い続けてもいい。

理想を言えば、PHP のコードを書くときには、よく知られた何らかの標準規約に従うべきだ。
さっき説明したPSRの組み合わせでもいいし、PEARとかZendのやつでもかまわない。
そうすれば、他の人にもコードを読んでもらいやすくなるし、手助けも得やすくなるだろう。
また、コンポーネントを実装するアプリケーションがいろんなサードパーティのコードを組み合わせても、一貫性を保てる。

* [PSR-0 とは][psr0]
* [PSR-1 とは][psr1]
* [PSR-2 とは][psr2]
* [PEARのコーディング規約][pear-cs]
* [Zendのコーディング規約][zend-cs]

[PHP_CodeSniffer][phpcs]を使えば、
自分のコードがこれらの標準のどれかひとつに準拠しているかどうかを確認できる。
あと、[Sublime Text 2][st-cs]みたいなテキストエディタのプラグインを使えば、
書いているその場でリアルタイムのフィードバックが得られる。

Fabien Potencierが作った[PHP Coding Standards Fixer][phpcsfixer]を使えば、
これらの標準に準拠するようにコードを自動的に修正してくれる。
いちいち手作業で修正する手間を省けるってわけだ。

変数名や関数名、そしてディレクトリ名なんかは、英語にしておくことをおすすめする。
コードのコメントに関しては、別に英語にこだわらなくてもかまわない。
そのコードを扱う(将来扱う可能性がある)すべての人が読みやすいものであれば、何語でもかまわない。

[fig]: http://www.php-fig.org/
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md
[psr2]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md
[pear-cs]: http://pear.php.net/manual/ja/standards.php
[zend-cs]: http://framework.zend.com/wiki/display/ZFDEV2/Coding+Standards
[phpcs]: http://pear.php.net/package/PHP_CodeSniffer/
[st-cs]: https://github.com/benmatselby/sublime-phpcs
[phpcsfixer]: http://cs.sensiolabs.org/
