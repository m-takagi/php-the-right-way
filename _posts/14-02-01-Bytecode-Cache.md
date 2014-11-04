---
title: バイトコードキャッシュ
isChild: true
anchor: bytecode_cache
---

## バイトコードキャッシュ {#bytecode_cache_title}

PHP ファイルを実行するときにその裏側で行われているのは、
まずバイトコード (オペコード) にコンパイルしてからそのバイトコードを実行するという処理だ。
PHP ファイルに変更がなければ、バイトコードも同じものになる。
ということは、PHP ファイルに変更がなければコンパイル処理は CPU リソースの無駄遣いになるということだ。

そこでバイトコードキャッシュの出番だ。
これはバイトコードをメモリに格納してそれ以降の呼び出しで再利用するという仕組みで、
冗長なコンパイルを回避する。バイトコードキャッシュを設定するのはほんの数分で済み、
それだけでアプリケーションの速度が劇的に向上する。使わない理由はないね。

PHP 5.5 からは、 [OPcache][opcache-book]
というバイトコードキャッシュが標準で組み込まれるようになった。
5.5 より前のバージョンでも使うことができる。

バイトコードキャッシュについて詳しく知りたければ、以下を参照すること。

* [OPcache][opcache-book] (PHP 5.5 以降に組み込まれている)
* [APC](http://php.net/manual/ja/book.apc.php) (PHP 5.4 以前のバージョン)
* [XCache](http://xcache.lighttpd.net/)
* [Zend Optimizer+](http://www.zend.com/products/server/) (Zend Server パッケージに組み込まれている)
* [WinCache](http://www.iis.net/download/wincacheforphp) (Microsoft Windows Server 用の拡張)

[opcache-book]: http://php.net/manual/ja/book.opcache.php
