---
title: Register Globals
isChild: true
---

## Register Globals

<strong>注意:</strong> PHP 5.4 以降では `register_globals`
という設定項目はなくなったので、この設定は使えない。

`register_globals`を有効にすると、`$_POST`や`$_GET`そして`$_REQUEST`
などの内容にアプリケーションのグローバルスコープでアクセスできるようになる。
これを使うとセキュリティの問題が発生しやすくなる。
というのも、そのデータがどこからきたものなのかをアプリケーション側で判断できなくなるからだ。

PHP 4.2.0 より前のバージョンを使っている場合は、この設定によるリスクがあることを意識しておこう。
PHP 4.2.0 以降は `register_globals` のデフォルトが "off" に変わった。
アプリケーションのセキュリティを守るため、この設定は可能な限り "off" にしておこう。

* [register_globals に関する PHP マニュアルでの説明](http://www.php.net/manual/ja/security.globals.php)
