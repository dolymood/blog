## CSS盒模型

CSS盒模型描述了在文档树中的元素通过‘可视化格式模型’进行布局的矩形盒子。

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

Margin属性指定了一个盒子的margin area的大小。'margin'简写属性（复合属性）同时设定4个边界，而其他margin属性只能设置他们的那一边。这些属性可以应用到所有的元素上去，但是垂直方向上的margin在非替换的inline行内元素上时不会又效果的。

margin值可以是如下几个值：

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

这些属性设置一个盒子的margin的top, right, bottom, left的值。

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

当两个或者多个margin发生折叠的时候，折叠margin结果会是取其中的margin较大的值。当出现负margin的情况下，取得是其中绝对值较大的，然后从0位置，负向偏移（其实就是正margin + 负margin的结果）。如果没有正的margin，那么绝对值的较大值就是0，然后开始减（其实也就是两者绝对值较大的值）。

如果一个盒子的top margin和bottom margin是相邻的，那么他的margin可能会通过它发生折叠（折叠穿透）。在这种情况下，这个元素的位置取决于他和其他折叠了的元素之间的关系了。

* 如果这个元素的margin和他的父级的top margin发生折叠，那么这个盒子的top border的边界和父级的相同。

* 其他情况，不管这个元素的父元素参不参与margin折叠或者只有父元素的bottom margin是参与的。这个元素的top border边界的位置和其原位置是相同的，如果这个元素有一个非0的bottom border。

注意，发生折叠穿透的元素的位置对于其他和他发生折叠的元素的位置没有任何影响。top border的边界位置仅仅是他们的后代元素布局所需要的。


### padding属性：'padding-top', 'padding-right', 'padding-bottom', 'padding-left'和'padding'

padding属性指定了一个盒子的padding area的大小。'padding'简写属性（复合属性）同时设定4个边界，而其他padding属性只能设置他们的那一边。

padding值可以是如下几个值：

* __<宽度值（length）>__
	
	指定的一个固定值。

* __<百分比（percentage）>__

	百分比的计算是相对于其包含块的width的。即使是padding-top和padding-bottom依旧是相对于包含块的宽度计算的。_如果其包含块的宽度取决于元素自身，那么在CSS2.1中，结果就是undefined。

不像margin属性，padding的值不可以是负的。和margin属性很像的是百分比的计算需要依赖于他的包含块的宽。

* __'padding-top', 'padding-right', 'padding-bottom', 'padding-left'__

	_值：_ <padding值> 或者 inherit

	_初始化：_ 0

	_应用在：_ 除了table-row-group, table-header-group, table-footer-group, table-row, table-column-group 和 table-column之外的一切元素

	_可继承：_ 不可以

	_百分比：_ 相对于包含块的宽度

	_媒介：_ 可见媒介

	_计算值：_ 特殊的百分比或者绝对的值

这些属性设置一个盒子的padding的top, right, bottom, left的值。

```css
blockquote { padding-top: 0.3em }
```


* _'padding'_

	_值：_ <padding值>{1,4}（1到4个） 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 除了table-row-group, table-header-group, table-footer-group, table-row, table-column-group 和 table-column之外的一切元素

	_可继承：_ 不可以

	_百分比：_ 相对于包含块的宽度

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

padding属性是'margin-top', 'margin-right', 'margin-bottom', 'margin-left'的简写形式。

如果只有一个值的话，他被应用到所有的边上。如果有2个值的话，top和bottom被设置成第一个值，而right和left被设置成第二个值。如果有3个值的话，top被设置为第一个值，right被设置为第二个值，bottom被设置为第4个值，left的值被设置为和right的值一样。如果有4个值的话，他们分别应用到top right bottom left上。

padding区域的颜色或者图由'background'属性指定：

```css
h1 { 
  background: white; 
  padding: 1em 2em;
} 
```

