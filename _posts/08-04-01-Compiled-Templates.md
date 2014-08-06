---
isChild: true
anchor: compiled_templates
---

## コンパイル形式のテンプレート {#compiled_templates}

PHPはオブジェクト指向言語として成熟してきてはいるものの、テンプレート言語としては
[いまいち](http://fabien.potencier.org/article/34/templating-engines-in-php) だ。
コンパイル形式のテンプレート、たとえば [Twig](http://twig.sensiolabs.org/) や [Smarty](http://www.smarty.net/)
が、この穴を埋めてくれる。テンプレートに特化した、新しい構文を用意してくれるんだ。
自動エスケープから継承や制御構文まで、コンパイル形式のテンプレートは、いかに読み書きしやすく、安心して使えるかを重視して作られている。
さらに、コンパイル形式のテンプレートは、別の言語でさえも使うことができる。[Mustache](http://mustache.github.io/) がそのよい例だ。
テンプレートをコンパイルする時間がかかるので、多少はパフォーマンスに影響する。
しかし、適切にキャッシュをすれば、その影響は微々たるものだ。

コンパイル形式のテンプレートは、たとえばこのようになる（[Twig](http://twig.sensiolabs.org/) ライブラリを使った）。

{% highlight text %}
{% raw %}
{% include 'header.html' with {'title': 'User Profile'} %}

<h1>User Profile</h1>
<p>Hello, {{ name }}</p>

{% include 'footer.html' %}
{% endraw %}
{% endhighlight %}