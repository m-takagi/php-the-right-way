---
isChild: true
anchor: plain_php_templates
---

## プレーンなPHPによるテンプレート {#plain_php_templates_title}

プレーンなPHPによるテンプレートとは、単にPHPのコードを使ったテンプレートという意味だ。
ごく自然な選択肢だとも言える。そもそもPHP自体がテンプレート言語だし。
あ、これって単に、PHPのコードをHTMLとかにも埋め込めるよねっていう以上の深い意味はないからね。
PHPの開発者にとっては、新しい構文を覚えずに済むというメリットがある。
どんな関数が使えるのかもわかっているし、ふだんPHPを書いているエディタのシンタックスハイライトや
自動補完機能も、そのまま使える。
その上、プレーンなPHPのテンプレートは高速であることが多い。コンパイルが不要だからだ。

いまどきのPHPフレームワークは、たいてい何らかの仕組みのテンプレートシステムを持っている。
その多くでデフォルトになっているのが、プレーンPHPによるテンプレートだ。
フレームワーク以外では、[Plates](http://platesphp.com/)や[Aura.View](https://github.com/auraphp/Aura.View)
といったライブラリがプレーンPHPテンプレートを使いやすくしてくれる。継承やレイアウト、拡張などの便利なテンプレート機能を用意してくれるんだ。

プレーンPHPテンプレートは、たとえばこのようになる（[Plates](http://platesphp.com/) ライブラリを使った）。

{% highlight php %}
<?php $this->insert('header', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->escape($name)?></p>

<?php $this->insert('footer') ?>
{% endhighlight %}