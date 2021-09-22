---
layout: default
title: 绘制
nav_order: 17
has_children: true
permalink: /tutorials/draw/
---

# draw 事件

在 GM8 众多的事件中，有一个叫做 draw 的事件，即绘制事件。

![Draw Event](/assets/images/draw/draw_event.png)

绘制事件与步事件一样，是**每一步都会执行一次**的事件。那么绘制事件和步事件有什么区别呢？

## 执行顺序不同

现在来复习一下 GM 的事件执行顺序：

1. 步开始事件
2. 计时器事件
3. 键盘事件
4. 鼠标事件
5. 步事件
6. （实例移动到新位置）
7. 碰撞事件
8. 步结束事件
9. 绘制事件

绘制事件是一步之内最后执行的那个事件。

## 功能不同

绘制事件是GM中唯一能够实现“绘制”的事件。正常情况下，`draw_系列`函数(即以 draw_ 开头的函数及衍生的函数)，只有写在绘制事件里才能生效。

准确的说，如果绘制目标是游戏屏幕，则必须经由绘制事件实现。后期我们会学到，可以改变绘制目标到**表面**（surface）上，这时 draw_系列函数可以不写在绘制事件中。

## 实例有隐藏的默认绘制事件

如果你没有给对象添加绘制事件，那么它的实例会自动包含一个隐藏的绘制事件，这个绘制事件只执行一个操作——绘制自身精灵，即相当于：

```c
draw_sprite_ext(sprite_index, image_index, x, y, image_xscale, image_yscale, image_angle, image_blend, image_alpha);
```

如果你添加了绘制事件，那么原本的**默认绘制事件会自动消失**，也就是说实例不再绘制自己的精灵。你可以向绘制事件中添加上述代码来绘制自身精灵。在 GMS 中，你可以使用 `draw_self();` 代替，在GM8中，你也可以新建一个脚本 **drawSelf**，写入上述代码，以后调用 `drawSelf();` 来绘制自身。

## 不可见时不执行

如果实例不可见（即 `visible = false`），则实例不会执行绘制事件。但是步事件是一定会执行的。

注意：

1. 通过绘制事件绘制的图案并**不会影响**到实例的**碰撞盒**，实例的碰撞盒只取决于自身的 mask 属性（但是碰撞盒受 image_系列变量的影响）。
2. **绘制事件应保持纯粹**。绘制事件每一步都执行一次，所以它看起来能承当步事件的所有职责。但是，不建议把 draw_函数以外的代码，尤其是涉及到变量自增自减的代码放在绘制事件中执行。绘制事件应该只承担“绘制”这一功能。在讲解表面时我们会实际感受到问题。
3. 代码自上而下执行，在绘制事件中，后执行的绘制函数会把图像画在更表层，从而遮挡住先前执行的绘制函数的图像。但是，所有绘制的图像的深度都和实例的深度一致。