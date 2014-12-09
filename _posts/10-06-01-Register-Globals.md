---
title: Register Globals
isChild: true
anchor:  register_globals
---

## Register Globals {#register_globals_title}

**注意:**
PHP 5.4.0 からは `register_globals`
という設定項目がなくなったので、この設定は使えない。
このページは単に、大昔のアプリケーションをアップグレードしている人たち向けの警告として用意したものでしかない。

`register_globals`を有効にすると、`$_POST`や`$_GET`そして`$_REQUEST`
などの内容にアプリケーションのグローバルスコープでアクセスできるようになる。
これを使うとセキュリティの問題が発生しやすくなる。
というのも、そのデータがどこからきたものなのかをアプリケーション側で判断できなくなるからだ。

たとえば `$_GET['foo']` の内容に `$foo` でアクセスできることになるのだが、
これは、宣言されていない変数の中身を自動的に上書きしてしまうことにつながる。
PHP 5.4.0 より前のバージョンを使っている場合は、
__確実に__ `register_globals` を __off__ にしておこう。

* [register_globals に関する PHP マニュアルでの説明](http://php.net/security.globals)
