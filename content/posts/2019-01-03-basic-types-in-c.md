---
title: Basic Types in C
date: 2019-01-02T14:57:30.438Z
draft: true
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

*
