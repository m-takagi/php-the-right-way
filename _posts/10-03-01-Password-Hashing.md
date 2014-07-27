---
title: パスワードのハッシュ処理
isChild: true
anchor: password_hashing
---

## パスワードのハッシュ処理 {#password_hashing_title}

誰もがいつかは、ログイン機能を持つ PHP アプリケーションを書くことになる。
ユーザー名とパスワード (のハッシュ) をデータベースに保存して、
ユーザーのログイン時にそれを使って認証するというやつだ。

データベースにパスワードを保存するときは、適切に [_ハッシュ_][3] することが大切だ。
パスワードのハッシュは不可逆な操作で、ユーザーのパスワードに対して一方通行で行う。
できあがる結果は固定長の文字列で、元には戻せない。
つまり、このハッシュを別のハッシュと比較すれば元の文字列どうしが一致するかどうかは判断できるが、
元の文字列が何だったかはわからないってことだ。
パスワードをハッシュせずにデータベースに保存していると、
万一第三者に不正アクセスされた場合に、すべてのユーザーアカウントが乗っ取られてしまう。
別のサービスでも同じパスワードを使い回しているって人も、中にはいるかもしれない。
なので、セキュリティについては真剣に考える必要がある。

**`password_hash`によるパスワードのハッシュ**

PHP 5.5からは、新たに`password_hash`関数が使えるようになった。
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



* [`password_hash` について調べる] [1]
* [PHP >= 5.3.7 && < 5.5 で使える `password_compat`] [2]
* [暗号学的なハッシュについて調べる] [3]
* [PHP `password_hash` RFC] [4]

[1]: http://www.php.net/manual/ja/function.password-hash.php
[2]: https://github.com/ircmaxell/password_compat
[3]: http://ja.wikipedia.org/wiki/暗号学的ハッシュ関数
[4]: https://wiki.php.net/rfc/password_hash
