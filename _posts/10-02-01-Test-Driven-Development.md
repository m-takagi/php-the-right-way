---
title: テスト駆動開発
isChild: true
---

## テスト駆動開発 {#test_driven_development_title}

[Wikipedia](http://en.wikipedia.org/wiki/Test-driven_development) によると、

> Test-driven development (TDD) is a software development process that relies on the repetition of a very short development cycle: first the developer writes a failing automated test case that defines a desired improvement or new function, then produces code to pass that test and finally refactors the new code to acceptable standards. Kent Beck, who is credited with having developed or 'rediscovered' the technique, stated in 2003 that TDD encourages simple designs and inspires confidence

アプリケーションのテストには、いくつかの種類がある。

### ユニットテスト

ユニットテストとはプログラミングの手法のひとつだ。
関数やクラスやメソッドが期待通りに動いていることを開発中に常に確かめる。
さまざまな関数やメソッドはの入出力の値をチェックすれば、
その内部ロジックが正しく動いていることを確認できる。
依存性注入の仕組みを利用してクラスのモックやスタブを使えば、
依存ライブラリが正しく使われていることを確かめられる。

クラスや関数を作るときに、その振る舞いを確かめるためのユニットテストも同時に作る。
最も基本的なレベルだと、間違った引数を渡した場合にエラーになることや、
正しい引数を渡したときに正常に動くことなどを確認しないといけない。
こうしておけば、後にクラスや関数に手を加えたときにも
今までの機能が期待通りに動くかどうかを確かめられるようになる。
test.php で var_dump() とかいうやり方もあるけど、
まともなアプリケーションを作るつもりならそれはあり得ない。

それ以外にもユニットテストの使い道はある。オープンソースに貢献する手段として使えるのだ。
うまく機能していないことを示すためにテストを書く。そして動くように修正する。
最後にテストが通ることを確認する。こんなパッチを送れば、きっと受け入れてもらいやすくなるだろう。
もし何かプロジェクトを運営していて pull request を受け付けているのなら、
「パッチにはテストをつけること」という条件をつけておくといいだろう。

[PHPUnit](http://phpunit.de)は、PHPアプリケーションでユニットテストを書くための
デファクトスタンダードのフレームワークだ。しかしそれ以外にも選択肢がある。

* [SimpleTest](http://simpletest.org)
* [Enhance PHP](http://www.enhance-php.com/)
* [PUnit](http://punit.smf.me.uk/)
* [atoum](https://github.com/atoum/atoum)

### インテグレーションテスト

[Wikipedia](http://en.wikipedia.org/wiki/Integration_testing) によると、

> Integration testing (sometimes called Integration and Testing, abbreviated "I&T") is the phase in software testing in which individual software modules are combined and tested as a group. It occurs after unit testing and before validation testing. Integration testing takes as its input modules that have been unit tested, groups them in larger aggregates, applies tests defined in an integration test plan to those aggregates, and delivers as its output the integrated system ready for system testing.

ユニットテスト用のツールの多くはインテグレーションテストにも使える。
ほぼ同じような指針で行うものだからである。

### 機能テスト

受け入れテストと呼ばれることもある。実際にアプリケーションを使う観点での自動テストを作ってその動きを確認する。
単にコード片が正しく動くとか、個々のパーツがお互いに正しくやりとりできるかとかいうレベルのテストではない。
このレベルのテストでは、実際のデータを使ったりアプリケーションの実際のユーザーをシミュレートしたりすることが一般的だ。

#### 機能テスト用のツール

* [Selenium](http://seleniumhq.com)
* [Mink](http://mink.behat.org)
* [Codeception](http://codeception.com) これはフルスタックのテスティングフレームワークで、受け入れテスト用のツール群も含んでいる
* [Storyplayer](http://datasift.github.io/storyplayer) これはフルスタックのテスティングフレームワークで、テスト環境をオンデマンドで作ったり破棄したりする機能も含んでいる
