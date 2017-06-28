---
title: fix point method
subtitle: Install, config Hexo and Next
date: 2017-06-28 11:51:53
author: Ricann
head-img:
categories:  tools
tags:
 - caffe
 - fix point
 - train
keywords:  caffe, fix point, train
comments: true
---

<!--引擎-->
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--
示例公示：
$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$
\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)

\begin{equation}
E=mc^2
\end{equation}
-->

# Overview

有些时候，在对神经网络进行训练时，我们需要对浮点数进行定点，例如定点成8/16/32bit等，下图中是一个8 bit定点的例子。

<!--more-->

![strategy](/img/fix-point-method/fix-point-strategy.png)

# 计算方法
如果进行8比特定点，最高位为符号位，那么还有七位可以表示数字，表示范围是[-128, 127)，如果需要定点的数据范围是(-111, 386)，定点步骤如下：
 - 找出一个数字N，使2的N次方正好刚刚大于等于386（386相比-111离0更远），这时N=9
 - 使用下面公式计算出定点位置，得到定点位置-2

$$p = 7 - N$$

# 代码实现
下面函数先根据上面的定点位置计算出定点后的step（二进制最低位加一时实际的变化值），然后根据这个step计算出定点后的数据表示范围的下限和上限：

$$step = \frac{1}{2^p}$$

$$lb = -(2^7)*step$$

$$ub = (2^7)*step - step$$

相关代码实现如下：
```
void caffe_cpu_fix(int n, float* x, float* y, int bit_width, int p) {
  float step = std::pow(float(2), -p);
  float lower_bound = -std::pow(float(2), bit_width-1)*step;
  float upper_bound = std::pow(float(2), bit_width-1)*step - step;
  for (auto i = 0; i < n; ++i) {
    y[i] = fix_data(x[i], step, lower_bound, upper_bound);
  }
}

```

其中的fix_data函数定义如下：
```
float fix_data(float x, float step, float lb, float ub) {
  return std::fmin(std::fmax(std::round(x/step)*step, lb), ub);
}
```
