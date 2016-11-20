---
title: パスワードのハッシュ処理
isChild: true
anchor:  password_hashing
---

## パスワードのハッシュ処理 {#password_hashing_title}

誰もがいつかは、ログイン機能を持つ PHP アプリケーションを書くことになる。
ユーザー名とパスワード (のハッシュ) をデータベースに保存して、
ユーザーのログイン時にそれを使って認証するというやつだ。

<<<<<<< HEAD
データベースにパスワードを保存するときは、適切に [_ハッシュ_][3] することが大切だ。
パスワードのハッシュは不可逆な操作で、ユーザーのパスワードに対して一方通行で行う。
できあがる結果は固定長の文字列で、元には戻せない。
つまり、このハッシュを別のハッシュと比較すれば元の文字列どうしが一致するかどうかは判断できるが、
元の文字列が何だったかはわからないってことだ。
パスワードをハッシュせずにデータベースに保存していると、
万一第三者に不正アクセスされた場合に、すべてのユーザーアカウントが乗っ取られてしまう。

Passwords should also be individually [_salted_][5] by adding a random string to each password before hashing. This prevents dictionary attacks and the use of "rainbow tables" (a reverse list of crytographic hashes for common passwords.)

Hashing and salting are vital as often users use the same password for multiple services and password quality can be poor. 

Fortunately, nowadays PHP makes this easy. 

**`password_hash`によるパスワードのハッシュ**

PHP 5.5からは、新たに`password_hash()`関数が使えるようになった。
現時点では、この関数はBCryptを使っている。これは、現在のPHPがサポートしているアルゴリズムの中では最強のものだ。
必要に応じて、将来はもっと強力なアルゴリズムをサポートするように更新されるだろう。
この関数をPHP 5.5より前のバージョンでも使えるようにするため、`password_compat`ライブラリも作られた。
このライブラリはPHP 5.3.7以降で使える。

この例では、文字列をハッシュした後でそのハッシュを新たな文字列と比較している。
二つの文字列は違っている('secret-password'と'bad-password')ので、このログインは失敗する。

{% highlight php %}
<?php
require 'password.php';

$passwordHash = password_hash('secret-password', PASSWORD_DEFAULT);

if (password_verify('bad-password', $passwordHash)) {
    // パスワードが一致した
} else {
    // パスワードが一致しなかった
}
{% endhighlight %}  

`password_hash()` takes care of password salting for you. The salt is stored, along with the algorithm and "cost", as part of the hash.  `password_verify()` extracts this to determine how to check the password, so you don't need a separate database field to store your salts. 

* [`password_hash` について調べる] [1]
* [PHP >= 5.3.7 && < 5.5 で使える `password_compat`] [2]
* [暗号学的なハッシュについて調べる] [3]
* [ソルトについて調べる] [5]
* [PHP `password_hash()` RFC] [4]


[1]: http://php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: http://ja.wikipedia.org/wiki/暗号学的ハッシュ関数
[4]: https://wiki.php.net/rfc/password_hash
[5]: https://en.wikipedia.org/wiki/Salt_(cryptography)
