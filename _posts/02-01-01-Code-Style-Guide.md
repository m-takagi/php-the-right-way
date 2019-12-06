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
コーディングスタイルとは関係ないものもあるけれど、[PSR-1][psr1]、[PSR-12][psr12]、そして[PSR-4][psr4]はコーディングスタイルを扱っている。
これって要するに、
Drupal や Zend、Symfony、Laravel、CakePHP、phpBB、AWS SDK、FuelPHP、Lithium
などのプロジェクトが採用しつつある規約をまとめただけのものなんだ。
自分のプロジェクトでこれを使ってもいいし、今までの自分のスタイルを使い続けてもいい。

理想を言えば、PHP のコードを書くときには、よく知られた何らかの標準規約に従うべきだ。
さっき説明したPSRの組み合わせでもいいし、PEARとかZendのやつでもかまわない。
そうすれば、他の人にもコードを読んでもらいやすくなるし、手助けも得やすくなるだろう。
また、コンポーネントを実装するアプリケーションがいろんなサードパーティのコードを組み合わせても、一貫性を保てる。

* [PSR-1 とは][psr1]
* [PSR-12 とは][psr12]
* [PSR-4 とは][psr4]
* [PEARのコーディング規約][pear-cs]
* [Symfonyのコーディング規約][symfony-cs]

[PHP_CodeSniffer][phpcs]を使えば、
自分のコードがこれらの標準のどれかひとつに準拠しているかどうかを確認できる。
あと、[Sublime Text][st-cs]みたいなテキストエディタのプラグインを使えば、
書いているその場でリアルタイムのフィードバックが得られる。

コードのレイアウトを自動的に修正するツールとしては、二つの選択肢がある。

- [PHP Coding Standards Fixer][phpcsfixer]がそのひとつで、これはきちんとテストされているコードだ。
- もうひとつの選択肢は [PHP Code Beautifier and Fixer][phpcbf] で、これはPHP_CodeSnifferに含まれている。これを使って、コードのレイアウトを適切に調整できる。

phpcs をシェルから手動で実行することもできる。

    phpcs -sw --standard=PSR1 file.php

これは、エラーの内容とその修正方法を表示してくれる。
このコマンドをgit hookに仕込んでおけば便利だろう。
そうすれば、標準規約に反する変更を含むブランチは、それを修正するまでリポジトリに投入できなくなる。

PHP_CodeSnifferを使っている場合は、指摘されたコードレイアウトの問題を自動的に修正することもできる。そのためには[PHP Code Beautifier and Fixer][phpcbf]を使えばいい。

    phpcbf -w --standard=PSR1 file.php

もうひとつの選択肢は[PHP Coding Standards Fixer][phpcsfixer]で、
これは、実際に修正するまえにコードにどんな問題があったのかを表示してくれる。

    php-cs-fixer fix -v --rules=@PSR1 file.php

変数や関数などのシンボル名、そしてディレクトリ名などのコード基盤なんかは、英語にしておくことをおすすめする。
コードのコメントに関しては、別に英語にこだわらなくてもかまわない。
そのコードを扱う(将来扱う可能性がある)すべての人が読みやすいものであれば、何語でもかまわない。

PHPでクリーンなコードを書くための資料としておすすめなのが [Clean Code PHP][cleancode] だ。

[fig]: https://www.php-fig.org/
[psr1]: https://www.php-fig.org/psr/psr-1/
[psr12]: https://www.php-fig.org/psr/psr-12/
[psr4]: https://www.php-fig.org/psr/psr-4/
[pear-cs]: https://pear.php.net/manual/en/standards.php
[symfony-cs]: https://symfony.com/doc/current/contributing/code/standards.html
[phpcs]: https://pear.php.net/package/PHP_CodeSniffer/
[phpcbf]: https://github.com/squizlabs/PHP_CodeSniffer/wiki/Fixing-Errors-Automatically
[st-cs]: https://github.com/benmatselby/sublime-phpcs
[phpcsfixer]: https://cs.sensiolabs.org/
[cleancode]: https://github.com/jupeter/clean-code-php
