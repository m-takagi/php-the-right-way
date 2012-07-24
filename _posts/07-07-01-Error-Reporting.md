---
title: エラーレポート
isChild: true
---

## エラーレポート

エラーを記録しておくと、アプリケーションに何か問題があったときにその原因を見つけやすくなる。
しかしその一方で、アプリケーションの構造に関する情報を外部に公開してしまうことにもなる。
エラーメッセージを出すことで起こる問題からアプリケーションを守るには、
開発環境と本番環境でサーバーの設定を切り替える必要がある。

### 開発環境

<strong>開発</strong>環境でエラーを表示するときには、`php.ini`で次のように設定する。

- display_errors: On
- error_reporting: E_ALL
- log_errors: On

### 本番環境

<strong>本番</strong>環境でエラーの情報を見せないようにするには、`php.ini`で次のように設定する。

- display_errors: Off
- error_reporting: E_ALL
- log_errors: On

この本番環境用の設定をしても、ウェブサーバーのエラーログにはエラーの内容がきちんと残る。
しかし、ユーザーにはエラーが見えなくなる。これらの設定項目についてもっと詳しく知りたければ、
PHP のマニュアルを読もう。

* [error_reporting](http://www.php.net/manual/ja/errorfunc.configuration.php#ini.error-reporting)
* [display_errors](http://www.php.net/manual/ja/errorfunc.configuration.php#ini.display-errors)
* [log_errors](http://www.php.net/manual/ja/errorfunc.configuration.php#ini.log-errors)
