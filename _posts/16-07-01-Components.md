---
title: コンポーネント
isChild: true
anchor:  components
---

## コンポーネント {#components_title}

先ほど説明したように、「コンポーネント」っていうのは
共有するコードを作ったりそれを配布したりするための手段のひとつだ。
コンポーネントを登録するリポジトリにもいろいろあるけど、中でも有名なのがこのふたつだ。

* [Packagist]
* [PEAR]

どちらのリポジトリについてもコマンドラインのツールが存在し、
コンポーネントのインストールやアップグレードを簡単にできる。
詳細は[依存関係の管理][dm]を参照すること。

コンポーネントベースのフレームワークもあれば、フレームワークを持たずにコンポーネントだけを提供するベンダーもある。
こういったプロジェクトが提供するパッケージは、他のパッケージや特定のフレームワークへの依存がほとんどない。

たとえば[FuelPHPのValidationパッケージ][fuelval]を使うときには、
別にFuelPHPフレームワークを使う必要はない。

* [Aura]
* CakePHP Components
    * [Collection]
    * [Database]
    * [Datasource]
    * [Event]
    * [I18n]
    * [ORM]   
* [FuelPHP]
* [Hoa Project]
* [Symfony Components]
* [The League of Extraordinary Packages]
* Laravel's Illuminate Components
    * [IoC Container]
    * [Eloquent ORM]
    * [Queue]

_Laravelの [Illuminate コンポーネント] も、将来的には Laravel フレームワークからきちんと切り離されるようになるだろう。
現段階では、Laravel フレームワークからうまく切り離せているコンポーネントだけをリストにあげておいた。_


[Packagist]: /#composer_と_packagist
[PEAR]: /#pear
[Dependency Management]: /#依存関係の管理
[FuelPHP Validation package]: https://github.com/fuelphp/validation
[Aura]: http://auraphp.com/framework/
[FuelPHP]: https://github.com/fuelphp
[Hoa Project]: https://github.com/hoaproject
[Symfony Components]: https://symfony.com/doc/current/components/index.html
[The League of Extraordinary Packages]: https://thephpleague.com/
[IoC Container]: https://github.com/illuminate/container
[Eloquent ORM]: https://github.com/illuminate/database
[Queue]: https://github.com/illuminate/queue
[Illuminate コンポーネント]: https://github.com/illuminate
[Collection]: https://github.com/cakephp/collection
[Database]: https://github.com/cakephp/database
[Datasource]: https://github.com/cakephp/datasource
[Event]: https://github.com/cakephp/event
[I18n]: https://github.com/cakephp/i18n
[ORM]: https://github.com/cakephp/orm
