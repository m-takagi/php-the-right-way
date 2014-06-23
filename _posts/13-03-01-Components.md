---
title: コンポーネント
isChild: true
anchor: components
---

## コンポーネント {#components_title}

先ほど説明したように、「コンポーネント」っていうのは
共有するコードを作ったりそれを配布したりするための手段のひとつだ。
コンポーネントを登録するリポジトリにもいろいろあるけど、中でも有名なのがこのふたつだ。

* [Packagist](/#composer_と_packagist)
* [PEAR](/#pear)

どちらのリポジトリについてもコマンドラインのツールが存在し、
コンポーネントのインストールやアップグレードを簡単にできる。
詳細は[依存関係の管理][dm]を参照すること。

コンポーネントベースのフレームワークもあれば、フレームワークを持たずにコンポーネントだけを提供するベンダーもある。
こういったプロジェクトが提供するパッケージは、他のパッケージや特定のフレームワークへの依存がほとんどない。

たとえば[FuelPHPのValidationパッケージ][fuelval]を使うときには、
別にFuelPHPフレームワークを使う必要はない。

  [dm]: /#依存関係の管理
  [fuelval]: https://github.com/fuelphp/validation

* [Aura](http://auraphp.github.com/)
* [FuelPHP](https://github.com/fuelphp)
* [Hoa Project](https://github.com/hoaproject)
* [Orno](https://github.com/orno)
* [Symfony Components](http://symfony.com/doc/current/components/index.html)
* [The League of Extraordinary Packages](http://thephpleague.com/)
* LaravelのIlluminateコンポーネント
    * [Eloquent ORM](https://github.com/illuminate/database)
    * [Queue](https://github.com/illuminate/queue)

_Laravelの [Illuminate コンポーネント](https://github.com/illuminate) も、将来的には Laravel フレームワークからきちんと切り離されるようになるだろう。
現段階では、Laravel フレームワークからうまく切り離せているコンポーネントだけをリストにあげておいた。 _
