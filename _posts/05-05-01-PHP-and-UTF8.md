---
title: UTF-8の扱い
isChild: true
anchor:  php_and_utf8
---

## UTF-8の扱い {#php_and_utf8_title}

_このセクションは、もともと[Alex Cabal](https://alexcabal.com/)が
[PHP Best Practices](https://phpbestpractices.org/#utf-8)向けに書いたものだ。この記事をもとに、UTF-8について説明する。_

### 一発で済ませる方法はない。注意して、きちんと一貫性を保つこと。

今のところPHPは、低レベルではUnicodeをサポートしていない。
PHPでUTF-8文字列をきちんと処理する方法もあるにはあるが、簡単ではない。さらに、ウェブアプリケーションのあらゆるレベル
（つまり、HTMLやSQLからPHPまで）に手を入れる必要がある。
ここでは、それらについて、現実的な範囲で手短にまとめた。

### PHPレベルでのUTF-8

文字列の連結や変数への代入などの基本操作については、UTF-8だからといって何か特別なことをする必要はない。
しかし、大半の文字列関数（`strpos()` や `strlen()` など）については、そういうわけにはいかない。
これらの関数には、対応する関数として `mb_*` が用意されていることが多い。
たとえば `mb_strpos()` や `mb_strlen()` だ。
これらの `mb_*` 関数は [マルチバイト文字列拡張モジュール] が提供するもので、
Unicode文字列を扱えるように設計されている。

Unicode文字列を扱う場合は、常に `mb_*` 関数を使う必要がある。
たとえば、UTF-8文字列に対して `substr()` を使うと、その結果の中に文字化けした半角文字が含まれてしまう可能性がある。
この場合、マルチバイト文字列のときに使うべき関数は `mb_substr()` だ。

常に `mb_*` 関数を使うように覚えておくのが大変なところだ。
たとえ一か所でもそれを忘れてしまうと、それ以降の Unicode 文字列は壊れてしまう可能性がある。

そして、すべての文字列関数に `mb_*` 版があるわけではない。
自分が使いたい関数にマルチバイト版がないだって？
ご愁傷様。

すべてのPHPスクリプトの先頭（あるいは、グローバルにインクルードするファイルの先頭）で `mb_internal_encoding()`
関数を使わないといけないし、スクリプトの中でブラウザに出力するつもりなら、それだけではなく
`mb_http_output()` 関数も使わなければいけない。
すべてのスクリプトでエンコーディングを明示しておけば、後で悩まされることもなくなるだろう。

さらに、文字列を操作する関数の多くには、文字エンコーディングを指定するためのオプション引数が用意されている。
このオプションがある場合は、常に UTF-8 を明示しておくべきだ。
たとえば `htmlentities()` には文字エンコーディングを設定する引数があるので、
UTF-8 文字列を扱うなら常にそう指定しておかないといけない。
PHP 5.4.0 以降では、 `htmlentities()` や `htmlspecialchars()` のデフォルトエンコーディングが UTF-8 に変わった。

最後に、他の人たち向けに配布するつもりのアプリケーションなど、その実行環境で `mbstring` が使えるかどうか定かではない場合は、
Composer の [patchwork/utf8] パッケージを使うことも検討しよう。
これは、もし `mbstring` があればそれを使い、なければ非 UTF-8 関数にフォールバックするというものだ。

[マルチバイト文字列拡張モジュール]: https://secure.php.net/book.mbstring
[patchwork/utf8]: https://packagist.org/packages/patchwork/utf8

### データベースレベルでのUTF-8

PHP スクリプトから MySQL に接続する場合は、上で書いた注意をすべて守ったにもかかわらず UTF-8
文字列がそれ以外のエンコーディングで格納されてしまう可能性がある。

PHP から MySQL に渡す文字列を確実に UTF-8 として扱わせるには、データベースとテーブルの文字セットや照合順序の設定を、すべて
`utf8mb4` にしておく必要がある。さらに、PDO の接続文字列にも、文字セットとして `utf8mb4` を指定する。
詳細は以下のコードを参照すること。 _これ、試験に出るよ。_

UTF-8 を完全にサポートするには、文字セット `utf8mb4` を使わないといけない。 `utf8` はダメ！！！
その理由が知りたければ「あわせて読みたい」を参照すること。

### ブラウザレベルでのUTF-8

`mb_http_output()` 関数を使えば、PHP スクリプトからブラウザへの出力が UTF-8 文字列になることを保証できる。

あとは、HTTPレスポンスの中で、そのページをUTF-8として扱うようブラウザに指示する必要がある。
いまどきなら、普通はHTTPレスポンスヘッダの中でこんなふうに設定するだろう。

{% highlight php %}
<?php
header('Content-Type: text/html; charset=UTF-8')
{% endhighlight %}

昔は、ページの `<head>` タグの中で [charset の `<meta>` タグ](http://htmlpurifier.org/docs/enduser-utf8.html) を指定したりしていたものだ。

{% highlight php %}
<?php
// PHP に対して、今後このスクリプトの中では UTF-8 文字列を使うことを伝える
mb_internal_encoding('UTF-8');
$utf_set = ini_set('default_charset', 'utf-8');
if (!$utf_set) {
    throw new Exception('could not set default_charset to utf-8, please ensure it\'s set on your system!');
}

// PHP に対して、ブラウザに UTF-8 で出力することを伝える
mb_http_output('UTF-8');

// UTF-8 のテスト用文字列
$string = 'Êl síla erin lû e-govaned vîn.';

// 何らかのマルチバイト関数で文字列を操作する。
// ここでは、デモの意味も込めて、非ASCII文字のところで文字列をカットしてみた。
$string = mb_substr($string, 0, 15);

// データベースに接続し、この文字列を格納する。
// このドキュメントにある PDO のサンプルを見れば、より詳しい情報がわかる。
// ここでの肝は、データソース名 (DSN) における `charset=utf8mb4` だ。
$link = new PDO(
    'mysql:host=your-hostname;dbname=your-db;charset=utf8mb4',
    'your-username',
    'your-password',
    array(
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_PERSISTENT => false
    )
);

// 変換した文字列を、UTF-8としてデータベースに格納する。
// DBとテーブルの文字セットや照合順序が、ちゃんとutf8mb4になっているかな？
$handle = $link->prepare('insert into ElvishSentences (Id, Body, Priority) values (default, :body, :priority)');
$handle->bindParam(':body', $string, PDO::PARAM_STR);
$priority = 45;
$handle->bindParam(':priority', $priority, PDO::PARAM_INT); // intを求めていることをpdoに対して明示する
$handle->execute();

// 今格納したばかりの文字列を取り出して、きちんと格納できているかどうかを確かめる
$handle = $link->prepare('select * from ElvishSentences where Id = :id');
$id = 7;
$handle->bindParam(':id', $id, PDO::PARAM_INT);
$handle->execute();

// 結果をオブジェクトに代入して、後でHTMLの中で使う
// このオブジェクトがメモリを圧迫することはない。必要になったその時点ではじめてフェッチする
$result = $handle->fetchAll(\PDO::FETCH_OBJ);

// ラッパーのサンプル。これはデータをhtml出力用にエスケープする
function escape_to_html($dirty){
    echo htmlspecialchars($dirty, ENT_QUOTES, 'UTF-8');
}

header('Content-Type: text/html; charset=UTF-8'); // すでに default_charset が utf-8 になっているのであればこれは不要
?><!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>UTF-8 テストページ</title>
    </head>
    <body>
        <?php
        foreach($result as $row){
            escape_to_html($row->Body);  // 変換したUTF-8文字列が、ブラウザに正しく出力されるはず
        }
        ?>
    </body>
</html>
{% endhighlight %}

### あわせて読みたい

* [PHPマニュアル: 文字列演算子](https://secure.php.net/language.operators.string)
* [PHPマニュアル: String関数](https://secure.php.net/ref.strings)
    * [`strpos()`](https://secure.php.net/function.strpos)
    * [`strlen()`](https://secure.php.net/function.strlen)
    * [`substr()`](https://secure.php.net/function.substr)
* [PHPマニュアル: マルチバイト文字列関数](https://secure.php.net/ref.mbstring)
    * [`mb_strpos()`](https://secure.php.net/function.mb-strpos)
    * [`mb_strlen()`](https://secure.php.net/function.mb-strlen)
    * [`mb_substr()`](https://secure.php.net/function.mb-substr)
    * [`mb_internal_encoding()`](https://secure.php.net/function.mb-internal-encoding)
    * [`mb_http_output()`](https://secure.php.net/function.mb-http-output)
    * [`htmlentities()`](https://secure.php.net/function.htmlentities)
    * [`htmlspecialchars()`](https://secure.php.net/function.htmlspecialchars)
* [Stack Overflow: What factors make PHP Unicode-incompatible?](https://stackoverflow.com/questions/571694/what-factors-make-php-unicode-incompatible)
* [Stack Overflow: Best practices in PHP and MySQL with international strings](https://stackoverflow.com/questions/140728/best-practices-in-php-and-mysql-with-international-strings)
* [How to support full Unicode in MySQL databases](https://mathiasbynens.be/notes/mysql-utf8mb4)
* [Bringing Unicode to PHP with Portable UTF-8](https://www.sitepoint.com/bringing-unicode-to-php-with-portable-utf8/)
* [Stack Overflow: DOMDocument loadHTML does not encode UTF-8 correctly](https://stackoverflow.com/questions/8218230/php-domdocument-loadhtml-not-encoding-utf-8-correctly)
