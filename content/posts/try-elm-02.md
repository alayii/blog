---
title: "Try Elm 02"
date: 2018-01-20T00:06:39+08:00
---

粗略的看完 Elm 的[官方指南](https://guide.elm-lang.org)后，发现这个语言跟 Haskell 有许多相似甚至相同的地方，其中有意思的则是 [Type Signature(类型特征)](https://en.wikipedia.org/wiki/Type_signature)，从写法上来看，Elm 与 Haskell 的完全一致。

#### 如何表示类型

`elm-repl` 会输出这样的内容：

```elm
> "hello"
"hello" : String

> not True
False : Bool
```

`"hello": String` 这种就是 Type Signature，与 Java 或者 C 语言不同，它们的类型是写在后面而不是前面，并且是多了个冒号，且不与值本身写在一起。

Elm 与 JavaScript 在 List (也就是 JavaScript 的 Array) 上有些不同，JS 的 array 可以存放任何类型的值，而 Elm 的 List 只能存放同一种类型的值，这样 List 才有方法表示自己的类型，比如：

```elm
> [ "Alice", "Bob" ]
[ "Alice", "Bob" ] : List String

> []
[] : List a
```

`[] : List a` 里的 `a` 表示一个类型变量（[type variable](https://en.wikipedia.org/wiki/Type_variable)），即 `a` 这个类型是不确定的，但是只要在 type signature 中保持一致即可，这些都是 Haskell 里基本的一些概念，所以了解 Haskell 的人对这些东西不会太陌生。

同样，函数的 type signature 与 Haskell 也是相同的写法

```elm
> import String
> String.length
<function> : String -> Int
```

以 `->` 来表示返回值，如果有多个参数，则写成：

```elm
> divide x y = x / y
<function> : Float -> Float -> Float
```

这样写乍看起来很难理解，因为函数显然接受了两个参数，返回一个值，但是从类型声明上看，就像只接受了一个 Float，这样的写法与 Partial Application 相关，表示了在 Elm 里（或者 Haskell）里，所有的函数都是只接受一个参数的，接受多个参数的函数只是一种返回另一个函数的函数，另一个函数再返回了最后的结果，所以上面的类型声明可以写成：

```elm
> divide x y = x / y
<function> : Float -> (Float -> Float)
```

即在类型声明里，是右结合的。

由于所有函数都是接受一个参数，返回另一个函数（或值），这样可以很容易的链式调用函数，把之前的结果传递到下一个函数内。

#### Type Alias

与 Haskell 相同，Elm 也具有 Type Alias 这种特性，顾名思义，Type alias 既是给某种类型赋予一个别名，比如：

```elm
type alias User =
  { name : String
  , bio : String
  , pic : String
  }
```

这些就把 { name: String, bio: String, pic: String } 这一长串的类型标记用 User 来表示了，其实 Type Alias 最大的好处是，可以抽象出自己的类型，提供相应的方法来操作此类型，而不是直接使用内置类型，如果某天这个类型的变量 get 方式修改了，那么我们需要修改大量业务代码来兼容最新方法，比如说 JS 的对象属性获取从 `.` 操作符改成了 `->`，那么需要改动的代码不计其数。

当然，这种语言层面的情况不可能发生，但是第三方库很可能会出现接口的 breaking change, 所以抽象出自己的类型，即使底层实现的改变，也能很轻易地修改，而不影响上层的使用。只需要改变类型的实现既可，而不需要改变对外的接口使用。

#### Union Type

看过[官方指南](https://guide.elm-lang.org)，感觉最 Powerful 的特性应该是 [Union Type](https://en.wikipedia.org/wiki/Union_type) 了。

如果写过一些 Web 应用，应该会发现经常要表示许多复杂的数据结构。这时候我们都会使用对象来表示，但是里面的属性只能用一些基本类型。而 Union Type 可以更自然地表示复杂的数据结构。比如

```elm
type User = Anonymous | Named String
```

表示了有姓名的 User 和没有姓名的 User，但他们都是 User 类型。当然同时只会是其中一个值，如果要对这个类型的值做操作的时候，则一定要穷尽这个类型里所有可能的值。

```elm
userPhoto : User -> String
userPhoto user =
  case user of
    Anonymous ->
      "anon.png"

    Named name ->
      "users/" ++ name ++ ".png"
```

如果没有穷尽其中所有可能的值，编译器则会报错，这样的话基本消除了在运行时出现不可预知的值导致的错误，可以说是一个非常大的进步了（这里还需要使用其他[错误处理](https://guide.elm-lang.org/error_handling/)的方式）。

而且类型里是可以递归使用这个类型本身的，这样可以更简单的表示树，链表等有递归特性的数据结构，更甚至，可以用这套类型系统实现一个程序语言（所以 Haskell 通常用来实现其他语言），这个则是用 Elm 实现的布尔表达式的[示例](http://elm-lang.org/examples/boolean-expressions)。
