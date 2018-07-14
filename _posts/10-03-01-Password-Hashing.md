---
title: パスワードのハッシュ処理
isChild: true
anchor:  password_hashing
---

## パスワードのハッシュ処理 {#password_hashing_title}

誰もがいつかは、ログイン機能を持つ PHP アプリケーションを書くことになる。
ユーザー名とパスワード (のハッシュ) をデータベースに保存して、
ユーザーのログイン時にそれを使って認証するというやつだ。

データベースにパスワードを保存するときは、適切に [_ハッシュ_][3] することが大切だ。
ハッシュと暗号化は [まったく違うもの][7] なのに、混同されることが多い。

パスワードのハッシュは不可逆な操作で、ユーザーのパスワードに対して一方通行で行う。
できあがる結果は固定長の文字列で、元には戻せない。
つまり、このハッシュを別のハッシュと比較すれば元の文字列どうしが一致するかどうかは判断できるが、
元の文字列が何だったかはわからないってことだ。
パスワードをハッシュせずにデータベースに保存していると、
万一第三者に不正アクセスされた場合に、すべてのユーザーアカウントが乗っ取られてしまう。

ハッシュと違って暗号化は元に戻せる (鍵さえ手元にあればね)。
暗号化そのものは使う場面を選べば便利なものだけど、ことパスワードの保存に関してはうまい手段ではない。

パスワードには個別に [_ソルト_][5] が必要だ。ランダムな文字列をパスワードに付けてからハッシュするってこと。
そうしておけば、辞書攻撃から守れるし「レインボーテーブル(ありがちなパスワードとそのハッシュをまとめた変換テーブル)」による攻撃も防げる。

ハッシュとソルトは欠かせない。だって、たいていのユーザーはいろんなサービスでパスワードを使い回すものだし、
パスワード自体も決して強力なものだとは言えないから。

あと、速度が売りの汎用ハッシュ関数 (SHA256とか) じゃなくて [_パスワードのハッシュ_ に特化したアルゴリズム][6] を使うこと。
2018年6月時点でパスワードのハッシュに使ってもかまわないアルゴリズムはこんな感じ。

* Argon2 (PHP 7.2 以降で使える)
* Scrypt
* **Bcrypt** (PHP が用意してくれている。詳細は後ほど)
* PBKDF2 と HMAC-SHA256 あるいは HMAC-SHA512 の組み合わせ

ありがたいことに、最近の PHP ならこのあたりも使いやすい。

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


[1]: https://secure.php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: http://ja.wikipedia.org/wiki/暗号学的ハッシュ関数
[4]: https://wiki.php.net/rfc/password_hash
[5]: https://wikipedia.org/wiki/Salt_(cryptography)
[6]: https://paragonie.com/blog/2016/02/how-safely-store-password-in-2016
[7]: https://paragonie.com/blog/2015/08/you-wouldnt-base64-a-password-cryptography-decoded

