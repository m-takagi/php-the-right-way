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
It's a solid and robust tool and has been around for a long time, however the tool could be perceived as a bit old fashioned because of the way it deals with configuration (XML files).

[Capistrano] は
*中級から上級のプログラマー* 向けのシステムだ。構造化された、繰り返し可能な形式で、
複数のリモートマシン上でコマンドを実行できる。
Ruby on Railsのアプリをデプロイするように設定されているが、
PHP のアプリもデプロイできる。
Capistranoを使いこなすには、RubyとRakeに関するそれなりの知識が必要だ。
Dave Gardnerのblog記事[PHP Deployment with Capistrano][phpdeploy_capistrano]
は、Capistranoに興味のあるPHP開発者への入門記事としておすすめだ。

[Rocketeer] gets its inspiration and philosophy from the Laravel framework. Its goal is to be fast, elegant and ease to use with smart defaults. It features multiple servers, multiple stages, atomic deploys and deployment can be performed in parallel. Everything in the tool can be hot swapped or extended, and everything is written in PHP.

[Deployer] はPHPで書かれたデプロイツールで、シンプルかつ機能的だ。
タスクを並列に実行し、アトミックなデプロイを行い、サーバー間の整合性を維持する。
SymfonyやLaravel、Zend Framework、そしてYiiなどで使える、一般的なタスクのレシピが用意されている。
Younes Rafie's article  [Easy Deployment of PHP Applications with Deployer][phpdeploy_deployer] is a great tutorial for deploying your application with the tool.

[Magallanes] another tool written in PHP with simple configuration done in YAML files. It has support for multiple servers and environments, atomic deployment, and have some built in tasks that you can leverage for common tools and frameworks.

#### あわせて読みたい:

* [Apache Antによるプロジェクトの自動化][apache_ant_tutorial]
* [Expert PHP Deployments][expert_php_deployments] - free book on deployment with Capistrano, Phing and Vagrant.
* [Deploying PHP Applications][deploying_php_applications] - paid book on best practices and tools for PHP deployment.

### サーバープロビジョニング

Managing and configuring servers can be a daunting task when faced with many servers. There are tools for dealing with this so you can automate your infrastructure to make sure you have the right servers and that they're configured properly. They often integrate with the larger cloud hosting providers (Amazon Web Services, Heroku, DigitalOcean, etc) for managing instances, which makes scaling an application a lot easier.

[Ansible] is a tool that manages your infrastructure through YAML files. It's simple to get started with and can manage complex and large scale applications. There is an API for managing cloud instances and it can manage them through a dynamic inventory using certain tools.

[Puppet] is a tool that has its own language and file types for managing servers and configurations. It can be used in a master/client setup or it can be used in a "master-less" mode. In the master/client mode the clients will poll the central master(s) for new configuration on set intervals and update itself if necessary. In the master-less mode you can push changes to your nodes. 

[Chef] is a powerful Ruby based system integration framework that you can build your whole server environment or virtual boxes with. It integrates well with Amazon Web Services through their service called OpsWorks.

#### あわせて読みたい:

* [An Ansible Tutorial][an_ansible_tutorial]
* [Ansible for DevOps][ansible_for_devops] - paid book on everything Ansible
* [Ansible for AWS][ansible_for_aws] - paid book on integrating Ansible and Amazon Web Services
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