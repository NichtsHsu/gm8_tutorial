---
layout: default
title: 位运算
nav_order: 1
parent: 二进制
---

# 模拟电路和数字电路

**模拟电路**（analog circuit）是一种更加贴合实际的电路，它的电压的取值范围是连续的。而**数字电路**（digital circuit）的电压通常只有两种取值，低电平（代表“关”）或高电平（代表“开”）。因此，数字电路是更加贴合用来设计二进制电子产品的电路，我们之后所谈到的电路，不特别说明的情况下都是指**数字电路**。

# 逻辑门

二进制的加减乘除与十进制类似，只要注意逢二进一，以及借位时借 2 即可，因此我们还是来介绍一些二进制独有的运算吧。

假设我们是一个做电路元件的老师傅，现在需要这么一种电路元件，他接通三根电线，其中两根输入电流，一根输出电流，而两根输入电线既可能是“关”的状态，也可能是“开”的状态，我们要根据这两根输入电线的状态决定输出电线的状态。如果要求两根输入电线都要用上并且地位完全平等，也就是说，我们不能只管一根电线无视另一根电线，或者干脆两根都不管稳定输出“关”或“开”，那么我们一共可以做出多少种不同的电路元件？

答案是六种。这种电路元件通常叫做**逻辑门**（logic gate）。我们用 1 和 0 分别表示“开”和“关”来介绍这六种逻辑门：

## AND（与门）

只有当输入都是 1 时，输出才是 1，任意一个输入为 0，输出是 0。

A | B | Out
-- | -- | --
0 | 0 | 0
0 | 1 | 0
1 | 0 | 0
1 | 1 | 1

## OR（或门）

任意一个输入是 1 时，输出是 1。只有输入全是 0 时，输出才是 0。

A | B | Out
-- | -- | --
0 | 0 | 0
0 | 1 | 1
1 | 0 | 1
1 | 1 | 1

## XOR（异或门）

输入不相同时，输出是 1，输入相同时，输出是 0。对于更多输入的异或门（输入大于 2）而言，当奇数个输入是 1 时输出是 1，当偶数个输入是 1 时输出是 0。

A | B | Out
-- | -- | --
0 | 0 | 0
0 | 1 | 1
1 | 0 | 1
1 | 1 | 0

## NAND（与非门）

和与门输出相反的逻辑门。

A | B | Out
-- | -- | --
0 | 0 | 1
0 | 1 | 1
1 | 0 | 1
1 | 1 | 0

## NOR（或非门）

和或门输出相反的逻辑门。

A | B | Out
-- | -- | --
0 | 0 | 1
0 | 1 | 0
1 | 0 | 0
1 | 1 | 0

## XNOR（同或门）

和异或门输出相反的逻辑门。

A | B | Out
-- | -- | --
0 | 0 | 1
0 | 1 | 0
1 | 0 | 0
1 | 1 | 1

---

除此之外，还有一种只有一个输入的逻辑门：

## NOT（非门）

将输入反转的逻辑门。

In | Out
-- | --
0 | 1
1 | 0

什么，你问我有没有输入什么就输出什么的逻辑门？那我只会建议你直接把电线连上去。不过一定要说的话，在设计逻辑电路时通常会使用一种名为 **BUF**（缓冲器）的电子元件，它就是输入啥就输出啥。由于逻辑门的计算需要消耗一定的时间，造成延迟，为了保证一定范围内所有电路的时间延迟保持一致，通常会增加BUF以增加某条线路上的时间延迟。不过 BUF 通常意义上并不会算作逻辑门的一种。

观察可知，NAND 等同于 AND 接上 NOT，NOR 等同于 OR 接上 NOT，XNOR 等同于 XOR 接上 NOT，因此我们实际**只需要 4 种逻辑门（AND, OR, XOR, NOT）**就可以实现全部 7 种逻辑门的功能了。

# 位运算

两个二进制数之间，除了可以加减乘除之外，还能进行一种特殊的运算，叫做**位运算**（bitwise operation）。位运算是基于上述逻辑门而实现的一种运算，例如我们有两个二进制数 10010101 和 00010111，我们取第一个数的第一位 1 和第二个数的第一位 0，通过某个逻辑门得到计算结果的第一位（如AND下为0）；取第一个数的第二位 0 和第二个数的第二位 0，通过某个逻辑门得到计算结果的第二位（如 AND 下为 0）；以此类推，直到得到计算结果的所有位数，这种运算就是位运算。

