---
title: コンポーネント
isChild: true
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

コンポーネントベースのフレームワークっていうのもあって、
そのコンポーネントだけをごくわずかな要件のもとで使える。
たとえば[FuelPHPのValidationパッケージ][fuelval]を使うときには、
別にFuelPHPフレームワークを使う必要はない。
再利用可能なコンポーネントとして、基本的に独立したリポジトリで管理されているんだ。

  [dm]: /#依存関係の管理
  [fuelval]: https://github.com/fuelphp/validation

* [Aura](http://auraphp.github.com/)
* [FuelPHP (2.0 限定)](https://github.com/fuelphp)
* [Symfony Components](http://symfony.com/doc/current/components/index.html)