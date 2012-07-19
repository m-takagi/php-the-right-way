---
isChild: true
---

## 日付や時刻の扱いかた

日付や時刻を扱うのはとても簡単で、PHP には DateTime クラスっていうのが用意されている。
文字列から DateTime への変換には、ファクトリーメソッド createFromFormat を使えばいい。
このメソッドにはオブジェクト指向スタイルじゃない関数もあって、それが date_create_from_format() だ。

{% highlight php %}
<?php
$rawDate = '22/11/1968';
$date = \DateTime::createFromFormat('d/m/Y', $rawDate);

{% endhighlight %}

DateTime を使った計算をするときに使えるのが the DateInterval クラスだ。
DateTime には add() や sub() といった関数があって、その引数に指定するのがこの DateInterval となる。
たとえば、さっき作った日付の一ヶ月後を計算したければ、このようにする。

{% highlight php %}
$date->add(new \DateInterval('P1M')); // 一ヶ月という期間を加える
{% endhighlight %}

DateTime クラスには、日付の書式を指定する関数もある。

{% highlight php %}
echo $date->format('d/m/Y h:i:s');
{% endhighlight %}

最後に示す例は、Unix タイムスタンプを DateTime に変換してからもう一度 Unix タイムスタンプに戻すものだ。

{% highlight php %}
$unixtime = '1239363000';
$date = DateTime::createFromFormat('U', $unixtime); // date は 2009-04-10 11:30:00 となる
echo $date->format('U'); // 1239363000 と出力する
{% endhighlight %}

* [DateTime][datetime]
* [DateInterval][dateinterval]
* [日付の書式][dateformat]

[datetime]: http://php.net/manual/ja/language.exceptions.php 
[dateinterval]: http://www.php.net/manual/ja/class.dateinterval.php
[dateformat]: http://www.php.net/manual/ja/function.date.php
