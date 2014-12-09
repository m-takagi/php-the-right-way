---
title: オペコードキャッシュ
isChild: true
anchor:  opcode_cache
---

## オペコードキャッシュ {#opcode_cache_title}

PHP ファイルを実行するときにその裏側で行われているのは、
まずオペコードにコンパイルしてからそのオペコードを実行するという処理だ。
PHP ファイルに変更がなければ、オペコードも同じものになる。
ということは、PHP ファイルに変更がなければコンパイル処理は CPU リソースの無駄遣いになるということだ。

そこでオペコードキャッシュの出番だ。
これはオペコードをメモリに格納してそれ以降の呼び出しで再利用するという仕組みで、
冗長なコンパイルを回避する。オペコードキャッシュを設定するのはほんの数分で済み、
それだけでアプリケーションの速度が劇的に向上する。使わない理由はないね。

PHP 5.5 からは、 [OPcache][opcache-book]
というオペコードキャッシュが標準で組み込まれるようになった。
5.5 より前のバージョンでも使うことができる。

オペコードキャッシュについて詳しく知りたければ、以下を参照すること。

* [OPcache][opcache-book] (PHP 5.5 以降に組み込まれている)
* [APC] (PHP 5.4 以前のバージョン)
* [XCache]
* [Zend Optimizer+] (Zend Server パッケージに組み込まれている)
* [WinCache] (Microsoft Windows Server 用の拡張)
* [Wikipediaにおける、PHPアクセラレータの一覧][PHP_accelerators]


[opcache-book]: http://php.net/book.opcache
[APC]: http://php.net/book.apc
[XCache]: http://xcache.lighttpd.net/
[Zend Optimizer+]: http://www.zend.com/products/server/
[WinCache]: http://www.iis.net/download/wincacheforphp
[PHP_accelerators]: http://en.wikipedia.org/wiki/List_of_PHP_accelerators
