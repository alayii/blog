---
title: "解释 Rust 的 Ownership"
date: 2018-03-19T13:06:10+08:00
---

Rust 一些基本语法和数据类型不需要讲太多，如果有编程经验，应该可以在熟悉的语言内快速找到类似的语法和数据类型。

今天主要记录下 Rust 的 Ownership 概念。

#### 什么是 Ownership

Rust 没有使用 GC 或者手动管理内存的方式来处理内存管理，所以引入了 Ownership 的概念。Ownership 其实是编译时编译器需要检查的一套规则，以此来实现内存管理，所以在运行时是不会有 ownership 这些特性的消耗的。

#### Ownership 的规则

1. Rust 里每个有变量绑定的值，这个变量就是值的 owner (所有者)。
2. 同一时间内只有有一个 owner.
3. 如果 owner 不在作用域内了，值则被丢弃。

Rust 的作用域，也是块级作用域，既是以大括号来分割作用域。当变量不在作用域内之后，比如：

1.1:

```rust
{
    let s = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}                                  // this scope is now over, and s is no
                                   // longer valid
```

在代码倒数第二行时，变量 `s` 则所申请的内存则会被自动返回给系统。


#### 变量 Move

下面两段代码表示了 Rust 如何处理多个变量与同一个数据交互的方式：

1.2:

```rust
let x = 5;
let y = x;
```

1.3:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

1.2 的代码，由于 x 的值为数字，在 rust 内这类值被放在 stack 里，当它被赋值给其他变量时，rust 会对值进行复制，由于是在 stack 里，大小是固定的，所以复制操作是比较快的。

而在 1.3 的代码中，s1 的值为 String，在 rust 里，String 是在 heap 内动态分配内存的，在赋值的时候，rust 并不会复制数据，而是如下图所示：

![string_move](/images/move_in_rust.svg)

这里可以看到，指向数据的指针，字符串长度和容量被复制了，其中指针指向 heap 内同一块数据。

这样 rust 在 s1 和 s2 在离开作用域的时候，会释放两次同一个内存区域，为了解决这个问题，当 `let s2 = s1;` 时，s1 就被废除（invalidate）了，就是说在赋值语句后在访问 s1 的话，编译器就会报错。这样在 s1 和 s2 都离开作用域后，rust 也只会对 s2 释放内存，调用 drop 函数，因为 s1 已经不存在了。这个操作在 rust 里叫做 `move`，可以看成 s1 被移动到 s2，s1 不再存在。

与 `move` 相对的，也有 Copy ，String 类型默认都是 move 的，如果需要深复制，则要调用 clone 方法，这样 heap 内的数据也会被复制一份，但可能会比较慢。

但是如 `int` `float` 这些类型，他们默认就是 copy 的，因为他们的数据在 stack 里，是固定大小，复制起来较快。

#### Ownership 和函数

如果函数调用时，将 String 变量当作参数传递后，那么这个变量的 ownership 就被 move (转移) 到另一个函数内了，之后再使用此变量，编译器也会报错。除非函数将参数再返回，调用者重新接收返回值，这样 ownership 才被转移回来，这样乍看起来十分麻烦，有种被语言限制的感觉，为了解决这类问题，rust 有 Borrowing 和 References 两个概念来解决问题。

这就是 rust 没用常规的方式来解决内存管理，需要引用其他多个概念来解决其引起的问题。下次再讲 Borrowing 与 References，更复杂的应该是 Lifetime 的概念和写法了。不知了解了之后是否真的可以很好的应用。

###### ps: 代码和图片都来自 [rust book](https://doc.rust-lang.org/book/second-edition/ch04-01-what-is-ownership.html)。
