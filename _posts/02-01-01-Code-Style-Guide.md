---
title: コーディングスタイル
anchor: code_style_guide
---

# コーディングスタイル  {#code_style_guide_title}

PHP のコミュニティはとてもでっかくて、いろんな人たちがいる。
そして、数え切れないほどのライブラリやフレームワークそしてコンポーネントが存在する。
そんな中からいくつか選んで、それを組み合わせてひとつのプロジェクトで使うっていうのもよくあることだ。
大切なのは、PHP のコードを書くときに、(できるだけ) 標準的なスタイルに従うことだ。
そうすれば、いろんなライブラリを組み合わせて使うのも簡単になる。

[Framework Interop Group][fig] っていうところ
が、おすすめのスタイルを提案している。
コーディングスタイルに関する提案は、[PSR-0][psr0]と[PSR-1][psr1]、[PSR-2][psr2]、そして[PSR-4][psr4]だ。
これって要するに、
Drupal や Zend、Symfony、CakePHP、phpBB、AWS SDK、FuelPHP、Lithium
などのプロジェクトが採用しつつある規約をまとめただけのものなんだ。
自分のプロジェクトでこれを使ってもいいし、今までの自分のスタイルを使い続けてもいい。

理想を言えば、PHP のコードを書くときには、よく知られた何らかの標準規約に従うべきだ。
さっき説明したPSRの組み合わせでもいいし、PEARとかZendのやつでもかまわない。
そうすれば、他の人にもコードを読んでもらいやすくなるし、手助けも得やすくなるだろう。
また、コンポーネントを実装するアプリケーションがいろんなサードパーティのコードを組み合わせても、一貫性を保てる。

* [PSR-0 とは][psr0]
* [PSR-1 とは][psr1]
* [PSR-2 とは][psr2]
* [PSR-4 とは][psr4]
* [PEARのコーディング規約][pear-cs]
* [Symfonyのコーディング規約][symfony-cs]

[PHP_CodeSniffer][phpcs]を使えば、
自分のコードがこれらの標準のどれかひとつに準拠しているかどうかを確認できる。
あと、[Sublime Text 2][st-cs]みたいなテキストエディタのプラグインを使えば、
書いているその場でリアルタイムのフィードバックが得られる。

コードのレイアウトを自動的に修正するツールとしては、二つの選択肢がある。
[PHP Coding Standards Fixer][phpcsfixer]がそのひとつで、
これはきちんとテストされているコードだ。
もうひとつの選択肢が[php.tools][phptools]だ。これは、
[sublime-phpfmt][sublime-phpfmt]エディタ用のプラグインのおかげで有名になった。
後発なだけあって、パフォーマンスは大きく改善されている。エディタ上でのリアルタイムの修正も、無理なくできる。

phpcs をシェルから手動で実行することもできる。

    phpcs -sw --standard=PSR2 file.php

これは、エラーの内容とその修正方法を表示してくれる。
このコマンドをgit hookに仕込んでおけば便利だろう。
そうすれば、標準規約に反する変更を含むブランチは、それを修正するまでリポジトリに投入できなくなる。
until those violations have been fixed.

変数名や関数名、そしてディレクトリ名なんかは、英語にしておくことをおすすめする。
コードのコメントに関しては、別に英語にこだわらなくてもかまわない。
そのコードを扱う(将来扱う可能性がある)すべての人が読みやすいものであれば、何語でもかまわない。

[fig]: http://www.php-fig.org/
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md
[psr2]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md
[pear-cs]: http://pear.php.net/manual/ja/standards.php
[symfony-cs]: http://symfony.com/doc/current/contributing/code/standards.html
[phpcs]: http://pear.php.net/package/PHP_CodeSniffer/
[st-cs]: https://github.com/benmatselby/sublime-phpcs
[phpcsfixer]: http://cs.sensiolabs.org/
[phptools]: https://github.com/dericofilho/php.tools
[sublime-phpfmt]: https://github.com/dericofilho/sublime-phpfmt
