---
title: 仮想サーバーあるいは専用サーバー
isChild: true
anchor: virtual_or_dedicated_servers
---

## 仮想サーバーあるいは専用サーバー {#virtual_or_dedicated_servers_title}

サーバー管理が苦にならない人、あるいはサーバー管理を勉強してみたい人は、仮想サーバーあるいは専用サーバーを選ぶといい。
そうすれば、アプリケーションの運用環境を完全に制御できる。

### nginx と PHP-FPM

PHP に組み込まれた FastCGI Process Manager (FPM) は [nginx](http://nginx.org)
と組み合わせるのに最適だ。nginx は、軽量でパフォーマンスに優れたウェブサーバーである。
Apache よりも少ないメモリで動き、同時にさばけるリクエストの数も多い。
これは特に、共有メモリの少ない仮想サーバーでは重要だ。

* [nginx](http://nginx.org)
* [PHP-FPM](http://php.net/manual/ja/install.fpm.php)
* [nginx と PHP-FPM で安全な環境を作る](https://nealpoole.com/blog/2011/04/setting-up-php-fastcgi-and-nginx-dont-trust-the-tutorials-check-your-configuration/)

### Apache と PHP

PHP と Apache は長い付き合いだ。
Apache はいろんな設定が可能で、さまざまな [モジュール](http://httpd.apache.org/docs/2.4/mod/)
で機能を拡張できる。共有サーバーで PHP のフレームワークを動かしたり、WordPress
みたいなアプリケーションを動かしたりするときにはよく使われる選択肢だ。
残念ながら Apache は、デフォルトでは nginx よりもメモリを食うし、
同時にさばけるユーザー数も nginx より少ない。

Apache で PHP を動かすにはいくつかの選択肢がある。
一番よく使われていて簡単に設定できるのが、[prefork MPM](http://httpd.apache.org/docs/2.4/mod/prefork.html) と mod_php5 の組み合わせだ。
メモリの使用効率はそれほどよくないが、とりあえず動かして使うには一番シンプルだ。
サーバー管理方面にあまり足を突っ込みたくない場合は、この方法がいいだろう。
注意すべき点は、mod_php5 を使う場合は必ず prefork MPM を使わないといけないということだ。

Apache 本来のパフォーマンスや安定性をもっと絞り出したいという場合は、nginx と同じように FPM を使うこともできる。
この場合は、[worker MPM](http://httpd.apache.org/docs/2.4/mod/worker.html) あるいは
[event MPM](http://httpd.apache.org/docs/2.4/mod/event.html) に mod_fastcgi あるいは mod_fcgid
を組み合わせる。この設定はメモリの利用効率がよくて高速に動作するが、設定に手間がかかる。

* [Apache](http://httpd.apache.org/)
* [Multi-Processing Modules](http://httpd.apache.org/docs/2.4/mod/mpm_common.html)
* [mod_fastcgi](http://www.fastcgi.com/mod_fastcgi/docs/mod_fastcgi.html)
* [mod_fcgid](http://httpd.apache.org/mod_fcgid/)
