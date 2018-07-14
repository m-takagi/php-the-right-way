---
title: 複雑な問題
isChild: true
anchor:  complex_problem
---

## 複雑な問題 {#complex_problem_title}

これまでに依存性の注入について調べたことがある人なら、
*"制御の反転（IoC：Inversion of Control）"* や *"依存関係逆転の原則（DIP：Dependency Inversion Principle）"*
といった言葉に見覚えがあるだろう。これらは複雑な問題で、依存性の注入によって解決できるものだ。

### 制御の反転

制御の反転とは、文字通り、システムの「制御を反転」することだ。システム全体の制御を、オブジェクトから切り離したままで行う。
依存性の注入の文脈では、依存関係の制御や作成を、システム内のどこか別の場所でやるってことを意味する。

PHPのフレームワークでも、制御の反転は実現されてきた。でも、実際のところ、何の制御を反転しているのだろう。そして、反転した結果、制御はどこに行ってしまったのだろう。
たとえば、たいていのMVCフレームワークには、あらゆるコントローラの親になる基底コントローラが用意されている。
そして、それを継承しなければ依存関係のアクセスできない。
これはこれで制御の反転だが、でも、依存関係を緩くしたというよりは、単純に依存関係を別の場所に移しただけのことだ。

依存性の注入を使えば、これをもっとすっきりと解決できる。依存関係が必要になったときに、必要なものだけを注入すればいい。
ハードコーディングする必要はない。

### S.O.L.I.D.

#### Single Responsibility Principle

The Single Responsibility Principle is about actors and high-level architecture. It states that “A class should have
only one reason to change.” This means that every class should _only_ have responsibility over a single part of the
functionality provided by the software. The largest benefit of this approach is that it enables improved code
_reusability_. By designing our class to do just one thing, we can use (or re-use) it in any other program without
changing it.

#### Open/Closed Principle

The Open/Closed Principle is about class design and feature extensions. It states that “Software entities (classes,
modules, functions, etc.) should be open for extension, but closed for modification.” This means that we should design
our modules, classes and functions in a way that when a new functionality is needed, we should not modify our existing
code but rather write new code that will be used by existing code. Practically speaking, this means that we should write
classes that implement and adhere to _interfaces_, then type-hint against those interfaces instead of specific classes.

The largest benefit of this approach is that we can very easily extend our code with support for something new without
having to modify existing code, meaning that we can reduce QA time, and the risk for negative impact to the application
is substantially reduced. We can deploy new code, faster, and with more confidence.

#### Liskov Substitution Principle

The Liskov Substitution Principle is about subtyping and inheritance. It states that “Child classes should never break
the parent class’ type definitions.” Or, in Robert C. Martin’s words, “Subtypes must be substitutable for their base
types.”

For example, if we have a `FileInterface` interface which defines an `embed()` method, and we have `Audio` and `Video`
classes which both implement the `embed()` method, then we can expect that the usage of the `embed()` method will always
do the thing that we intend. If we later create a `PDF` class or a `Gist` class which implement the `FileInterface`
interface, we will already know and understand what the `embed()` method will do. The largest benefit of this approach
is that we have the ability to build flexible and easily-configurable programs, because when we change one object of a
type (e.g., `FileInterface`) to another we don't need to change anything else in our program.

#### Interface Segregation Principle

The Interface Segregation Principle (ISP) is about _business-logic-to-clients_ communication. It states that “No client
should be forced to depend on methods it does not use.” This means that instead of having a single monolithic interface
that all conforming classes need to implement, we should instead provide a set of smaller, concept-specific interfaces
that a conforming class implements one or more of.

For example, a `Car` or `Bus` class would be interested in a `steeringWheel()` method, but a `Motorcycle` or `Tricycle`
class would not. Conversely, a `Motorcycle` or `Tricycle` class would be interested in a `handlebars()` method, but a
`Car` or `Bus` class would not. There is no need to have all of these types of vehicles implement support for both
`steeringWheel()` as well as `handlebars()`, so we should break-apart the source interface.

#### 依存関係逆転の原則

The Dependency Inversion Principle is about removing hard-links between discrete classes so that new functionality can
be leveraged by passing a different class.
これは *「抽象に依存しろ。具象に依存するな」* という原則だ。
簡単に言うと、依存関係はインターフェイスや抽象クラスに対して設定すべきものであり、それを実装したクラスに対して設定してはいけないってこと。
先ほどの例をこの原則に沿って書き直すと、こんなふうになる。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
{% endhighlight %}

これで `Database` クラスは、具象クラスではなくインターフェイスに依存するように変わった。で、いったい何がうれしいんだろう。

こんな場面を考えてみよう。君は今、とあるチームの一員として作業をしている。アダプターを作っているのは別のメンバーだ。
最初の例だと、その人がアダプターを完成させるまでは、自分のコードのユニットテストのモックを作れない。
インターフェイスに依存するようにした新しいバージョンだと、そのインターフェイスを使ってモックを作ることができる。
同僚が作るアダプターもそのインターフェイスに沿っているとわかっているからだ。

そんなことより、もっとすばらしいメリットもある。こっちの方式のほうが、コードがずっとスケーラブルになるんだ。
仮に将来、別のデータベースに移行することになったとしよう。
そんな場合でも、このインターフェイスを実装した新しいアダプターを書いたらそれでおしまいだ。
それ以外は何もいじる必要がない。
新しいアダプターがきちんと決まりごとに従っているということを、インターフェイスが保証してくれるからだ。
