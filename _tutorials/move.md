---
layout: post
title: 运动
nav_order: 7
---

本章相关内容：GML 汉化文档 21-22 页

---

## 与运动有关的 GM 自带变量

再次强调：

* 改变 GM 自带的对象变量会导致对象属性改变，所以绝对不能将其作为自定义变量来使用。GM 自带的对象变量会自动染上红色，以此作为判断变量名是否可以用作一般变量。脚本同理，不能以 GM 自带的函数名或变量名作为自己的脚本名。
* 帧是一个时间点概念，步是一个时间段概念，一步是相邻两帧之间的一小段时间。
* 在 GM 中，左上角为原点，向右为 x 轴正方向，向下为 y 轴正方向。其中角度以正右方为 0，正上为 90，正左为 180，正下为 270。角度超出 0~360 的范围是允许的，比如 -90 和 270 在 GM 中都是指正下方。

### 坐标

* `x` 对象的实例在房间内的 x 坐标。
* `y` 对象的实例在房间内的 y 坐标。
* `xprevious` 对象的实例以前的 x 坐标。
* `yprevious` 对象的实例以前的 y 坐标。
* `xstart` 对象在房间里的初始 x 坐标。
* `ystart` 对象在房间里的初始 y 坐标。

所谓以前的坐标，即一步开始时实例的坐标。换而言之，就是上一步的绘制事件结束之后，这一步的步开始事件开始之前，实例的位置。

### 速度

* `speed` 对象的实例在房间里的速度。单位是像素/帧。
* `direction` 速度的方向。单位是角度。默认的方向是 0，即实例默认向右运动。如上所说，范围不一定是 0~360 之间，也可以超出。
* `hspeed` speed 在水平方向的分速度。注意这里 "h" 是指 "horizontal"，即“水平的”，切勿记成了物理课上用 h 来表示高度。
* `vspeed` speed 在垂直方向的分速度。"v" 是指 "vertical"，即“垂直的”。

*注意：`speed`，`direction`，`hspeed`，`vspeed` 是连锁关系。*

即代码中写：

```c
speed = 5;
direction = 36.8699;
show_message(string(vspeed) + " and " + string(hspeed));
```

会输出：`-3 and 4`。

即，一旦改变 `speed` 和 `direction` 的值，`vspeed` 和 `hspeed` 的值也会自动改变为对应的值。同样的，一旦改变 `vspeed` 和 `hspeed` 的值，`speed` 和 `direction`也会自动地变为对应的值。

### 加速度

* `gravity` 重力。单位为**像素/帧²**。默认方向为 270，即正向下。实际上，由于重力方向可变，再加上 GM 里并没有“质量”的概念，重力直接作用给速度，所以其实“重力”理解为“加速度”才是最准确的。
* `gravity_direction` 重力的方向。
* `friction` 阻力。顾名思义，方向始终保持和 `speed` 相反。当 `speed` 减为 0 时不再起作用。

*注意：`gravity` 与 `friction` 生效早于 `speed` 生效。*

如果有代码：

```c
speed = 5;
friction = 0.7;
```

那么实例这一步会移动 4.3 个像素。

## 最基本的运动函数

* `move_towards_point(x, y, spd)` 以速度 spd 向坐标 (x, y) 运动。

这个函数与 `speed`，`direction`，`hspeed`，`vspeed` 是连锁关系。其实从本质上来说，这个函数就是通过给 `hspeed` 和 `vspeed` 赋值来实现实例的运动。

提示：

如果 (x, y) 是玩家的坐标——即形如 `move_towards_point(player.x, player.y, 5)`，此处假设 player 就是玩家可以操控的对象——那么这个函数可以生成两种最常见的弹幕类型：写在 create 事件中就是自机狙，写在 step 事件中就是跟踪弹。

原理：

如果这个函数写在 create 事件里，那么只有在实例刚刚创建时执行一次，之后如果没有别的代码改变 `direction` 的话，运动方向就会一直保持不变。

如果这个函数写在 step 里，实例每一步都会根据 (x, y) 的变化自动调整 `direction`，使实例的运动方向始终保持指向 (x, y) 的位置。

*注意*：当实例运动到 (x, y) 这个位置时，实例并不会停止，速度仍然保持不变，会原地无限鬼畜。。。所以如果 (x, y) 是一个固定的坐标，千万别写不要放在 step 事件里。

如果想让某个实例移动到某一个位置并在这个位置停止，`move_towards_point` 无论是写在 create 事件里还是 step 事件里都不会在到达终点时令实例停止，但是我们[以后]({{ site.baseurl }}{% link _tutorials/path_move.md %}#运动设计)会讲到函数 `mp_linear_step_object(x, y, stepsize, obj)` 来实现这个功能。

## 保持精灵方向与运动方向一致

如果对象的精灵是有方向性的，比如炮弹啊什么的，如果精灵的方向和运动方向不一样，看起来就会很别扭。这时候我们介绍两个 GM 自带的对象变量：

### 旋转

* `image_angle` 精灵的旋转角度。默认为 0，即不旋转。

如果精灵方向是向右的，由于 GM 中坐标系是以正右作为 0 度，那么只要保持精灵旋转的角度和运动方向一致就行：

```c
image_angle = direction;
```

由于GM的代码是按顺序从上到下运行，所以切记要写在改变了 `direction` 的代码的后面。

如果精灵方向不是向右的，就得通过计算得出 `image_angle` 和 `direction` 的关系。

如果精灵向上，则：

```c
image_angle = direction - 90;
```

如果精灵向左，则：

```c
image_angle = direction - 180;
```

如果精灵向下，则：

```c
image_angle = direction - 270;
```

其实就是相当于先把精灵旋转为向右，然后再和 `direction` 保持一致。

### 翻转

有的精灵并不能靠旋转来保持和运动方向一致。

比如玩家操作的角色，向左运动则精灵向左，向右运动则精灵向右，如果用 `image_angle` 来旋转精灵，那么旋转 180 度之后，精灵不仅左右会翻转，上下也会翻转，但是上下翻转显然不是我们想要的，我们只要左右翻转就行了。

所以我们应该用到：

* `image_xscale` 在 x 方向上扩大/缩小精灵。默认为 1，大于 1 为放大，0~1 之间为缩小。

该变量的值可以是负数，此时先左右翻转，再放大/缩小。

于是我们想要的效果就是：

```c
image_xscale = -1;
```

*注意：`image_angle` 和 `image_xscale` 都是相对于原精灵进行的改变，而不是叠加的*。例如：

```c
image_angle = 50;
image_angle = 70;
```

此时精灵的实际旋转角度是 70 而不是 120。`image_xscale` 和 `image_yscale` 也是同理。
