---
layout: post
title: Understand The D3js Data Binding
date: 2015-01-20 22:14
comments: true
categories: programming
author: Eric Zhang, Simon Shi
---

d3js很好地对svg元素与数据之间作绑定，在编写好javascript代码之后，只需要通过更新数据，就可以很好地实现svg的绘制，图像会基于数据的更新而更新，目前比较多数据可视化的网页都有通过d3js进行实现。但对于初学者来说，理解d3js中的数据绑定概念可能相对会困难一些。但只有在理解了数据绑定的前提下，才能准确清楚的写出专业的数据可化视化代码。本文旨在介绍d3js的数据绑定的相应概念，对其它svg属性的介绍不在本文范围之内。

## 基本概念
### 数据(data)与DOM元素(element)的pair
我们知道，在使用d3js时，常常用到`d3.selectAll()`，这是什么意思呢？它的意思是会选择相应的data与element(s)的pari(s)，我们可以这样想像一下，在d3js的概念中，每个DOM元素都会有与之对应的data，这样就组成了data/selement这样的pair，如图所示：

那么，每一行的d3js的代码调用结果会是怎样的呢？

```html
<!DOCTYPE html>
<html>
<head>
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
</head>
<body>
	<h1>Demo</h1>
    <script type="text/javascript">
	    var data = [1, 2, 3];
		var s = d3.selectAll("rect").data(data);
    </script>
</body>
</html>
```
当执行代码`d3.selectAll("rect")`时，返回值会是一系列的data/element的pairs，只是这个时候data区域是空的，如图，
只有在调用`data()`函数后，元素与数据进行了绑定，这样，原来的pairs中的data区域就填上了数值，pairs有多少对，取决于`data`变量的长度。

### `enter().append(element)`

### `exit().remove()`

## 模式

-EOF-



