---
title: アプリケーションのビルドとデプロイ
isChild: true
anchor:  building_and_deploying_your_application
---

## アプリケーションのビルドとデプロイ {#building_and_deploying_your_application_title}

まさか、データベースのスキーマを変更したりテストを実行したりとかいったことを手作業でやってるなんてことはないよね？
ちょっと待った！新しいバージョンのアプリケーションをデプロイするときに手作業がひとつでも増えると、
致命的な間違いを犯してしまう可能性もそのぶん増えてしまうんだ。たとえ単純な更新作業だとしても、
きちんとしたビルド手順にしたがうこと。継続的インテグレーションの戦略にしたがって、
[ビルドの自動化][buildautomation] をしておくといい。

自動化できるタスクには、こんなものがある。

* 依存関係の管理
* 必要な資産のコンパイル
* テストの実行
* ドキュメントの生成
* パッケージング
* デプロイ


### ビルド自動化ツール

ビルドツールとは、ソフトウェアのデプロイにからむありがちな作業を処理するスクリプトをまとめたものだと言える。
ビルドツール自体は君が作るソフトウェアの一部ではない。ソフトウェアを「外部から」支援するものだ。

ビルド自動化の助けとなるオープンソースのツールがたくさん公開されている。PHPで書かれているものもあれば、
そうじゃないものもある。PHP製じゃないからといって、それを使わない理由はない。
もし自分のやりたいことに適したツールがあるのなら、使うべきだ。いくつか例をあげよう。

[Phing] は、PHPな人がデプロイを自動化しようとするときに、いちばん取っつきやすいツールだ。
Phingを使えば、パッケージングやデプロイそしてテストといった処理をシンプルなXMLビルドファイルで設定できる。
Phingは[Apache Ant] をベースに作られたもので、
Webアプリのインストールやアップデートに必要となるタスク群を提供する。
カスタムタスクで機能を追加することもでき、カスタムタスクはPHPで書ける。

[Capistrano] は
*中級から上級のプログラマー* 向けのシステムだ。構造化された、繰り返し可能な形式で、
複数のリモートマシン上でコマンドを実行できる。
Ruby on Railsのアプリをデプロイするように設定されているが、
これを使って **PHP のアプリをデプロイすることもできる**。
Capistranoを使いこなすには、RubyとRakeに関するそれなりの知識が必要だ。

Dave Gardnerのblog記事[PHP Deployment with Capistrano][phpdeploy_capistrano]
は、Capistranoに興味のあるPHP開発者への入門記事としておすすめだ。

[Chef] は単なるデプロイフレームワークではない。
Rubyで書かれた強力なシステムインテグレーションフレームワークで、
単にアプリをデプロイするだけじゃなくサーバー環境全体を構築したり
仮想環境を構築したりもできる。

[Deployer] はPHPで書かれたデプロイツールで、シンプルかつ機能的だ。
タスクを並列に実行し、アトミックなデプロイを行い、サーバー間の整合性を維持する。
SymfonyやLaravel、Zend Framework、そしてYiiなどで使える、一般的なタスクのレシピが用意されている。

#### PHP開発者向けのChefの資料

* [LAMPアプリケーションのデプロイにChefやVagrantそしてEC2を使うというお題で書かれた全3回のシリーズ][chef_vagrant_and_ec2]
* [Chefのクックブック。PHPのインストールと設定やPEARについて扱っている][Chef_cookbook]
* [Chefのビデオチュートリアルシリーズ][Chef_tutorial]

#### あわせて読みたい:

* [Apache Antによるプロジェクトの自動化][apache_ant_tutorial]

### 継続的インテグレーション

> 継続的インテグレーションはソフトウェア開発のプラクティスのひとつで、
> チームのメンバーが自分たちの作業を頻繁に統合するというものだ。
> 通常は、各自が少なくとも一日に一度は統合する。一日に何度も統合することもある。
> 多くのチームが、この方針のおかげで統合時の問題が少なくなるし、
> きちんとしたソフトウェアをより素早く開発できるようになると実感している。

*-- マーティン・ファウラー*

PHPで継続的インテグレーションを実践する方法はいろいろある。
[Travis CI] のおかげで、
ちょっとしたプロジェクトにも簡単に継続的インテグレーションを組み込めるようになった。
Travis CIは継続的インテグレーションのホスティング環境で、オープンソースコミュニティに開放されている。
GitHubと統合されており、PHPを含むさまざまな言語に対応している。

#### あわせて読みたい

* [Jenkinsによる継続的インテグレーション][Jenkins]
* [PHPCIによる継続的インテグレーション][PHPCI]
* [Teamcityによる継続的インテグレーション][Teamcity]


[buildautomation]: http://ja.wikipedia.org/wiki/ビルド_(ソフトウェア)
[Phing]: http://www.phing.info/
[Apache Ant]: http://ant.apache.org/
[Capistrano]: https://github.com/capistrano/capistrano/wiki
[phpdeploy_capistrano]: http://www.davegardner.me.uk/blog/2012/02/13/php-deployment-with-capistrano/
[Chef]: https://www.chef.io/
[chef_vagrant_and_ec2]: http://www.jasongrimes.org/2012/06/managing-lamp-environments-with-chef-vagrant-and-ec2-1-of-3/
[Chef_cookbook]: https://github.com/chef-cookbooks/php
[Chef_tutorial]: https://www.youtube.com/playlist?list=PL11cZfNdwNyPnZA9D1MbVqldGuOWqbumZ
[apache_ant_tutorial]: http://net.tutsplus.com/tutorials/other/automate-your-projects-with-apache-ant/
[Travis CI]: https://travis-ci.org/
[Jenkins]: http://jenkins-ci.org/
[PHPCI]: http://www.phptesting.org/
[Teamcity]: http://www.jetbrains.com/teamcity/
[Deployer]: https://github.com/deployphp/deployer
