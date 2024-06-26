---
layout: post
title: 索引
nav_order: 22
---

## 实例索引

实例的索引，即实例的 id，应该是我们最为熟悉的索引了，因此本章就从实例的索引开始讲起。

我们知道，**对象只是一个模板**，一个对象在房间内可以有多个实例，区别不同的对象可以使用对象名，而区别同不同的实例就用实例的索引。GM8 会自动给每一个实例分配一个索引，这个索引用变量 id 储存，可以通过访问变量 id 来获取实例的索引。通常情况下，**实例 id 是从 100001 开始的**。GM 给实例分配索引是按照**时间先后顺序**的，首先给 gmk 工程中就已经摆在房间里的实例分配索引（按照摆上去的时间），然后在游戏运行时再接着给通过 `instance_create` 创建的实例分配索引。

*注意，被删除的实例的索引不会再被分配*。例如，你的房间内有四个实例，id 分别是 100001，100002，100003，100004，此时先删除 id 为 100002 的实例，再放置一个新的实例在房间内，新实例的 id 仍然是 100005。也就是说，100002 这个 id 永久置空了，再也不会有实例被分配为 100002。

实例的索引一定是连续分配的。比如：

```c
for (i = 0; i < 10; i += 1)
    inst[i] = instance_create(0 , 0, objSample);
```

那么 `inst[0]` 到 `inst[9]` 一定是十个连续的数字。因此，`(inst[0] + 9).speed = 5;` 和 `(inst[9]).speed = 5;` 是等效的。

在 GML 汉化文档的 27-28 页，提供了许多可以获取实例 id 的变量或函数。

![Doc](/assets/images/id/doc.png)

注意，在GML汉化文档的第27页有这么一句话：

![Wrong](/assets/images/id/wrong.png)

英文文本为：

> Note that the assignment of the instances to the **instance id**\'s changes every step so you cannot use values from previous steps.

实际上是文档错误，把函数 `instance_id` 漏写了下划线 `_`，变成了 `instance id`，原文意思就变味了。正确的文档是：

> Note that the assignment of the instances to the **instance_id**\'s changes every step so you cannot use values from previous steps.

即：分配给 `instance_id` 的实例索引每一步都会不相同，所以不可以使用上一步保存的值。

## 资源名？索引！

首先我们需要明确一个事实，与其他很多语言不同的是，GM 只有两种数据，**实数**型（real）和**字符串**型（string），如果你学过 C 语言或者 C++，这两种类型就分别对应 C/C++ 里的 `double` 类型和 `char*` 类型。如果你想用 C/C++ 给 GM8 写插件的话，这一点是一定要牢记的。

给未学过编程的 GM 学习者的科普：通常一种语言有许多种数据类型，就拿 C++ 举例，有空类型（`void`），字符型（`char`），宽字符型（`wchar_t`），整形（`int`），单浮点型（`float`），双浮点型（`double`），这些类型又分为有符号型（`signed`）和无符号型（`unsigned`），例如无符号整形（`unsigned int`），整形和双浮点型又有长（`long`）短（`short`）之分，比如无符号长整型（`unsigned long int`），每一种类型都有对应的指针类型（`*`），比如单浮点指针类型（`float*`），指针类型本身还可以有指针，比如长双浮点形指针的指针类型（`long double**`），编程者也可以通过 `struct`/`class` 创建新的类型，比如 `class complex{}`，就创建了一个新的 `complex` 类型。因此，GM 只有两种数据类型，可以说是真的相当友好了。

咳，扯远了。现在假设我们有一个精灵 sprLight，我们用一个变量来保存它：

```c
spr = sprLight;
```

那么spr这个变量到底储存了什么呢，是字符串 `"sprLight"` 吗？如果你有心自己试验一下，通过 `show_message` 或者 `draw_text`，你会发现 **spr 储存了一个数字**。

