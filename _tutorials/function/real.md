---
layout: post
title: 实数函数
nav_order: 1
parent: 函数
toc: false
---

即 GML 汉化文档 16-18 页的函数。

GM8 可以使用许多的数学函数，如：

* `abs(x)` 返回 x 的绝对值
* `sign(x)` 返回 x 的符号（正数为 1，0 为 0，负数为 -1）
* `round(x)` 返回 x 的四舍五入
* `floor(x)` 返回比 x 小的最大整数
* `ceil(x)` 返回比 x 大的最小整数
* `frac(x)` 返回 x 的小数部分
* `sqrt(x)` 返回 x 的开方
* `sqr(x)` 返回 x 的平方
* `power(x, n)` 返回 x 的 n 次方
* `exp(x)` 返回 e 的 x 次方
* `ln(x)` 返回 x 的自然对数
* `log2(x)` 返回 x 关于 2 的对数
* `log10(x)` 返回 x 关于 10 的对数
* `logn(n, x)` 返回 x 关于 n 的对数
* `sin(x)` 返回 x 的正弦，x 为弧度制
* `cos(x)` 返回 x 的余弦，x 为弧度制
* `tan(x)` 返回 x 的正切，x 为弧度制
* `arcsin(x)` 返回 x 的反正弦
* `arccos(x)` 返回 x 的反余弦
* `arctan(x)` 返回 x 的反正切
* `arctan2(y, x)` 返回 y/x 的反正切，并且根据 y 和 x 自动纠正象限
* `degtorad(x)` 将角度 x 转换为弧度
* `radtodeg(x)` 将弧度 x 转换为角度
* `min(x1, x2, x3, ...)` 返回一系列数值中的最小值
* `max(x1, x2, x3, ...)` 返回一系列数值中的最大值
* `mean(x1, x2, x3, ...)` 返回一系列数值中的平均值
* `median(x1, x2, x3, ...)` 返回一系列数值中的中位数

除此之外，GM8 还允许你使用随机数：

* `random(x)` 返回一个 [0, x) 之间的随机实数（包含整数和小数）。
* `irandom(x)` 返回一个 [0, x] 之间的随机整数。
* `random_range(x1,x2)` 返回一个 [x1, x2) 之间的随机实数。
* `irandom_range(x1,x2)` 返回一个 [x1, x2] 之间的随机整数。
* `choose(val1,val2,val3,....)` 随机返回最多 16 个参数中的一个。
* `random_set_seed(seed)` 设置随机种子，随机种子要求是整数。
* `random_get_seed()` 返回当前正在使用的随机种子。
* `randomize()` 设置随机数作为随机种子。

> ### 随机数和随机种子
>
>
> 在量子计算机被发明并普及之前，现在的电脑还只能实现**伪随机**而不是真正意义上的随机。所谓的伪随机，就是当计算机需要生成一个随机数时，先找到一个难以预料的数值（比如说系统时间），然后利用数学算法计算出一个数字（或一系列数字）作为“随机数”使用（这个难以预料的数值，就被叫做**随机种子**），随机种子相同的话，生成的随机数也是相同的。
>
> 随机数并不是真的随机，在条件完全相同的情况下，计算机给出的随机数也是完全相同的。
>
> *扩展*：一个随机种子并不是只能计算出一个随机数，而是可以计算出成百上千个随机数，这些随机数的顺序是固定的，这个由随机数组成的数列，就叫**随机序列**。GM8仅在游戏开始时获取一次系统时间作为随机种子，之后每次需要随机数时，都从随机序列里按顺序取出。当然，从实际实现的角度来看，GM8 并没有把整个序列都一口气算出并储存起来。GM8 每次生成随机数的同时，都会同时更新一次随机种子（因此每次执行随机函数后 `random_get_seed()` 都会发生变化），这次的随机种子就是由上一次的随机种子计算得到的；当下一次计算随机值时，则通过这次生成的随机种子生成新的随机值和新的随机种子；只要初始随机种子是固定的，那么后续所有产生的随机值和随机种子的值和顺序就都是固定的。

注意：

1. 所有三角函数均使用弧度制。其他的函数大都使用的是角度制。
2. 在 GM8 里，向右为 0°，向上为 90°，向左为 180°，向下为 270°，在使用角度时应多加注意。
3. GM8 的房间以左上角为原点，向右为 x 轴正方向，向下为 y 轴正方向。

* `lengthdir_x(len,dir)` 返回指定长度和角度的向量在 x 轴上的投影长度。使用角度制。
* `lengthdir_y(len,dir)` 返回指定长度和角度的向量在 y 轴上的投影长度。使用角度制。

![Length Dir](/assets/images/function/lengthdir.png)

* `point_distance(x1, y1, x2, y2)` 返回坐标 (x1, y1) 与 (x2, y2) 的距离。
* `point_direction(x1, y1, x2, y2)` 返回坐标 (x2, y2) 相对于 (x1, y1) 的角度。
* `is_real(x)` 返回 x 是否为实数值。（返回 1 为实数值，返回 0 为字符串）
* `is_string(x)` 返回 x 是否为字符串。（返回 1 为字符串，返回 0 为实数值）

**实数（Real）**与**字符串（String）**是 GM8 的两种数据类型。

**实数**即是指整数和小数，或者说有理数和无理数的总称。

**字符串**即是指形如 `"I love world!"`，`"abc123"`，`"1+2=3"`，`"这个辣鸡教程"`，`"$&%^(+/$#"` 等，由字符（字母、数字、文字、符号）组成的，为了与编程语句区分开而加上了**双引号**的一种数据。注：也可以用单引号，但是不建议使用。GMS2 已停止支持单引号字符串。
