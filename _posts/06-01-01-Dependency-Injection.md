---
title: 依存性の注入
---

# 依存性の注入 {#dependency_injection_title}

[Wikipedia](http://en.wikipedia.org/wiki/Dependency_injection)によると、

> Dependency injection is a software design pattern that allows the removal of hard-coded dependencies and makes it 
> possible to change them, whether at run-time or compile-time.

> 依存性の注入とはソフトウェアデザインパターンの1つである。依存関係をソースコードから排除して、実行時あるいはコンパイル時にその依存関係を変更できるようにする。

とのことだが、この説明は必要以上に小難しく言っているように思える。
依存性の注入とはコンポーネントに依存関係を渡せる仕組みのことで、コンストラクタで注入したりメソッド呼び出しをしたりプロパティを設定したりといった方法がある。
それだけのことだ。
