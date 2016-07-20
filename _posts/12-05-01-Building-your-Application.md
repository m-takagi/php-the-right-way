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


### デプロイツール

デプロイツールとは、ソフトウェアのデプロイにからむありがちな作業を処理するスクリプトをまとめたものだと言える。
デプロイツール自体は君が作るソフトウェアの一部ではない。ソフトウェアを「外部から」支援するものだ。

ビルド自動化やデプロイの助けとなるオープンソースのツールがたくさん公開されている。PHPで書かれているものもあれば、
そうじゃないものもある。PHP製じゃないからといって、それを使わない理由はない。
もし自分のやりたいことに適したツールがあるのなら、使うべきだ。いくつか例をあげよう。

[Phing] を使えば、パッケージングやデプロイそしてテストといった処理をシンプルなXMLビルドファイルで設定できる。
Phingは[Apache Ant] をベースに作られたもので、
Webアプリのインストールやアップデートに必要となるタスク群を提供する。
カスタムタスクで機能を追加することもでき、カスタムタスクはPHPで書ける。
古くからあるツールだけあって堅実で安定してるけど、ちょっと古臭さも感じる
(設定をXMLファイルで管理するところとかね)。

[Capistrano] は
*中級から上級のプログラマー* 向けのシステムだ。構造化された、繰り返し可能な形式で、
複数のリモートマシン上でコマンドを実行できる。
Ruby on Railsのアプリをデプロイするように設定されているが、
PHP のアプリもデプロイできる。
Capistranoを使いこなすには、RubyとRakeに関するそれなりの知識が必要だ。
Dave Gardnerのblog記事[PHP Deployment with Capistrano][phpdeploy_capistrano]
は、Capistranoに興味のあるPHP開発者への入門記事としておすすめだ。

[Rocketeer] は、Laravelフレームワークの思想にヒントを得たものだ。
高速かつエレガントであり、よく考えられたデフォルト設定で簡単に使えることを目標としている。
複数サーバーや複数ステージ、アトミックなデプロイに対応していて、複数のデプロイを並列に実行できる。
すべてのパーツは動かしたままでの交換や拡張に対応しているし、ぜんぶPHPで書かれている。

[Deployer] はPHPで書かれたデプロイツールで、シンプルかつ機能的だ。
タスクを並列に実行し、アトミックなデプロイを行い、サーバー間の整合性を維持する。
SymfonyやLaravel、Zend Framework、そしてYiiなどで使える、一般的なタスクのレシピが用意されている。
Younes Rafieの記事[Easy Deployment of PHP Applications with Deployer][phpdeploy_deployer]
は、Deployerを使ってアプリケーションをデプロイするためのよいチュートリアルになっている。

[Magallanes] もPHPで書かれたツールで、YAMLでのシンプルな設定ができる。
複数サーバーや複数環境、アトミックなデプロイに対応していて、
一般的なツールやフレームワークで使える組み込みのタスクが用意されている。

#### あわせて読みたい:

* [Apache Antによるプロジェクトの自動化][apache_ant_tutorial]
* [Expert PHP Deployments][expert_php_deployments] - CapistranoやPhing、Vagrantによるデプロイを扱ったフリーの書籍
* [Deploying PHP Applications][deploying_php_applications] - PHPのデプロイに関するベストプラクティスやツールを扱った書籍

### サーバープロビジョニング

サーバーの構成管理は、大量のサーバーを扱うようになると特に大変なタスクだ。
いろんなツールが用意されているので、こういったインフラの構築を自動化できる。
ツールを使えば、適切なサーバーが適切な構成になっていることを確実にできる。
これらのツールは大規模なクラウドホスティングプロバイダー
(Amazon Web Services, Heroku, DigitalOceanなど)
にもインスタンスの管理用に統合されていることが多くて、
より簡単にアプリケーションをスケールできるようになっている。

[Ansible] は、YAMLファイルでインフラを管理するツールだ。
気軽に使い始められるし、複雑で大規模なアプリケーションにも使える。
クラウドのインスタンスを管理するためのAPIも用意されていて、
対応したツールを使えば動的インベントリを通じてインスタンスを管理できる。

[Puppet] は、独自の言語やファイルタイプを使ってサーバーや構成を管理する。
マスター／クライアント形式で使うこともできるし、「マスターレス」モードで使うこともできる。
マスター／クライアントモードの場合は、所定のインターバルでクライアントが中央サーバーをポーリングして、
新しい構成が見つかったら自分自身を更新する。
マスターレスモードでは、変更内容を各ノードにプッシュする。

[Chef] はRubyで作られた強力なシステムインテグレーションフレームワークで、
サーバー環境や仮想マシンをまるごと構築できる。
Amazon Web Servicesとも統合されていて、OpsWorksというサービスを通じて利用する。

#### あわせて読みたい:

* [Ansibleチュートリアル][an_ansible_tutorial]
* [Ansible for DevOps][ansible_for_devops] - Ansibleのすべてを扱った書籍
* [Ansible for AWS][ansible_for_aws] - AnsibleとAmazon Web Servicesとの統合について扱った書籍
* [LAMPアプリケーションのデプロイにChefやVagrantそしてEC2を使うというお題で書かれた全3回のシリーズ][chef_vagrant_and_ec2]
* [Chefのクックブック。PHPのインストールと設定やPEARについて扱っている][Chef_cookbook]
* [Chefのビデオチュートリアルシリーズ][Chef_tutorial]

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
[phpdeploy_deployer]: http://www.sitepoint.com/deploying-php-applications-with-deployer/
[Chef]: https://www.chef.io/
[chef_vagrant_and_ec2]: http://www.jasongrimes.org/2012/06/managing-lamp-environments-with-chef-vagrant-and-ec2-1-of-3/
[Chef_cookbook]: https://github.com/chef-cookbooks/php
[Chef_tutorial]: https://www.youtube.com/playlist?list=PL11cZfNdwNyPnZA9D1MbVqldGuOWqbumZ
[apache_ant_tutorial]: http://net.tutsplus.com/tutorials/other/automate-your-projects-with-apache-ant/
[Travis CI]: https://travis-ci.org/
[Jenkins]: http://jenkins-ci.org/
[PHPCI]: http://www.phptesting.org/
[Teamcity]: http://www.jetbrains.com/teamcity/
[Deployer]: http://deployer.org/
[Rocketeer]: http://rocketeer.autopergamene.eu/
[Magallanes]: http://magephp.com/
[expert_php_deployments]: http://viccherubini.com/assets/Expert-PHP-Deployments.pdf
[deploying_php_applications]: http://www.deployingphpapplications.com
[Ansible]: https://www.ansible.com/
[Puppet]: https://puppet.com/
[ansible_for_devops]: https://leanpub.com/ansible-for-devops
[ansible_for_aws]: https://leanpub.com/ansible-for-aws
[an_ansible_tutorial]: https://serversforhackers.com/an-ansible-tutorial