我们之前学习逻辑运算时，我们使用的运算符是 &&，\|\|，^^，肯定有很多人奇怪为什么要写两遍，那是因为 &，\|，^ 是**位运算符**（bitwise operator）。

## &（按位与）

将两个数值的每一位二进制通过 AND 逻辑门得到输出结果对应的那一位。

如，计算 `6&13=8` 的过程：

```c
0 1 1 0  (6)
1 1 0 1  (13)
---------------
0 1 0 0  (8)
```

## \|（按位或）

将两个数值的每一位二进制通过 OR 逻辑门得到输出结果对应的那一位。

如，计算 `6|13=15` 的过程：

```c
0 1 1 0  (6)
1 1 0 1  (13)
---------------
1 1 1 1  (15)
```

## ^（按位异或）

将两个数值的每一位二进制通过 XOR 逻辑门得到输出结果对应的那一位。

如，计算 `6^13=11` 的过程：

```c
0 1 1 0  (6)
1 1 0 1  (13)
---------------
1 0 1 1  (11)
```

## ~（按位取反）

将数值中所有 1 变为 0，所有 0 变为 1。*根据数值精度的不同，运算结果也不同。*

假设数值精度是四位，那么 `~6=9` 的过程如下：

```c
0 1 1 0  (6)
--------------
1 0 0 1  (9)
```

假设数值精度是八位，那么 `~6=249` 的过程如下：

```c
0 0 0 0 0 1 1 0  (6)
------------------------
1 1 1 1 1 0 0 1  (249)
```

---

除此之外，位运算还包含两种**移位**（shift）运算：

## <<（左移）

`a << b` 代表将 a 中每个二进制位向左移动b位，多出的位置用 0 补上。通常 b 很小。

例如，假设数值精度是八位，那么 `6<<3=48` 的计算过程：

```c
0 0 0 0 0 1 1 0  (6)
------------------------------
      0 0 1 1 0 0 0 0  (48)
```

如果左移过程中，有某个位置的 1 超出了数值精度范围，那么他将被略去。

例如，假设精度数值为八位，那么 `35<<3=24` 的计算过程：

```c
0 0 1 0 0 0 1 1  (35)
-------------------------------
      0 0 0 1 1 0 0 0  (24)
```

在不超出精度的情况下，`a<<b` 等效于 `ax2^b`。

## >>（右移）

右移又分为三种：

1. 逻辑右移：类似左移，将所有二进制位向右移动，超出部分略去，多出的位置用 0 补上。如 `11000011` 右移三位变成 `00011000`。
2. 循环右移：把整个数值当做首尾相连的循环，从最右边移出来的位将补到最左边多出来的位上。如 `11000011` 右移三位变成 `01111000`。
3. 算术右移：超出部分略去，多出来的位置用原数值最高位填满。如 `11000011` 右移三位变成 `11111000`。

算术右移的出现是负数的二进制表示方式引出的，我们将在后文中讲解负数的二进制表示方式，有时也会使用 `>>>` 表示算术右移。而 GM8 中不支持对负数右移（但是支持对负数的左移），这点就很令人难受。`a>>b` 通常等效于 `a÷2^b`。

---

## 扩展：按位异或的特性

按位异或有一个特性：**如果 `c = a ^ b`，那么 `b = a ^ c`，且 `a = b ^ c`**。

利用这种特性，我们可以不使用第三个变量就能交换两个变量的值：

```c
a ^= b;
b ^= a;
a ^= b;
```

此外，我们把这个特性看做`加密文本 = 原始文本 ^ 秘钥`的话，那么`原始文本 = 加密文本 ^ 秘钥`，构成了一个简单的数据加密。事实上，很多主流的加密算法中基本上都包含有按位异或的步骤。不过纯粹的按位异或加密是不安全的，别人只需要拿到加密程序，输入一段明文，得到密文后，通过`秘钥 = 原始文本 ^ 加密文本`反推出秘钥，导致所有密文都被破解。