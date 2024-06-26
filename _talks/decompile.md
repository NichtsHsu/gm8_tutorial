---
layout: post
title: GM8 与反编译与混淆
nav_order: 2
---

## 反编译

GM8 的代码和资源是毫无加密地封装进 exe 中的，因此通过反向推导 exe 的封装逻辑，就可以通过 exe 复现出 gmk，即**反编译**（Decompile），俗称“**拆**”。由于 exe 不再储存资源的结构层次，因此反编译唯一丢失的 gmk 信息就是资源结构层次。

最早的 GM8 反编译器（Decompiler）用 C++ 写的，GitHub 地址为：[GameMaker 8.1 Decompiler](https://github.com/WastedMeerkat/gm81decompiler)。该版本存在一些 bug，并且多年来无人维护。近段时间有大佬使用 Rust 重写了反编译器，Github 地址：[GM8Decompiler](https://github.com/OpenGM8/GM8Decompiler)。

## 反反编译

虽然有些作者并不是很在意自己的游戏被拆，但是也有作者并不希望自己的游戏被拆。自然，也就会有人去研究**反反编译**（Anti-Decompile）和反反编译器（Anti-Decompiler）。

反反编译的原理几乎都是加壳，其中最常见的是 **UPX 壳**，原理是向 exe 加入无用字段后再压缩为新的 exe，这个新的 exe 是一个自解压程序，运行时解压出原本的 exe 到内存中，更详细的步骤就不介绍了，有兴趣可以自己了解。加壳之后的游戏可以有效抵抗反编译器，但是加壳后的 exe 包含了原本的所有信息，通过一些软件和手段依然可以对 exe 脱壳，并不是完全安全的。

一个反反编译器：[GameMaker Anti-Decompiler](https://iwbte-nikaple-edition-1255674901.cos.ap-guangzhou.myqcloud.com/engine/anti-decompiler.zip)

PS: 目前 Rust 版本的反编译器也有能力反拆许多主流的加壳程序，包括上面给出的反反编译器。

## 混淆

只要 exe 仍然包含原本的所有信息，那么无论怎么加密都只能阻挡 99.9% 的人，对于 0.1% 的高手而言都是白纸，这一点在我混迹 I Wanna 圈的时候深刻的理解到了。

但是，为了对抗反编译，作者们又想出了新的招式：**混淆**（Obfuscate）。

混淆并不是加密，它不是对 exe 动手，而是对 gmk 动手。混淆的原理是利用了 GM8 中资源名、关键字等都是实数的特性，将代码中的数字转换为关键字和资源名，将关键字和资源名转换为数字，另外，他将自定义资源名，变量名都改成乱七八糟的名字，将大量代码全部放在同一行。例如：

混淆前的代码：

![Obfuscate](/assets/images/decompile/obfuscate.png)

混淆之后的代码：

![Obfuscate Result](/assets/images/decompile/obfuscate_result.png)

是不是感觉完全看不懂？但是这并不是乱码，这段混淆后的代码仍然具有和混淆前一模一样的执行效果。由于这是直接对 gmk 的改动，因此没有任何复原的可能性。

混淆的原则就是：你拆任你拆，能看懂代码算你赢。

一个混淆器：[GM Obfuscator](https://iwbte-nikaple-edition-1255674901.cos.ap-guangzhou.myqcloud.com/engine/GM%20Obfuscator%20030.jar)。注：该混淆器是一个 jar 包，请使用

```c
java -jar "GM Obfuscator 030.jar"
```

来运行。

PS：虽然 Rust 版本的反编译器有提供反混淆功能，但实际上只是：代码格式化、资源和变量重命名（`field1`, `field2`, `object1`, `object2`），代码的可读性仍然可以说是没有。
