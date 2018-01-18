---
title: "Try Elm"
date: 2018-01-18T23:23:30+08:00
---

[Elm](https://elm-lang.org) 是一门编译到 JavaScript 的函数式语言，官方文档表示语言没有运行时错误，作为可以在 Web 上运行的语言，没有运行时错误大大减少了 debug 的难度，经常写 JavaScript 的话应该对运行时错误很头疼。


#### 安装

在 macOS 上可以通过 `brew install elm` 来安装 elm 语言，这个包括了语言本身和 `elm-repl`, `elm-reactor`, `elm-make`, `elm-package` 四个命令行工具。其他平台的安装可以参考官方文档 [https://guide.elm-lang.org/install.html](https://guide.elm-lang.org/install.html)

#### 第一次使用

刚开始使用会用到 `elm-repl` 在命令行内体验语言本身的一些基本特性。不过刚开始使用就会发现一些问题，比如输入

```
> "hello"
```

并回车后，命令行并没有打印出任何东西，预期应该是类似 python 的 repl 一样输出结果的。

这里 Google 了一下无果，以为是 termial emulator 的原因，切换到 macOS 自带的 Terminal 了，它的窗口上会显示当前运行的程序，果然在回车后，显示成了 `elm make`，这表示 Elm 在编译这个表达式（确实也太慢了），更神奇的事情在于在第一次编译表达式的时候居然访问了 [http://package.elm-lang.org/all-packages?elm-package-version=0.18](http://package.elm-lang.org/all-packages?elm-package-version=0.18) 这个地址，然后由于天朝网络问题，访问超时了，只能翻墙了事。

![超时图片](/images/timeout.png)

作为一个函数式语言，并且使用了 Haskell 来实现，其语法也与 Haskell 有点相似，比如函数的调用不用括号来包含参数，而是使用空格

```elm
String.length "hello"
```

这种选择与 [Curring](https://en.wikipedia.org/wiki/Currying) 和 [Partial Application](https://en.wikipedia.org/wiki/Partial_application) 的概念相关，算是可以更好的表示出这两种语义的语法了，并不是一个奇怪的选择。

并且需要注意的是。Elm 有所谓的 syntactically significant whitespace，即缩进也是语法的一部分，这个跟 Python 类似，所以在赋值语句等情况下，空格是必须的。关于语言的一些基本特性最好是查看[官方文档](http://elm-lang.org/docs)，使用中也可以查阅 [Core Libraries 文档](http://package.elm-lang.org/packages/elm-lang/core/latest/)。