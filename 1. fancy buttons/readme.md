# CSS实现8种炫酷按钮

在各种UI组件库大行其道的今天，大家已经很少自己用CSS去实现一些效果了，久而久之CSS的水平也越来越退步，所以有空还是得练练。今天给大家分享8种炫酷按钮的CSS实现。

# 1. 3D按钮1

![3D Button 1 Gif](http://lc-PX2vd1LW.cn-n1.lcfile.com/4fca21c8987d856cb121/3d-button-1.gif)

现在的主流是扁平化的设计，拟物化的设计比较少见了，所以我们仅从技术角度去分析如何实现这个3D按钮（文中只列出各种实现的关键代码，完整代码请参考CodePen）。

该按钮的立体效果主要由按钮多出的左、下两个侧面衬托出来，我们可以使用`box-shadow`模拟出这两个侧面：

**HTML：**

```
<button class="button-3d-1">3D Button 1</button>
```

**CSS:**

```
.button-3d-1{
  position: relative;
  background: orangered;
  border: none;
  color: white;
  padding: 15px 24px;
  font-size: 1.4rem;
  outline: none;
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

最后，我们需要重新计算左上、右下两个三角形的尺寸和位置，以适应按下后上下缺口的变小：

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

**最终效果：**[CodePen 3D Button 1](https://codepen.io/mudontire/pen/aeJQqB)


# 2. 3D按钮2

![3D Button 2 Gif](http://lc-PX2vd1LW.cn-n1.lcfile.com/35463e1e209c26cb05a3/3d-button-2.gif)

对于这种圆柱形的按钮，思路和上面矩形3D的按钮类似，也是通过`box-shadow`构造侧面呈现立体感。为了使效果更加真实，侧面的颜色呈现渐变效果，越往下颜色越深，因此我们可以通过叠加多层`box-shadow`来实现：

**HTML：**

```
<button class="button-3d-2">Circle!</button>
```

**CSS：**

```
.button-3d-2{
  position: relative;
  background: #ecd300;
  background: radial-gradient(hsl(54, 100%, 50%), hsl(54, 100%, 40%));
  font-size: 1.4rem;
  text-shadow: 0 -1px 0 #c3af07;
  color: white;
  border: 1px solid hsl(54, 100%, 20%);
  border-radius: 100%;
  height: 120px;
  width: 120px;
  z-index: 4;
  outline: none;

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
**最终效果：**[CodePen 3D Button 2](https://codepen.io/mudontire/pen/KOmWBN)

# 3. 渐变按钮1

![Gradient Button 1](http://lc-PX2vd1LW.cn-n1.lcfile.com/ead81e2eb909a81a1d59/gradient%20button%201.gif)

提到渐变色大家一般都会想到`background-image`属性可以设置为`linear-gradient`以呈现渐变色的背景，事实上`linear-gradient`的类型属于`<image>`的一种，所以凡是可以使用图片的属性都可以用`linear-gradient`代替，包括`border-image`。实现这个按钮的关键在于实现一个渐变色和边框，而且当鼠标悬浮的时候边框和背景融为一体。

首先，我们实现渐变色的边框。

**HTML：**

```
<button class="gradient-button-1">Gradient Button 1</button>
```

**CSS：**
```
.gradient-button-1{
  position: relative;
  z-index: 1;
  display: inline-block;
  padding: 20px 40px;
  font-size: 1.4rem;
  box-sizing: border-box;
  background-color: #e7eef1;
  color: orangered;
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

**最终效果：**[CodePen Gradient Button 1](https://codepen.io/mudontire/pen/jgmmWw)

# 4. 渐变按钮2

![Gradient Button 2](http://lc-PX2vd1LW.cn-n1.lcfile.com/ab3324814835912d35ea/gradient%20button%202.gif)

与上一种按钮类似，只不过颜色是逐渐变透明，实现也类似：

**HTML：**
```
<button class="gradient-button-2">Gradient Button 2</button>
```

**CSS：**

```
.gradient-button-2{
  ...
  border:solid 4px transparent;
  border-image: linear-gradient(to right, orangered, transparent);
  border-image-slice: 1;
}

.gradient-button-2:hover{
  color: white;
  background-image: linear-gradient(to right, orangered, transparent);
  border-right: none;
}
```

> 注意点：当hover的时候需要设置 `border-right: none`，否则右侧边框无法呈现消失的状态。

**最终效果：**[CodePen Gradient Button 2](https://codepen.io/mudontire/pen/GVEMJV)

# 5. 动画按钮1

![Animted Button 1](http://lc-PX2vd1LW.cn-n1.lcfile.com/220b77e4c5514baf21e1/animated%20button%201.gif)

给按钮加上一个动态背景的思路是：先找一个可以repeat的背景图（可以去 [siteorigin](http://bg.siteorigin.com/) 生成），然后使用`keyframe`自定义一段动画，当鼠标悬浮在按钮上的时候运行该动画：

**HTML：**

```
<button class="animated-button-1">Animated Button 1</button>
```

**CSS：**

```
.animated-button-1{
  position: relative;
  display: inline-block;
  padding:  20px 40px;
  font-size: 1.4rem;
  background-color: #00b3b4;
  background-image: url("wave.png");
  background-size: 46px 26px;
  border: 1px solid #555;
  color: white;
  transition: all ease 0.3s;
}

.animated-button-1:hover{
  animation: waving 2s linear infinite;
}

@keyframes waving{
  from{
    background-position: 0 0;
  }
  to{
    background-position: 46px 0;
  }
}
```

> 注意点：`background-position` 水平方向的值需要等于背景图片的宽度或其整数倍，这样动画循环播放的时候首尾才能平稳过渡。

**最终效果：**[CodePen Animted Button 1](https://codepen.io/mudontire/pen/KOqXRX)

# 6. 动画按钮2

![Animated Button 2](http://lc-PX2vd1LW.cn-n1.lcfile.com/47f0c740db50e8929905/animated%20button%202.gif)

该按钮的实现思路是：用 `::after` 伪元素创建右侧的箭头，使用绝对定位固定在按钮右侧，静止状态下通过设置`opacity: 0`隐藏，当鼠标悬浮时，增大按钮的`padding-right`，同时增加箭头的不透明度，并将其位置往左移动：

**HTML：**

```
<button class="animated-button-2">Animated Button 2</button>
```

**CSS：**

```
.animated-button-2{
  position: relative;
  padding:  20px 40px;
  font-size: 1.4rem;
  background-color: #00b3b4;
  background-size: 46px 26px;
  border: 1px solid #555;  
  color: white;
  transition: all ease 0.3s;
}

.animated-button-2::after{
  position: absolute;
  top: 50%;
  right: 0.6em;
  transform: translateY(-50%);
  content: "»";
  font-size: 1.2em;
  transition: all ease 0.3s;
  opacity: 0;
}

.animated-button-2:hover{
  padding: 20px 60px 20px 20px;
}

.animated-button-2:hover::after{
  right: 1.2em;
  opacity: 1;
}
```

**最终效果：**[CodePen Animated Button 2](https://codepen.io/mudontire/pen/gVRXLW)

# 7. 开关按钮1

![Switch Button 1](http://lc-PX2vd1LW.cn-n1.lcfile.com/6ad3b574f3f8397ad654/switch%20button%201.gif)

这算是一个挺常见的开关按钮，它的实现思路是：

1. 通过一个checkbox记录开关的状态，并隐藏该checkbox; 
1. 使用一个 `<label>` 作为整个按钮容器，并通过 `for` 属性与checkbox关联，这样点击按钮的任何地方都能改变checkbox的状态；
1. 使用一个 `<span>` 作为按钮可视的部分，并作为 checkbox 的相邻元素，这样通过 checkbox的伪类选择器 `:checked` 和相邻选择器 `+` 选中 `<span>`并显示不同状态下的内容。

**HTML：**

```
<label for="toggle1" class="toggle1">
  <input type="checkbox" id="toggle1" class="toggle1-input">
  <span class="toggle1-button"></span>
</label>
```

**CSS：**

```
.toggle1{
  font-family: Arial, Helvetica, sans-serif;
  vertical-align: top;
  width: 80px;
  display: block;
  margin: 100px auto;
}

.toggle1-input{
  display: none;
}

.toggle1-button{
  position: relative;
  display: inline-block;
  font-size: 1rem;
  line-height: 20px;
  text-transform: uppercase;
  background-color: #f2395a;
  border: 1px solid #f2395a;
  color: white;
  width: 100%;
  height: 30px;
  transition: all 0.3s ease;
  cursor: pointer;
}


.toggle1-button::before{
  position: absolute;
  top: 5px;
  left: 38px;
  content: "off";
  display: inline-block;
  height: 20px;
  padding: 0 3px;
  background: white;
  color: #f2395a;
  transition: all 0.3s ease;
}

.toggle1-input:checked + .toggle1-button{
 background: #00b3b4; 
 border-color: #00b3b4;
}

.toggle1-input:checked + .toggle1-button::before{
  left: 5px;
  content: 'on';
  color: #00b3b4;
}
```

> 注意点：`<label>` 的 `for` 属性的作用；`:checked` 伪类的使用；`+` 相邻选择器的使用。

**最终效果：**[CodePen Switch Button 1](https://codepen.io/mudontire/pen/MNoroK)

# 8. 开关按钮2

![Switch Button 2](http://lc-PX2vd1LW.cn-n1.lcfile.com/ca358d2cc32ba4d8b8c3/switch%20button%202.gif)


与开关按钮1类似，动画效果上更简单，只要切换颜色就行了：

**HTML：**

```
<label for="toggle2" class="toggle2">
  <input type="checkbox" id="toggle2" class="toggle2-input">
  <span class="toggle2-button">Click to activate</span>
</label>
```

**CSS：**

```
.toggle2{
  font-family: Arial, Helvetica, sans-serif;
  font-size: 0.8rem;
  display: inline-block;
  vertical-align: top;
  margin: 0 15px 0 0;
}

.toggle2-input{
  display: none;
}

.toggle2-button{
  position: relative;
  display: inline-block;
  line-height: 20px;
  text-transform: uppercase;
  background: white;
  color: #aaa;
  border: 1px solid #ccc;
  padding: 5px 10px 5px 30px;
  transition: all 0.3s ease;
  cursor: pointer;
}

.toggle2-button::before{
  position: absolute;
  top: 10px;
  left: 10px;
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #ccc;
  content: "";
  transition: all 0.3s ease;
  border-radius: 50%;
}

.toggle2-input:checked + .toggle2-button{
  background: #00b3b4;
  border-color: #00b3b4;
  color: white;
}

.toggle2-input:checked + .toggle2-button::before{
  background: white;
}
```

**最终效果：**[CodePen Switch Button 2](https://codepen.io/mudontire/pen/gVRvpV)

> 本文参考：https://youtu.be/pmKyG3NBY_k