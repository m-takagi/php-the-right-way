---
title: データのフィルタリング
isChild: true
anchor:  data_filtering
---

## データのフィルタリング {#data_filtering_title}

PHP のコードに外部から渡される入力は、絶対に信用してはいけない。
外部からの入力は、常に検証してから使うようにしよう。
`filter_var()` 関数や `filter_input()` 関数で、入力の検証や書式の判定 (メールアドレスなど)
ができる。

外部からの入力にはいろいろな種類がある。フォームから渡される `$_GET` や `$_POST` もあれば、
スーパーグローバル `$_SERVER` に含まれるものもある。そして
`fopen('php://input', 'r')` でやってくる HTTP リクエストのボディもそうだ。
外部からの入力といっても、ユーザーがフォームで入力したものばかりとは限らないことに注意。
アップロードしたりダウンロードしたりしたファイル、セッションのデータ、
クッキーのデータ、サードパーティのウェブサービスからのデータ。
これらはみんな外部からの入力となる。

外部からのデータをいったん保存して、何かと組み合わせて後で使うとしよう。
それでも、そのデータが外部からの入力であるという事実は変わらない。
そのデータを処理したり出力したり何かとつなげたりコードに組み込んだりするときには
「適切にフィルタリングできてる？」「信頼できる？」と確認しよう。

データのフィルタリング方法は、その利用目的によって異なる。
たとえば、外部の入力を何も処理せずに HTML ページに出力すると、
あなたのサイト上で任意の JavaScript が実行できてしまうことになる！
これが、いわゆるクロスサイトスクリプティング (XSS) である。
とても危険な攻撃だ。こんなときに XSS を回避する方法のひとつは、
`strip_tags()` で入力からすべての HTML タグを取り除くか、
あるいは `htmlentities()` や `htmlspecialchars()` でエスケープして HTML エンティティに変換することだ。

別の例として、外部の入力をコマンドラインのオプションとして渡すことを考えよう。
これって非常に危険なことだし、ふつうはあまりやるべきではないことだ。
でも、もしやるなら、組み込みの関数 `escapeshellarg()`
を使えばコマンドの引数を実行されてしまうことが防げる。

最後の例は、外部の入力に基づいてファイルシステム上のファイルを読み込むというものだ。
このときは、ファイル名のかわりにファイルパスを渡されてしまうという攻撃が考えられる。
外部の入力から"/"や"../"、[null バイト][6]などを取り除いて、
隠しファイルや公開すべきでない場所のファイルを読み込まないようにする必要がある。

* [データのフィルタリング][1]
* [`filter_var` 関数][4]
* [`filter_input` 関数][5]
* [null バイトの扱い][6]

### サニタイズ

サニタイズとは、外部の入力から危険な文字を取り除く (あるいはエスケープする) ことだ。

たとえば、外部の入力を HTML に含めたり SQL クエリに組み込んだりする前に、
サニタイズが必要となる。[PDO](#databases) でバインド変数を使う場合は、
PDO が入力をサニタイズする。

外部の入力を HTML として組み込むときに、いくつかの安全な HTML
タグはそのまま使わせたいという場合もある。そんな要求を実現するのは非常に難しいので、
たいていの場合は Markdown や BBCode などのフォーマットを代替手段として使うのだが、
どうしてもとうい場合は [HTML Purifier][html-purifier]
のようなホワイトリストライブラリを使える。

[サニタイズフィルター][2]

### Unserialization

It is dangerous to `unserialize()` data from users or other untrusted sources.  Doing so can allow malicious users to instantiate objects (with user-defined properties) whose destructors will be executed, **even if the objects themselves aren't used**.  You should therefore avoid unserializing untrusted data.

If you absolutely must unserialize data from untrusted sources, use PHP 7's [`allowed_classes`][unserialize] option to restrict which object types are allowed to be unserialized.

### バリデーション

バリデーションとは、外部の入力が期待通りであるかどうかを確かめること。
たとえばユーザー登録の処理では、
メールアドレスや電話番号、あるいは年齢などを検証することになるだろう。

[バリデーションフィルター][3]


[1]: http://php.net/book.filter
[2]: http://php.net/filter.filters.sanitize
[3]: http://php.net/filter.filters.validate
[4]: http://php.net/function.filter-var
[5]: http://php.net/function.filter-input
[6]: http://php.net/security.filesystem.nullbytes
[html-purifier]: http://htmlpurifier.org/
[unserialize]: https://secure.php.net/manual/en/function.unserialize.php
