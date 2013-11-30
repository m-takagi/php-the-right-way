---
title: 振る舞い駆動開発
isChild: true
---

## 振る舞い駆動開発 {#behavior_driven_development_title}

振る舞い駆動開発 (BDD) には二種類ある。SpecBDD と StoryBDD だ。
SpecBDD はコードの技術的な振る舞いを重視し、StoryBDD は業務的あるいは機能的な振る舞いを重視する。
PHP には、これら二種類の BDD 用のフレームワークが存在する。

StoryBDD では、人間が読める形式のストーリーを書いてアプリケーションの振る舞いを表す。
そしてそのストーリーを、アプリケーションのテストとして実行する。
PHP アプリケーションで StoryBDD をするために使えるフレームワークが
Behat で、これは Ruby の [Cucumber](http://cukes.info/) の影響を受けたフレームワークである。
Gherkin DSL を使ってフィーチャを記述できる。

SpecBDD では、実際のコードのあるべき振る舞いをスペックとして書く。
関数やメソッドを単独でテストするのではなく、その関数やメソッドがどのように振る舞うのかを記述するのだ。
PHP で SpecBDD をするときに使えるフレームワークが PHPSpec で、これは
Ruby の [RSpec project](http://rspec.info/) の影響を受けている。

### BDD に関するリンク

* [Behat](http://behat.org/) は PHP 用の StoryBDD フレームワークで、Ruby の [Cucumber](http://cukes.info/) プロジェクトの影響を受けている。
* [PHPSpec](http://www.phpspec.net/) は PHP 用の SpecBDD フレームワークで、Ruby の [RSpec](http://rspec.info/) プロジェクトの影響を受けている。
* [Codeception](http://www.codeception.com) はフルスタックのテストフレームワークで、BDD の原則に従っている。
