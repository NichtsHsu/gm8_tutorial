---
layout: post
title: 精灵与图像
nav_order: 10
---

本节对应 GML 汉化文档 41-42

---

## 精灵（Sprites）与图像（Images）

一个精灵是由一或多张图像组成的。一般的，images 更多被叫做“**子图**”。

## GM 自带的精灵相关变量

* `visible` 实例的精灵是否可见，参数为0（不可见）或者1（可见）。准确来说 `visible` 决定的是“是否执行绘制”，即使 `visible` 为 0，也会进行碰撞检测。（莫急，下一章就是碰撞）
* `sprite_index` 实例的精灵。可以用精灵的名字给它赋值，以改变实例所显示的精灵。
* `sprite_width` 实例当前精灵的宽度。该变量不可被赋值，只会随着 `sprite_index` 的改变而自动改变。
* `sprite_height` 实例当前精灵的高度。同上，不可被赋值。
* `sprite_xoffset` 实例当前精灵的水平偏移量。同上，不可被赋值。
* `sprite_yoffset` 实例当前精灵的垂直偏移量。同上，不可被赋值。

什么是**偏移量（offset）**：

![Offset](/assets/images/sprite_image/offset.png)

水平偏移量就是**原点（Origin）** x 坐标对应的值，垂直偏移量就是原点 y 坐标对应的值。

我们知道，一个对象的实例在房间中有一个坐标（即 GM 的自带变量 x 和 y）来表示它的位置的。那么它的精灵要画在哪个位置？这就是由原点所决定的。GM 会把精灵的原点放在 (x, y) 的位置来绘制。

* `image_number` 实例当前精灵所包含的子图数量。该变量不可被赋值。
* `depth` 深度，影响绘制事件的执行顺序。参见[浅谈对象]({{ site.baseurl }}{% link _tutorials/start/object.md %}#面板介绍)。

## GM 自带的图像相关变量

* `image_speed` 实例的精灵的播放速度（如果精灵是动态的）。值为0时表示实例的精灵不播放，静止在某一张子图上。值为 1 时表示播放速度为 1 张/帧。举个例子，一个 20 张子图的精灵，当游戏帧率是 60 帧/秒时，这张精灵一秒内将循环播放三次（即 60/20）。当值在 0\~1 之间时，会减慢播放速度。比如上一个例子中，`image_speed = 1/3;` 会让精灵一秒钟才完成一次播放。

* `image_index` 一个精灵的图像都有各自的编号：精灵的第一张图像是 0 号，第二张图像是 1 号，第 n 张图像编号是 n-1。`image_index` 这个变量储存的就是精灵这一帧正在播放的那张图像的编号。通常和 `image_speed = 0;` 一起使用，使精灵停留在某一张子图像上，很少单独使用。

  *提示：要牢记编程中都是从 0 开始编号的。*

* `image_single` 一个被官方遗忘的变量。该变量在官方的文档中没有被记录。

  作用如下：

  ```c
  image_single = k;
  ```

  等效于：

  ```c
  image_speed = 0;
  image_index = k;
  ```

* `image_xscale` 精灵在 x 方向上的伸缩。1 为正常尺寸，0~1 之间为缩小，大于 1 为放大，若为负数，先左右翻转，再放缩。注意，该 x 方向指精灵的 x 方向，不受 `image_angle` 的影响。
* `image_yscale` ：精灵在 y 方向上的伸缩。
* `image_angle` 精灵的旋转角度。旋转为逆时针旋转。
* `image_alpha` 精灵的透明度。1 为保持原样（默认），0 为完全透明。

  *注：如果精灵的子图本身就有地方是半透明的，那么最终透明度为二者相乘。例如原本某个像素的 alpha 值为 200，设置 `image_alpha` 为 0.4，那么最终透明度是 200 x 0.4 = 80。*