_上边的例子指定了一个'1em'的垂直方向的padding（padding-top和padding-bottom）和'2em'的水平方向的padding（padding-right, padding-left）。'em'单位是一个相对于元素font size的单位：1em相当于和当前元素使用的font size的值是一样的。_

### border属性：

border属性指定了一个盒子的border area的大小，颜色以及样式。这个属性可以应用到所有的元素上。

#### border width：'border-top-width', 'border-right-width', 'border-bottom-width', 'border-left-width'和'border-width'

border width属性指定了一个盒子的border area的大小。

border width值可以是如下几个值：

* __thin__
	
	一个窄的border。

* __medium__
	
	一个中等的border。

* __thick__
	
	一个厚的border。

* __<数值（length）>__

	border的厚度有一个显式的值。显式的border宽度不能是负的。

前面的3个值被解释成的大小是由用户代理决定的。然而，他们之间的关系必须是这样的：

> 'thin' <='medium' <= 'thick'

未来，这些宽必须通过文档来指定他们的大小（常量）。

* __'border-top-width', 'border-right-width', 'border-bottom-width', 'border-left-width'__

	_值：_ <border值> 或者 inherit

	_初始化：_ medium

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_绝对的值；如果border的样式是'none'或者'hidden'的话，计算值就是0

这些属性设置一个盒子的border的top, right, bottom, left的值。

* _'border-width'_

	_值：_ <border宽>{1,4}（1到4个） 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

border-width属性是'border-top-width', 'border-right-width', 'border-bottom-width', 'border-left-width'的简写形式。

如果只有一个值的话，他被应用到所有的边上。如果有2个值的话，top和bottom被设置成第一个值，而right和left被设置成第二个值。如果有3个值的话，top被设置为第一个值，right被设置为第二个值，bottom被设置为第4个值，left的值被设置为和right的值一样。如果有4个值的话，他们分别应用到top right bottom left上。

下边的示例中，注释部分表明了border的top, right, bottom, 和 left的值：

```css
h1 { border-width: thin }                   /* thin thin thin thin */
h1 { border-width: thin thick }             /* thin thick thin thick */
h1 { border-width: thin thick medium }      /* thin thick medium thick */
```

#### border color：'border-top-color', 'border-right-color', 'border-bottom-color', 'border-left-color'和'border-color'

border color属性指定了一个盒子的border的颜色。

* __'border-top-color', 'border-right-color', 'border-bottom-color', 'border-left-color'__

	_值：_ <color值> 或者 transparent 或者 inherit

	_初始化：_ color属性的值

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 如有color属性的话，那么就是color的计算值；其他情况就是指定的值

* _'border-color'_

	_值：_ [<color值> 或者 transparent]{1,4}（1到4个） 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

border-color属性设置四个border的颜色。值具有下边的意义：

* __<color>__
	
	指定一个color值（颜色）

* __transparent__
	
	border就是透明的（尽管有可能有宽度）

border-color属性可以有1到4组值，就像border-width一样他们分别设置在不同的边上。

如果一个元素的border颜色没有通过border属性指定，用户代理必须使用这个元素的color属性的值作为border颜色的计算值。

_下边的示例中，border是一个实心的(solid)的黑色线：_

```css
p { 
  color: black; 
  background: white; 
  border: solid;
}
```

#### border style：'border-top-style', 'border-right-style', 'border-bottom-style', 'border-left-style'和'border-style'

border style属性指定了一个盒子的border的线的样子（solid, double, dashed等等）。

border style可以是如下几个值：

* __none__
	
	没有border；border宽度的计算值值0。

* __hidden__
	
	和'none'一样除了在解决table元素的‘border冲突解决方案’。

* __dotted__
	
	点状的border。

* __dashed__
	
	一系列短线状的border。

* __solid__
	
	就是实心的线状。

* __double__
	
	双线状border。

* __groove__

	3D 凹槽边框。

* __ridge__

	3D 垄状边框。

* __inset__

	3D inset 边框。

