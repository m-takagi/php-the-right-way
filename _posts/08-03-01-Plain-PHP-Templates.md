---
isChild: true
title: プレーンなPHPによるテンプレート
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
フレームワーク以外では、[Plates][plates]や[Aura.View][aura]
といったライブラリがプレーンPHPテンプレートを使いやすくしてくれる。継承やレイアウト、拡張などの便利なテンプレート機能を用意してくれるんだ。

### プレーンPHPテンプレートのシンプルな例

[Plates][plates] ライブラリを使った。

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->insert('header', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->escape($name)?></p>

<?php $this->insert('footer') ?>
{% endhighlight %}

### プレーンPHPテンプレートで継承を使う例

[Plates][plates] ライブラリを使った。

{% highlight php %}
<?php // template.php ?>

<html>
<head>
    <title><?=$title?></title>
</head>
<body>

<main>
    <?=$this->section('content')?>
</main>

</body>
</html>
{% endhighlight %}

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->layout('template', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->escape($name)?></p>
{% endhighlight %}


[plates]: http://platesphp.com/
[aura]: https://github.com/auraphp/Aura.View
