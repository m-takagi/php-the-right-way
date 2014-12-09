---
title: 日付や時刻の扱いかた
isChild: true
anchor:  date_and_time
---

## 日付や時刻の扱いかた {#date_and_time_title}

PHP の DateTime クラスを使えば、日付や時刻の読み書き、比較、そして計算ができる。
PHP には DateTime クラス以外にも日付や時刻がらみの関数が大量にあるけど、
DateTime クラスにはちゃんとしたオブジェクト指向のインターフェイスがあるので
たいていの場合はこのクラスを使ったほうがいい。
タイムゾーンだって扱えるけど、ここではそこまでは深追いしない。

DateTime を使って何かの操作をするためには、日付や時刻を表す文字列をファクトリーメソッド
`createFromFormat()` でオブジェクトに変換するか、あるいは `new DateTime`
で現在の日時を取得する。`format()` メソッドを使えば、DateTime を文字列に戻して出力できる。

{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTime::createFromFormat('d. m. Y', $raw);

echo 'Start date: ' . $start->format('Y-m-d') . "\n";
{% endhighlight %}

DateTime を使った計算をするときに使えるのが the DateInterval クラスだ。
DateTime には `add()` や `sub()` といった関数があって、その引数に指定するのがこの DateInterval となる。
1日が86400秒であることを前提としたコードを書いてはいけない。
サマータイムとかタイムゾーンの移動がからむと、この前提はあっさり崩れてしまうからだ。
そんなときには DateInterval を使う。二つの日付の差を計算するときには
`diff()` メソッドを使う。このメソッドは DateInterval を返し、結果を表示するのも簡単だ。

{% highlight php %}
<?php
// $start をコピーして、1か月と6日を足す
$end = clone $start;
$end->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Difference: ' . $diff->format('%m month, %d days (total: %a days)') . "\n";
// Difference: 1 month, 6 days (total: 37 days)
{% endhighlight %}

DateTime オブジェクトどうしでごく普通に比較することもできる。

{% highlight php %}
<?php
if ($start < $end) {
    echo "Start is before end!\n";
}
{% endhighlight %}

最後にもうひとつ DatePeriod クラスの例を示そう。繰り返し発生するイベントを順に処理するときに使える。
開始日時と終了日時を表す二つの DateTime 、そしてイベントの間隔を受け取って、すべてのイベントを返すものだ。

{% highlight php %}
<?php
// $start から $end までの間のすべての木曜日を返す
$periodInterval = DateInterval::createFromDateString('first thursday');
$periodIterator = new DatePeriod($start, $periodInterval, $end, DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // 毎木曜日を表示する
    echo $date->format('Y-m-d') . ' ';
}
{% endhighlight %}

* [DateTime][datetime]
* [日付の書式][dateformat] (日付の書式指定文字列に使えるオプション)

[datetime]: http://php.net/book.datetime
[dateformat]: http://php.net/function.date
