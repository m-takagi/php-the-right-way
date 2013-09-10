---
title: Vagrant
isChild: true
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

Vagrantは、共有フォルダを使ってホストと仮想マシンの間でコードを共有する。
つまり、ホストマシンで作ったり編集したりしたファイルをそのまま仮想マシンで実行できるっていうことだ。

### A little help

If you need a little help to start using Vagrant there are two services that might be useful:

- [Rove][rove]: service that allows you to pregenerate typical Vagrant builds, PHP among the options. The
  provisioning is made with Chef.
- [Puphpet][puphpet]: simple GUI to set up virtual machines for PHP development. **Heavily focused in PHP**. Besides
  local VMs, can be used to deploy to cloud services as well. The provisioning is made with Puppet.

[vagrant]: http://vagrantup.com/
[puppet]: http://www.puppetlabs.com/
[chef]: http://www.opscode.com/
[rove]: http://rove.io/
[puphpet]: https://puphpet.com/
