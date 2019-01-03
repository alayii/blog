---
title: Basic Types in C
date: 2019-01-02T14:57:30.438Z
draft: false
---
### Integer Types

* short int
* unsigned short int
* int
* unsigned int
* long int
* unsigned login int
* long long int (C99)
* unsigned long long int (C99)

各类型的大小：

![integer size in 16 bit machine](/images/uploads/int_16bit.png "integer size in 16 bit machine")

![integer size in 32 bit machine](/images/uploads/int_32bit.png "integer size in 32 bit machine")

![integer size in 64 bit machine](/images/uploads/int_64bit.png "integer size in 64 bit machine")

8 进制使用 `0` 开头，16 进制使用 `0x` 或者 `0X` 开头，如

```
017 024 0xff 0XFF
```

#### Overflow

如果值超出了类型可以表示的最大值，就可以说是发生了溢出（`overflow`）。

#### 读写 Integer

在 conversion specification 里使用 `u`，`x` 或 `o` 来表示 unsigned integer，`o` 表示 8 进制的数，`x` 表示 16 进制的数。

short integer 的情况使用 `hd`。
long integer 使用 `ld`。
long long integer 使用 `lld`（只在 C99 内可使用）

### Floating Types

C 语言没有说明 `float`, `double`, `long double` 的精度，因为不同的计算机使用不同的方式存储浮点数。大多数现代计算机都使用 `IEEE Standard 754`。

![float point number precision](/images/uploads/float_point_precision.png "float point number precision")

#### 读写 Float

`e`, `f`, `g` 表示单精度浮点数。
`le`, `lf`, `lg` 表示双精度浮点数。
 `Le`, `Lf`, `Lg` 表示 extended-precision floating-point

### Character Types

#### Arithmetic Types

C89 把 Arithmetic Types 分为两类：

1. Integeral types
   1. char
   2. Signed integer types
   3. Unsigned integer types
   4. Enumerated types
2. Floating types

C99 的分类则是：

1. Integer types
   1. char
   2. Signed integer types and extended (long long int)
   3. Unsigned integer types and extended (unsigned long long int, _Bool)
   4. Enumerated types
2. Floating types
   1. Real floating types
   2. Complex types

### Type Conversion (Important)

Implicit conversion 在下列情况下发生：

* 算术表达式和逻辑表达式中的 operand 类型不同（usual arithmetic conversion）
* 赋值操作的右侧与左侧类型不同
* 调用函数的参数（argument）与形参（parameter）类型不同
* return 语句的类型与函数返回类型不同

#### Arithmetic conversion

Arithmetic conversion 可以分为两种类型，在这之前，需要先了解什么是 _narrower type _，即是如果一个类型比其他类型用更少的字节来存储，则说这个 type 是 _narrower_ 的。

* operand 中某个类型是 floating type 的话，将 narrower 的类型提升
  ![floating type promotion](/images/uploads/float_promotion.png "floating type promotion")
* 没有 operand 是 floating type

![integer type promotion](/images/uploads/int_promotion.png "integer type promotion")

#### Conversion during assignment

右侧的类型转换成左侧的类型。

#### C99 的隐式转换规则

integer conversion rank

![c99 integer conversion rank](/images/uploads/c99_integer_conversion_rank.png "c99 integer conversion rank")

C99 的 Arithmetic conversion 规则：

* operand 中某个类型是 floating type 的话，且没有 `complex` 类型，规则与之前相同
* 都不是 floating type
  * 都是有符号或者无符号类型，将较低 rank 的类型转换成较高类型
  * 如果无符号类型的 rank 高于有符号类型，将有符号的类型转换成无符号的
  * 如果有符号的类型可以表示无符号类型所有的值，将无符号的转换为有符号的
  * 否则将有符号类型转换成对应的无符号类型
  * 所有类型都可以转换成 _Bool，0 还是 0，其他的为 1
