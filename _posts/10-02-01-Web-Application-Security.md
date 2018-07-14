---
title: ウェブアプリケーションのセキュリティ
isChild: true
anchor:  web_application_security
---

## ウェブアプリケーションのセキュリティ {#web_application_security_title}

PHP開発者は全員、[Webアプリケーションセキュリティの基本][4] を学ぶべきだ。内容としてはだいたいこんな感じになる。
down into a handful of broad topics:

1. コードとデータの分離
   * データをコードとして実行するときにはSQLインジェクションやクロスサイトスクリプティングやファイルインクルード攻撃の恐れがある
   * コードをデータとして表示するときには情報漏洩の恐れがある (ソースコードが晒されてしまったり、C言語の場合ならアドレス空間配置のランダム化もある [ASLR][5])
2. アプリケーションロジック
   * 認証・認可の制御不全
   * 入力の検証
3. 運用環境
   * PHPのバージョン
   * サードパーティのライブラリ
   * OS
4. 暗号化の弱点
   * [弱い乱数][6].
   * [選択暗号文攻撃][7].
   * [サイドチャネル情報漏洩][8].

世の中には悪い人たちがいて、あなたの書いたウェブアプリケーションもきっと狙われている。
必要な対策をして、ウェブアプリケーションのセキュリティを固めておくことが大切だ。
ありがたいことに、[The Open Web Application Security Project][1] (OWASP)
の人たちが、既知のセキュリティ問題とその対策をまとめてくれている。
セキュリティが気になる開発者は必読だ。
Padraic Bradyが書いた[Survive The Deep End: PHP Security][3] も、
ウェブアプリケーションセキュリティに関してPHP向けに書かれたよいドキュメントだ。

* [OWASP Security Guideを読む][2]


[1]: https://www.owasp.org/
[2]: https://www.owasp.org/index.php/Guide_Table_of_Contents
[3]: https://phpsecurity.readthedocs.io/en/latest/index.html
[4]: https://paragonie.com/blog/2015/08/gentle-introduction-application-security
[5]: http://searchsecurity.techtarget.com/definition/address-space-layout-randomization-ASLR
[6]: https://paragonie.com/blog/2016/01/on-design-and-implementation-stealth-backdoor-for-web-applications
[7]: https://paragonie.com/blog/2015/05/using-encryption-and-authentication-correctly
[8]: http://blog.ircmaxell.com/2014/11/its-all-about-time.html

