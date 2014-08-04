## CSS盒模型

### 盒尺寸

每一个盒子都有一个内容区域（content area）和周围的可选的内边距（padding），边框（border） 和外边距（margin areas）；每一个区域的大小按照下边的属性指定。下边的图标展示了这些区域之间的关系以及margin，border以及padding所用到的术语：

<!--more-->

_PS：为了方便，后边这几个词的均以英文代替中文_

![content padding borders和margins之间的关系](http://www.w3.org/TR/CSS21/images/boxdim.png)

margin, border和padding可以被拆分为top, right, bottom 和 left 部分。

这四个区域（content, padding, border, margin）的每一个边界称作"edge"边界，因此每一个盒有4个边界：

* __content边界或者内边界__

	content边界围绕着由盒子的宽高（width, height）组成的矩形，这个尺寸往往由元素渲染内容所决定。这四个边界组成的矩形框就是该元素的content box。

* __padding边界__
	
	padding边界围绕着盒子的padding。如果padding为0，padding边界和content边界一样。这四个边界组成的矩形框就是该元素的padding box。

* __border边界__
	
	border边界围绕着盒子的border。如果border为0，border边界和padding边界一样。这四个边界组成的矩形框就是该元素的border box。

* __margin边界或者外边界__
	
	margin边界围绕着盒子的margin。如果margin为0，margin边界和border边界一样。这四个边界组成的矩形框就是该元素的margin box。

每一个边界都可以分解成top, right, bottom, left四个边界。

一个盒子的content区域大小——content width和content height——取决于一些因素：元素具有width和height属性、盒box包含文本或者其他盒子、盒子是table等等。盒子的宽高将在后边的‘可视化格式模型细节’一章中详细讨论。

一个元素的content, padding, borders区域的背景background样式通过其background属性指定。margin背景往往是透明的。

### margins, paddings, borders示例

这个例子描述了margins, paddings, borders之间是如何相互影响的。HTML文档：

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
  <HEAD>
    <TITLE>Examples of margins, padding, and borders</TITLE>
    <STYLE type="text/css">
      UL { 
        background: yellow; 
        margin: 12px 12px 12px 12px;
        padding: 3px 3px 3px 3px;
                                     /* No borders set */
      }
      LI { 
        color: white;                /* text color is white */ 
        background: blue;            /* Content, padding will be blue */
        margin: 12px 12px 12px 12px;
        padding: 12px 0px 12px 12px; /* Note 0px padding right */
        list-style: none             /* no glyphs before a list item */
                                     /* No borders set */
      }
      LI.withborder {
        border-style: dashed;
        border-width: medium;        /* sets border width on all sides */
        border-color: lime;
      }
    </STYLE>
  </HEAD>
  <BODY>
    <UL>
      <LI>First element of list
      <LI class="withborder">Second element of list is
           a bit longer to illustrate wrapping.
    </UL>
  </BODY>
</HTML>
```

结果示意图：

![padding borders和margins 例子的示意图](http://www.w3.org/TR/CSS21/images/boxdimeg.png)

注意的是：

* 每一个li盒子的content width是上下计算的；每一个li的包含块是由ul元素确定的。

* 每一个li盒子的magin box的高取决于他的content height加上上下padding, borders和margins。注意每一个li盒子之间的垂直margins是折叠collapse的（也就是常说的外边距折叠）。

* 每一个li盒子的右padding被设置为了0,。效果可见图上的的下图。

* 每一个li盒子的magins是透明的——margin背景往往是透明的——因此ul的padding和content区域的背景色（黄色）通过了他们。

* 第2个li元素指定了一个dashed效果的border。

### Margin属性：'margin-top', 'margin-right', 'margin-bottom', 'margin-left'和'margin'

Margin属性指定了一个盒子的magin area的大小。'margin'简写属性（复合属性）同时设定4个边界，而其他margin属性只能设置他们的那一边。这些属性可以应用到所有的元素上去，但是垂直方向上的margin在非替换的inline行内元素上时不会又效果的。

magin值可以是如下几个值：

* __<宽度值（length）>__
	
	指定的一个固定值。

* __<百分比（percentage）>__

	百分比的计算是相对于其包含块的width的。_注意：margin-top和margin-bottom依旧是相对于包含块的宽度计算的。_如果其包含块的宽度取决于元素自身，那么在CSS2.1中，结果就是undefined。

* __auto__
	
	请看后边关于magins的计算值部分。

margin的值可以是负值，但是可能会有特定于具体实现上的一些限制。

* __'margin-top', 'margin-bottom'__

	_值：_ <margin值> 或者 inherit

	_初始化：_ 0

	_应用在：_ 除了display类型是table, table-caption以及inline-table之外的一切元素

	_可继承：_ 不可以

	_百分比：_ 相对于包含块的宽度

	_媒介：_ 可见媒介

	_计算值：_ 特殊的百分比或者绝对的值

_这两个在非替代行内元素上无效_

* __'margin-right', 'margin-left'__

	_值：_ <margin值> 或者 inherit

	_初始化：_ 0

	_应用在：_ 除了display类型是table, table-caption以及inline-table之外的一切元素

	_可继承：_ 不可以

	_百分比：_ 相对于包含块的宽度

	_媒介：_ 可见媒介

	_计算值：_ 特殊的百分比或者绝对的值

这些属性设置一个盒子的top, right, bottom, left的margin。

```css
h1 {margin-top:2em}
```


* _'margin'_

	_值：_ <margin值>{1,4}（1到4个） 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 除了display类型是table, table-caption以及inline-table之外的一切元素

	_可继承：_ 不可以

	_百分比：_ 相对于包含块的宽度

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

margin属性是'margin-top', 'margin-right', 'margin-bottom', 'margin-left'的简写形式。

如果只有一个值的话，他被应用到所有的边上。如果有2个值的话，top和bottom被设置成第一个值，而right和left被设置成第二个值。如果有3个值的话，top被设置为第一个值，right被设置为第二个值，bottom被设置为第4个值，left的值被设置为和right的值一样。如果有4个值的话，他们分别应用到top right bottom left上。

```css
body { margin: 2em }         /* all margins set to 2em */
body { margin: 1em 2em }     /* top & bottom = 1em, right & left = 2em */
body { margin: 1em 2em 3em } /* top=1em, right=2em, bottom=3em, left=2em */
```

_上边例子的最后一条规则和下边的例子是等同的：_

```css
body {
  margin-top: 1em;
  margin-right: 2em;
  margin-bottom: 3em;
  margin-left: 2em;        /* copied from opposite side (right) */
}
```

#### 外边距折叠

在CSS中，相邻的两个或者多个盒子的margins可以合并成一个单独的margin。margin的这种合并方式称作‘折叠（collapse）’，并且因此所结合成的margin称作‘折叠外边距（collapsed margin）’。

相邻的盒在垂直方向上发生magin折叠，除了：

* 根元素盒不会发生margin折叠。

* 如果一个有间隙的（clearance）元素的top和bottom的margins是相邻的，他的top margin会和紧随其后的同级相邻margin发生折叠，但是margin的结果他的margin不和父级块的bottom margin发生折叠。

水平方向的margin永远不会折叠。

两个margin相邻，那么他们必须符合下边的条件：

* 他们必须在同一个块级格式化上下文（BFC）中，并且是在普通流中的块级盒子。

* 没有线盒（line boxes）、没有间隙（clearance）、没有padding并且没border分开他们（_注意：具有0高度的线盒是被忽略的_）。

* 他们都属于垂直相邻的盒边界，下面的几种形式中的一种：

	* 盒子的top margin和他的第一个在普通流中的子元素

	* 盒子的bottom margin和紧邻他的下一个在普通流中的兄弟元素

	* 在普通流中的最后一个子元素的bottom margin和他的父元素的bottom margin，前提是父元素的高度计算值不是'auto'

	* 一个没有创建新的块级格式化上下文并且min-height的计算值为0、height的计算值为0或者auto、且没有处于正常流中的孩子（所有的孩子都不在正常流中）的盒子的top和bottom margin（上、下外边距）。


一个折叠margin和另外一个margin是相邻的，如果他的任何的组margin（component margin）和那个margin都是相邻的。

_注意：相邻的margin不一定是兄弟关系也可以是父子关系或者祖先关系。_

_注意以上的规则意味着：_

_* 一个浮动盒子（floated box）和其他任何盒子之间的margin不会发生折叠（即使是浮动和他的普通流中的孩子）。_

_* 创建了新的块级格式化上下文（即定义了属性overflow且值不是visible）的元素和他的普通流中的孩子的margin不发生折叠。_

_* 绝对定位的盒子的不会发生折叠（即使和他们的在普通流中的孩子们）。_

_* inline-block的盒子不发生折叠（即使和他们的在普通流中的孩子们）。_

_* 普通流中的块级元素的bottom margin和他紧邻的下一个普通流中块级兄弟元素会发生折叠，除非他们的相邻有间隙（clearance）。_

_* 如果普通流中的一个块元素没有 border-top、padding-top，且其第一个浮动的块级子元素没有间隙，则该元素的top margin会与其普通流中的第一个块级子元素的top margin折叠。_

_* 如果一个在普通流中的块级盒子的height是auto并且他的min-height是0，且他没有bottom padding、bottom border，那么他和他的最后的在普通流中的块级子元素（他的bottom margin不和有间隙的top margin发生折叠）发生折叠。_

_* 如果一个在盒子的 min-height 属性为0，且没有上或下边框以及上或下内边距，且 height 为0或者 auto，且不包含行框，且其属于普通流的所有孩子的外边距都折叠了，则折叠其外边距。_

当两个或者多个margin发生折叠的时候，折叠margin结果会是取其中的margin较大的值。当出现负margin的情况下，取得是其中绝对值较大的，然后从0位置，负向偏移（其实就是减法）。如果没有正的margin，那么绝对值的较大值就是0，然后开始减。

如果一个盒子的top margin和bottom margin是相邻的，那么他的margin可能会通过它发生折叠（折叠穿透）。在这种情况下，这个元素的位置取决于他和其他折叠了的元素之间的关系了。

* 如果这个元素的margin和他的父级的top margin发生折叠，那么这个盒子的top border的边界和父级的相同。

* 其他情况，不管这个元素的父元素参不参与margin折叠或者只有父元素的bottom margin是参与的。这个元素的top border边界的位置和其原位置是相同的，如果这个元素有一个非0的bottom border。

注意，发生折叠穿透的元素的位置对于其他和他发生折叠的元素的位置没有任何影响。top border的边界位置仅仅是他们的后代元素布局所需要的。


### padding属性：'padding-top', 'padding-right', 'padding-bottom', 'padding-left'和'padding'




