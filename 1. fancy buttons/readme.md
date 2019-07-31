# CSS实现8种炫酷按钮

在各种UI组件库大行其道的今天，大家已经很少自己去实现一些炫酷的效果了，久而久之写CSS的水平也越来越退步，所以有空还是得练练。今天给大家分享8种炫酷按钮的CSS实现。

# 1. 3D按钮1

![3D Button 1 Gif](http://lc-PX2vd1LW.cn-n1.lcfile.com/4fca21c8987d856cb121/3d-button-1.gif)

[3D Button 1](https://codepen.io/mudontire/pen/aeJQqB)

虽然现在的主流是扁平化的UI设计，拟物化的设计比较少见了，所以我们仅从技术角度去分析如何实现这个3D按钮（文中只列出各种按钮实现的关键代码，完整代码请参考CodePen）。

该按钮的立体效果主要由按钮多出的左、下两个侧面衬托出来，我们可以使用`box-shadow`模拟出这两个侧面：

**CSS:**

```
.button-3d-1{
  ...
  box-shadow: -6px 6px 0 hsl(16, 100%, 30%);
}
```

**效果：**

![just box shadow](http://lc-PX2vd1LW.cn-n1.lcfile.com/94a6687ad41624dd0411/just%20box%20shadow.png)

加了`box-shadow`之后整体形状有点像了，但是立方体的左上和右下确缺了一块。通过观察我们发现，缺的这两块是三角形的，所以如果我们能构造两个三角形补到这两个角上就行了。用CSS画三角形对大家来说应该是基本操作了，如果还有同学不知道，下面的动画很好的解释了原理（代码参考 [Triangle](https://codepen.io/mudontire/pen/MNmVjR)）：

![triangle animation](http://lc-PX2vd1LW.cn-n1.lcfile.com/12409591e06fc8a71bfd/triangle%20animation.gif)

我们使用`::before`和`::after`伪元素来分别绘制左上、右下的两个三角形，并通过绝对定位将它们固定到两个角落：

**CSS：**
```
.button-3d-1::before {
  content: "";
  display: block;
  width: 0;
  height: 0;
  position: absolute;
  top: 0;
  left: -6px;
  border: 6px solid transparent;
  border-right: 6px solid hsl(16, 100%, 30%);
  border-left-width: 0px;
}

.button-3d-1::after {
  content: "";
  display: block;
  width: 0;
  height: 0;
  position: absolute;
  bottom: -6px;
  right: 0;
  border: 6px solid transparent;
  border-top: 6px solid hsl(16, 100%, 30%);
  border-bottom-width: 0px;
 
}
```

接下来，我们需要实现点击时按钮被按下的效果，思路是点击时将按钮正面向左下角移动，同时减少`box-shadow`的偏移量以达到按钮底部固定不动的效果：

**CSS：**
```
.button-3d-1:active {
  background: hsl(16, 100%, 40%);
  top: 3px;
  left: -3px;
  box-shadow: -3px 3px 0 hsl(16, 100%, 30%);
}
```

最后，我们需要重新计算左上、右下两个三角形的尺寸和位置，让它们与按钮主体看起来始终为一个整体：

**CSS：**
```
.button-3d-1:active::before {
  border: 3px solid transparent;
  border-right: 3px solid hsl(16, 100%, 30%);
  border-left-width: 0px;
  left: -3px;
}

.button-3d-1:active::after {
  border: 3px solid transparent;
  border-top: 3px solid hsl(16, 100%, 30%);
  border-bottom-width: 0px;
  bottom: -3px;
}
```

# 2. 3D按钮2

![3D Button 2 Gif](http://lc-PX2vd1LW.cn-n1.lcfile.com/35463e1e209c26cb05a3/3d-button-2.gif)

[3D Button 2](https://codepen.io/mudontire/pen/KOmWBN)

对于这种圆柱形的按钮，思路和上面矩形3D的按钮类似，也是通过`box-shadow`构造侧面实现立体感。为了使效果更加真实，侧面的颜色呈现渐变效果，越往下颜色越深，因此我们可以通过叠加多层`box-shadow`来实现：

**CSS：**

```
.button-3d-2{
  ...
  box-shadow: inset 0 1px 0 hsl(54, 100%, 50%),
              0 2px 0 hsl(54, 100%, 20%),
              0 3px 0 hsl(54, 100%, 18%),
              0 4px 0 hsl(54, 100%, 16%),
              0 5px 0 hsl(54, 100%, 14%),
              0 6px 0 hsl(54, 100%, 12%),
              0 7px 0 hsl(54, 100%, 10%),
              0 8px 0 hsl(54, 100%, 8%),
              0 9px 0 hsl(54, 100%, 6%);
}
```

当点击按钮的时候，通过下移按钮和减少`box-shadow`的层数来实现按钮被按下的效果：

**CSS：**

```
.button-3d-2:active{
  ...
  top: 2px;
  box-shadow: inset 0 1px 0 hsl(54, 100%, 50%),
              0 2px 0 hsl(54, 100%, 16%),
              0 3px 0 hsl(54, 100%, 14%),
              0 4px 0 hsl(54, 100%, 12%),
              0 5px 0 hsl(54, 100%, 10%),
              0 6px 0 hsl(54, 100%, 8%),
              0 7px 0 hsl(54, 100%, 6%);
}
```

# 3. 渐变按钮1

![Gradient Button 1](http://lc-PX2vd1LW.cn-n1.lcfile.com/ead81e2eb909a81a1d59/gradient%20button%201.gif)

[Gradient Button 1](https://codepen.io/mudontire/pen/jgmmWw)

提到渐变色大家一般都会想到`background-image`属性可以设置为`linear-gradient`以呈现渐变色的背景，事实上`linear-gradient`的类型属于`<image>`的一种，所以凡是可以使用图片的属性都可以用`linear-gradient`代替，包括`border-image`。实现这个按钮的关键在于实现一个渐变色和边框，而且当鼠标悬浮的时候边框和背景融为一体。

首先，我们实现渐变色的边框。

**CSS：**
```
.gradient-button-1{
  ...
  border:solid 10px transparent;
  border-image: linear-gradient(to top right, orangered, yellow);
}
```

**效果：**

![just border image](http://lc-PX2vd1LW.cn-n1.lcfile.com/554fdfbf554b5b81857d/border-image.png)

很奇怪，只有四个顶点有图像。这是因为`border-image-slice`默认为100%，所以导致四条边线上并没有分配背景图像。`border-image-slice`的用法参考 [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/border-image-slice)，简而言之就是用四条（上下、左右各两条平行线，想象一下九宫格火锅。。）将图片切割成9块，在border的对应区域显示对应的图像切片，而`border-image-slice`的值就是那四条线的偏移量。这下大家应该能理解偏移量为100%的时候只有四个顶点上才有图片了吧。

所以我们需要调整`border-image-slice`的值，鉴于`border-image-source`为`linear-gradient`，我们需要将`border-image-slice`的值设置为`1`（表示四条线的偏移量都为1px）才能显示连续的渐变色背景：

**CSS：**

```
.gradient-button-1{
  ...
  border-image-slice: 1;
}
```

最后，我们只需要在鼠标悬浮的时候用渐变色填充按钮内部就行了，此处有两种实现，用 `background-image` 或者 将`border-image-slice` 设置为 `fill` （表示填充border中间的区域）：

**CSS：**

```
.gradient-button-1:hover{
  color: white;
  background-image: linear-gradient(to top right, orangered, yellow);

  /* 或者 */

  border-image-slice: 1 fill;
}
```