* __outset__

	3D outset 边框。

所有的border都是渲染在盒子的背景色之上的。border的样式中 'groove', 'ridge', 'inset', 和 'outset'的颜色的值取决于当前元素的border color属性，但是用户代理可以选择他们自己的关于计算颜色的实际使用值的算法。例如，如果border-color的值有silver的话， 那么一个用户代理可以利用白色到黑色的渐变来渲染倾斜的border颜色。

* __'border-top-style', 'border-right-style', 'border-bottom-style', 'border-left-style'__

	_值：_ <border-style值> 或者 inherit

	_初始化：_ none

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定的值一样

* _'border-style'_

	_值：_ <border-style>{1,4}（1到4个） 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

border-style属性设置四个border的样式。他可以有1到4组值，就像border-width一样他们分别设置在不同的边上。

如果只有一个值的话，他被应用到所有的边上。如果有2个值的话，top和bottom被设置成第一个值，而right和left被设置成第二个值。如果有3个值的话，top被设置为第一个值，right被设置为第二个值，bottom被设置为第4个值，left的值被设置为和right的值一样。如果有4个值的话，他们分别应用到top right bottom left上。

```css
\#xy34 { border-style: solid dotted }
```

_上边的例子中，水平方向的border样式是solid的 而垂直方向的将会是dotted。_

由于border style的默认值是none，只有border style设置了border才会可见。

#### border简写属性：'border-top', 'border-right', 'border-bottom', 'border-left'

* __'border-top', 'border-right', 'border-bottom', 'border-left'__

	_值：_ [<border-width> || <border-style> || <'border-top-color'>] 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

这是一个设置一个盒子的上top、右right、下bottom、 左left 的四个边的宽（width）、样式（style）以及颜色（color）的简写属性。

```css
h1 { border-bottom: thick solid red }
```

_上边的规则会将H1元素的下边的border设置了width style以及color的样式。缺省的值将会被设置为他的默认值（初始值）。由于下边的这个示例中没有指定border的颜色，所以border的颜色值将和元素的color属性值相同：_

```css
H1 { border-bottom: thick solid }
```

* __border__

	_值：_ [<border-width> || <border-style> || <'border-top-color'>] 或者 inherit

	_初始化：_ 参见单独的属性

	_应用在：_ 一切元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 参见单独的属性

border属性是给一个盒子设置相同width, color以及style的简写属性。不像margin和padding简写属性，border属性不能呢过针对于4边单独设置值。如果想这样做的话，必须用到其他的border属性。

_例如，下边的第一条规则和其后的四条规则是同等效的：_

```css
p { border: solid red }
p {
  border-top: solid red;
  border-right: solid red;
  border-bottom: solid red;
  border-left: solid red
}
```

所以在某种程度上来说，功能重复的属性去指定他们的顺序是很重要的。

_考虑下边的示例：_

```css
blockquote {
  border: solid red;
  border-left: double;
  color: black;
}
```

_在上边的示例中，左border的颜色会是黑色的，其他边的颜色是红色的。这是因为border-left设置了width、style以及color。由于border-left上并没有设置颜色值，所以说将会取color属性的值。实际上这和color属性写在了border-left的后边是没有啥关系的。_

### 在不同方向（双向）的上下文中的行内元素的盒模型

对于每一个线盒（line box）来说，用户代理必须以视觉顺序（而不是逻辑顺序）为每一个元素生成行内盒（inline boxes），并且去渲染margin，border以及padding。

当这个元素的direction属性值是ltr的时候，生成的的盒子的最左边就是第一个线盒中的元素的left margin，left border以及left padding，并且生成的盒子最右边就是最后一个线盒中的元素的right margin，right border以及right padding。

当这个元素的direction属性值是rtl的时候，生成的盒子最右边就是第一个线盒中的元素的right margin，right border以及right padding，并且生成的的盒子的最左边就是最后一个线盒中元素的left margin，left border以及left padding。