如果你更有心，把所有资源：精灵，音频，背景图片，路径，脚本，字体，时间轴，对象，房间，全部用变量来保存，你会发现，所有这些变量保存的都是一个数字，而不是他们的名字对应的字符串。这些数字就是**资源的索引**。

索引并不是实例的特权，任何存在于 GM 中的资源都会被分配索引。

## 万物皆索引

“任何存在于 GM 中的资源都会被分配索引”并不是夸大其词，除了实例的索引和资源的索引外，视野，表面，图块，颜色，粒子，数据结构，dll 插件，文件流等等，都是通过索引来操作的。这些索引大都遵循着共同的规则：

* 他们的本质是整数。
* 从 0 开始分配第一个索引。（特例：字符串从 1 开始。实例索引从 100001 开始，贴图索引从 10000001 开始）
* 通常，分配是连续的，上一个分配 n，下一个就分配 n+1。（特例：表面会重复利用索引）
* 任何函数的参数要求某项索引时，直接填入准确的数字是可以的。

## 对象与实例

那么为什么实例的索引是从 100001 开始的呢？因为前 100000 是预留给对象的。

我们知道，在很多时候，一个参数位置是既可以填入对象名也可以填入实例索引的，通常用 obj/id 来表示，例如：`with (obj/id)`，`place_meeting(x, y, obj/id)`，`instance_exists(obj/id)` 等。如果实例索引也是从 0 开始的，在调用函数时，GM 就无法确认这个索引到底是对象的还是实例的。因此，GM8 给对象预留了 0-100000 的索引位置，让实例的索引从 100001 开始。

我们之前还提到过特殊实例，`self`，`other`，`all`，`noone`，`global`，无主，`local`，他们的索引分别是 -1，-2，-3，-4，-5，-6，-7，`self`（-1）表示调用者自身，通常用于当局部变量与 `globalvar` 变量冲突时，区别局部变量和 `globalvar` 变量，`other`（-2）表示对方，用于碰撞事件中表示碰撞的实例，或在 `with ()` 结构中表示 `with` 的调用者，`all`（-3）表示所有实例，`noone`（-4）表示无实例，因此 `noone` 不能储存变量或执行函数，`global`（-5）表示全局变量，**无主变量**（-6）是一种特殊的变量，表示不属于任何实例的变量，目前已知的有两种，第一是通过 `globalvar` 声明的变量，第二种是在房间创建代码（Room Creation Code）中使用的变量，`local`（-7）表示通过 `var` 声明的变量。

注意，通过 `globalvar` 声明的变量可以同时使用 `(-5)` 和 `(-6)` 调用，但是通过 `global.` 声明的变量只能使用 `(-5)` 调用，除非再用 `globalvar` 声明一次。

由于索引本质就是实数，因此 `num = global * local;` 会给 num 赋值 35。

至于贴图索引从 10000001 开始，有两种说法，一种是前 10000000 是预留给背景资源的，但是这种说法我个人并不认同，因为显然一个游戏中背景资源数量不可能达到对象资源数量的 100 倍量级，因此我更倾向于认为：100001\~10000000 是预留给实例的。不过不管是哪一种，目前并没有发现能够将贴图索引和其他索引混用的函数，因此也无从证明。

## 扩展：句柄

**句柄**（Handle）是 Windows 系统的概念，不属于 GM。

句柄可以理解为 Windows 用来管理资源的索引，其本质是一个 0 至 4294967295 之间的整数。与 GM 的索引相同，Windows 对系统内几乎所有的资源都分配了句柄，文件有文件句柄，窗口有窗口句柄，进程有进程句柄，位图有位图句柄，内存有内存句柄等，想要管理 Windows 资源，大都得先知道资源对应的句柄。

由于 GM 没有提供对句柄的操作函数，因此并不能直接管理 Windows 资源，但是我们却可以管理 GM 的资源。GM中有一个函数 `window_handle()`，返回游戏主窗口的句柄。在编写 dll 插件时，如果想要直接向游戏窗口内添加内容，而不是经过 GM 的函数，就必须得到游戏窗口的句柄。
