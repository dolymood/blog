## CSS 颜色color和背景background

CSS属性允许作者指定一个元素的前景色color和背景background。背景可以是颜色也可以是图片。background属性允许作者定位背景图、重复他以及声明他是否应该相对于viewport规定或者随着文档滚动。


### 前景色：color属性

* __color__
	
	_值：_  <颜色color> | inherit

	_初始化：_ 取决于用户代理

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性描述了一个元素文本内容的前景色。下边的是不同的方式来指定值为红色：

```css
em { color: red }              /* predefined color name */
em { color: rgb(255,0,0) }     /* RGB range 0-255   */
```

<!--more-->

