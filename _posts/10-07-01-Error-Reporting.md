---
title: エラーレポート
isChild: true
anchor:  error_reporting
---

## エラーレポート {#error_reporting_title}

エラーを記録しておくと、アプリケーションに何か問題があったときにその原因を見つけやすくなる。
しかしその一方で、アプリケーションの構造に関する情報を外部に公開してしまうことにもなる。
エラーメッセージを出すことで起こる問題からアプリケーションを守るには、
開発環境と本番環境でサーバーの設定を切り替える必要がある。

### 開発環境

**開発** 環境で、起こりうるエラーをすべて表示するときには、`php.ini`で次のように設定する。

{% highlight ini %}
display_errors = On
display_startup_errors = On
error_reporting = -1
log_errors = On
{% endhighlight %}

> 値に`-1`を指定すると、仮に将来のバージョンのPHPで新しいレベルと定数が追加されたとしてもすべてのエラーを表示するようになります。E_ALL 定数も、PHP 5.4以降これと同じ挙動になります。 - [php.net](https://secure.php.net/function.error-reporting)

`E_STRICT`エラーレベル定数は5.3.0で導入されたもので、当時は
`E_ALL`には含まれていなかった。でも5.4.0からは`E_ALL`に含まれるようになった。
だからどうなんだって？
あらゆるエラーを表示させたいときには、5.3の場合は
`-1`あるいは`E_ALL | E_STRICT`を使わないといけないってことだ。

**PHPのバージョン別の、すべてのエラーを表示させるための設定**

* &lt; 5.3 `-1` or `E_ALL`
* &nbsp; 5.3 `-1` or `E_ALL | E_STRICT`
* &gt; 5.3 `-1` or `E_ALL`

### 本番環境

**本番** 環境でエラーの情報を見せないようにするには、`php.ini`で次のように設定する。

{% highlight ini %}
display_errors = Off
display_startup_errors = Off
error_reporting = E_ALL
log_errors = On
{% endhighlight %}

この本番環境用の設定をしても、ウェブサーバーのエラーログにはエラーの内容がきちんと残る。
しかし、ユーザーにはエラーが見えなくなる。これらの設定項目についてもっと詳しく知りたければ、
PHP のマニュアルを読もう。

* [error_reporting](https://secure.php.net/errorfunc.configuration#ini.error-reporting)
* [display_errors](https://secure.php.net/errorfunc.configuration#ini.display-errors)
* [display_startup_errors](https://secure.php.net/errorfunc.configuration#ini.display-startup-errors)
* [log_errors](https://secure.php.net/errorfunc.configuration#ini.log-errors)
