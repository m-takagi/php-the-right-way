---
title: Vagrant
isChild: true
anchor: vagrant
---

## Vagrant {#vagrant_title}

開発環境と違う環境でアプリケーションを動かすと、謎のバグが出ることがある。
あと、チームで開発しているときとかに、各自の開発環境で使ってるライブラリのバージョンをきちんとそろえておくっていうのも
けっこう大変。

Windows で開発して Linux (など、Windows 以外の環境) にデプロイするとか、
あるいはチームで開発しているとかいうのなら、
仮想マシンを導入すべきだろう。なんか難しいことを言ってるように聞こえるかもしれない。
けど、[Vagrant][vagrant]を使えばちょっとした手順でシンプルなラッパーを用意できるし、
あとは[Puppet][puppet]とか[Chef][chef]を使えば仮想環境を配布できる。
ベースとなる環境を配布できるようにしておけば、複数の開発環境をまったく同じ状態に構築できる。
複雑怪奇なコマンドを羅列した「環境構築手順書」だとかいうのもいらなくなるってこと。
ベース環境を「破棄」したり作りなおしたりするのもそんなに手間がかからないので、
まっさらな環境を用意するのもお手軽にできる。

Vagrantは、フォルダを作って、ホストと仮想マシンの間でコードを共有する。
つまり、ホストマシンで作ったり編集したりしたコードを、そのまま仮想マシンの中で実行できるっていうことだ。

### ちょっとした手助け

Vagrantを使い始めるときの手助けになるのが、これらのサービスだ。

- [Rove][rove]: PHPなどのオプション込みで、一般的なVagrantビルドを作ってくれるサービス。プロビジョニングにはChefを使う。
- [Puphpet][puphpet]: PHPの開発用の仮想マシンを作ってくれる、シンプルなGUI。
  **PHPに特化している。**
  ローカルVM以外に、クラウドサービスにデプロイすることもできる。
  プロビジョニングにはPuppetを使う。
- [Protobox][protobox]: vagrant をラップしたウェブ GUI で、ウェブ開発向けの仮想マシンを用意してくれる。
  シンプルな YAML ドキュメントを使って、仮想マシン上にインストールするすべてものを制御できる。
- [Phansible][phansible]: 使いやすいインターフェイスで、PHP プロジェクト用の Ansible Playbook を生成してくれる。

[vagrant]: http://vagrantup.com/
[puppet]: http://www.puppetlabs.com/
[chef]: http://www.opscode.com/
[rove]: http://rove.io/
[puphpet]: https://puphpet.com/
[protobox]: http://getprotobox.com/
[phansible]: http://phansible.com/
