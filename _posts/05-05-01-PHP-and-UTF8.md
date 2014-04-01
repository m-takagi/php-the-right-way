---
title: PHPとUTF-8
isChild: true
anchor: php_and_utf8
---

## PHPとUTF-8 {#php_and_utf8_title}

_このセクションは、もともと[Alex Cabal](https://alexcabal.com/)が
[PHP Best Practices](https://phpbestpractices.org/#utf-8)向けに書いたものだ。ここでも共有する。_

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
これらの関数をひとまとめにして、マルチバイト文字列関数と呼ぶ。
マルチバイト文字列関数は、Unicode文字列を扱えるように設計されている。

Unicode文字列を扱う場合は、常に `mb_*` 関数を使う必要がある。
たとえば、UTF-8文字列に対して `substr()` を使うと、その結果の中に文字化けした半角文字が含まれてしまう可能性がある。
この場合、マルチバイト文字列のときに使うべき関数は `mb_substr()` だ。

常に `mb_*` 関数を使うように覚えておくのが大変なところだ。
たとえ一か所でもそれを忘れてしまうと、それ以降の Unicode 文字列は壊れてしまう可能性がある。

そして、すべての文字列関数に `mb_*` 版があるわけではない。
自分が使いたい関数にマルチバイト版がないだって？
ご愁傷様。

さらに、すべてのPUPスクリプトの先頭（あるいは、グローバルにインクルードするファイルの先頭）で `mb_internal_encoding()`
関数を使わないといけないし、スクリプトの中でブラウザに出力するつもりなら、それだけではなく
`mb_http_output()` 関数も使わなければいけない。
すべてのスクリプトでエンコーディングを明示しておけば、後で悩まされることもなくなるだろう。

最後に、文字列を操作する関数の多くには、文字エンコーディングを指定するためのオプション引数が用意されている。
このオプションがある場合は、常に UTF-8 を明示しておくべきだ。
たとえば `htmlentities()` には文字エンコーディングを設定する引数があるので、
UTF-8 文字列を扱うなら常にそう指定しておかないといけない。

PHP 5.4.0 以降では、 `htmlentities()` や `htmlspecialchars()` のデフォルトエンコーディングが UTF-8 に変わった。

### データベースレベルでのUTF-8

If your PHP script accesses MySQL, there's a chance your strings could be stored as non-UTF-8 strings in the database 
even if you follow all of the precautions above.

To make sure your strings go from PHP to MySQL as UTF-8, make sure your database and tables are all set to the 
`utf8mb4` character set and collation, and that you use the `utf8mb4` character set in the PDO connection string. See 
example code below. This is _critically important_.

Note that you must use the `utf8mb4` character set for complete UTF-8 support, not the `utf8` character set! See 
Further Reading for why.

### ブラウザレベルでのUTF-8

Use the `mb_http_output()` function to ensure that your PHP script outputs UTF-8 strings to your browser. In your HTML, 
include the [charset `<meta>` tag](http://htmlpurifier.org/docs/enduser-utf8.html) in your page's `<head>` tag. 

{% highlight php %}
<?php
// Tell PHP that we're using UTF-8 strings until the end of the script
mb_internal_encoding('UTF-8');
 
// Tell PHP that we'll be outputting UTF-8 to the browser
mb_http_output('UTF-8');
 
// Our UTF-8 test string
$string = 'Êl síla erin lû e-govaned vîn.';
 
// Transform the string in some way with a multibyte function
// Note how we cut the string at a non-Ascii character for demonstration purposes
$string = mb_substr($string, 0, 15);
 
// Connect to a database to store the transformed string
// See the PDO example in this document for more information
// Note the `set names utf8mb4` commmand!
$link = new \PDO(   
                    'mysql:host=your-hostname;dbname=your-db;charset=utf8mb4',
                    'your-username',
                    'your-password',
                    array(
                        \PDO::ATTR_ERRMODE => \PDO::ERRMODE_EXCEPTION,
                        \PDO::ATTR_PERSISTENT => false
                    )
                );
 
// Store our transformed string as UTF-8 in our database
// Your DB and tables are in the utf8mb4 character set and collation, right?
$handle = $link->prepare('insert into ElvishSentences (Id, Body) values (?, ?)');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->bindValue(2, $string);
$handle->execute();
 
// Retrieve the string we just stored to prove it was stored correctly
$handle = $link->prepare('select * from ElvishSentences where Id = ?');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->execute();
 
// Store the result into an object that we'll output later in our HTML
$result = $handle->fetchAll(\PDO::FETCH_OBJ);
?><!doctype html>
<html>
    <head>
        <meta charset="UTF-8" />
        <title>UTF-8 test page</title>
    </head>
    <body>
        <?php
        foreach($result as $row){
            print($row->Body);  // This should correctly output our transformed UTF-8 string to the browser
        }
        ?>
    </body>
</html>
{% endhighlight %}

### あわせて読みたい

* [PHP Manual: String Operations](http://php.net/manual/en/language.operators.string.php)
* [PHP Manual: String Functions](http://php.net/manual/en/ref.strings.php)
    * [`strpos()`](http://php.net/manual/en/function.strpos.php)
    * [`strlen()`](http://php.net/manual/en/function.strlen.php)
    * [`substr()`](http://php.net/manual/en/function.substr.php)
* [PHP Manual: Multibyte String Functions](http://php.net/manual/en/ref.mbstring.php)
    * [`mb_strpos()`](http://php.net/manual/en/function.mb-strpos.php)
    * [`mb_strlen()`](http://php.net/manual/en/function.mb-strlen.php)
    * [`mb_substr()`](http://php.net/manual/en/function.mb-substr.php)
    * [`mb_internal_encoding()`](http://php.net/manual/en/function.mb-internal-encoding.php)
    * [`mb_http_output()`](http://php.net/manual/en/function.mb-http-output.php)
    * [`htmlentities()`](http://php.net/manual/en/function.htmlentities.php)
    * [`htmlspecialchars()`](http://www.php.net/manual/en/function.htmlspecialchars.php)
* [PHP UTF-8 Cheatsheet](http://blog.loftdigital.com/blog/php-utf-8-cheatsheet)
* [Stack Overflow: What factors make PHP Unicode-incompatible?](http://stackoverflow.com/questions/571694/what-factors-make-php-unicode-incompatible)
* [Stack Overflow: Best practices in PHP and MySQL with international strings](http://stackoverflow.com/questions/140728/best-practices-in-php-and-mysql-with-international-strings)
* [How to support full Unicode in MySQL databases](http://mathiasbynens.be/notes/mysql-utf8mb4)
* [Brining Unicode to PHP with Portable UTF-8](http://www.sitepoint.com/bringing-unicode-to-php-with-portable-utf8/)
