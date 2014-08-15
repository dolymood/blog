## CSS 文本text

text相关的属性会在下边的段落中定义，他们影响了字符characters，空格spaces，单词words以及段落paragraphs的可视化表现。

### 缩进：text-indent属性

* __text-indent__
	
	_值：_  <长度length> | <百分比percentage> | inherit

	_初始化：_ 0

	_应用在：_ 块容器

	_可继承：_ 可以

	_百分比：_ 相对于包含块的宽度width

	_媒介：_ 可见媒体

	_计算值：_ 指定的百分比或者绝对长度

<!--more-->

这个属性指定了再一个块容器中第一行文本的缩进值。更详细来说，他指定了流入块的第一个行盒的第一个盒的缩进值。这个盒根据行盒的左边界（对于由右到左的布局来说就是右边界）来缩进的。用户代理必须以空白的空格渲染缩进。

text-indent只会影响一个元素的第一个格式化的行。例如，一个匿名块盒的第一行（如果他是他的父元素的第一个孩子的话）才会受到影响。

他的值有如下含义：

* __<长度length>__

	缩进是一个固定的长度。

* __<百分比percentage>__

	缩进是其包含块的宽度的百分比。

text-indent的值可以是负的，但是可能会有一些实现上的独特限制。如果text-indent的值是负的或者超出了块（上边描述的第一个盒）的宽度可以溢出这个块。overflow的值会影响这个文本溢出块之外的部分是否可见。

_下边的例子导致了3em的文本缩进。_

```css
p { text-indent: 3em }
```

###对齐：text-align属性

* __text-align__
	
	_值：_  left | right | center | justify | inherit

	_初始化：_ 如果direction是ltr的话就是left，如果direction是rtl的话就是right

	_应用在：_ 块容器

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 初始值或者指定的值

这个属性描述了再一个块容器中的行内级内容如何对齐。他的值有如下含义：

* __left, right, center, justify__
	
	left，right，center和justify分别对齐文本，在行内格式化上下文IFC段落中描述了。

文本块是一个行盒的层叠。在left、right和center的情况下，这个属性指定了行内级盒中的每一个行盒怎么相对于这个行盒的左边界和右边界对齐；对齐不受viewport的影响。在justify的情况下，这个属性指定了在可能的情况下，行内级盒通过扩大或者缩小其内容来和这个行盒的两端齐平；否则的话以初始值对齐。

如果一个元素的white-space的计算值是pre或者pre-wrap的话，那么无论这个元素中文本内容的字形还是他的空白都不能够两端对齐。

_这个例子，注意由于text-align是可继承的，所有的带有class是important的DIV元素内部的块级元素中的行内内容居中对齐。_

```css
div.important { text-align: center }
```

### 装饰品

#### 下划线、上划线、引人注目以及闪烁：text-decoration属性

* __text-decoration__
	
	_值：_  none | [ underline || overline || line-through || blink ] | inherit

	_初始化：_ none

	_应用在：_ 所有元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定的值一样

这个属性描述了一个元素用其颜色增添到文本上的装饰品。当指定到了或者传播到了（继承）一个行内元素上时，他影响这个元素生成的所有盒，且能够进一步传播到拆分这个行内元素的任何在普通流中的块级盒。但是，在CSS2.1中，如果装饰品传播到了块级table的话，这种情况是未定义的。对于创建IFC的块容器来说，这个声明会传播到一个包含着这个块容器的在普通流中的所有的行内级孩子的匿名行内元素上。对于其他元素来说，他会传播到任何的在普通流中的孩子。注意文本装饰品不能传播到浮动和绝对定位后代上的，也不会传播到原子行内级后代的内容上，例如行内级块和行内级table。

下划线，上划线以及横穿线只能应用到文本上：margin。border和padding是跳过去的。用户代理不能给不是文本的内容渲染文本装饰品。例如，图片和行内盒不能有下划线。

_注意：如果一个元素E有visibility:hidden以及text-decoration:underline，那么下划线是不可见的。然而，在CSS2.1中没有指定E元素的孩子们的下划线是否可见。_

```css
<span style="visibility: hidden; text-decoration: underline">
 <span style="visibility: visible">
  underlined or not?
 </span>
</span>
```

后代元素上text-decoration属性对于其祖先没有任何影响。在确定文本下换线的位置和粗细时，用户代理可以考虑后代元素的font尺寸以及其下划线的明显性，但是必须对每一行用同样的下划线和粗细程度。相对定位一个后代元素会影响所有的文本的装饰品就如同一项文本一样；但是他不会影响那一行上的装饰品位置的计算。

他的值有如下含义：

* __none__

	没有任何的文本装饰。

* __underline__

	每一行文本都有下划线。

* __overline__

	每一行文本上边都有一条线。

* __line-through__

	每一行都有一条线横穿过文本中间。

* __blink__

	文本闪烁。

文本装饰品的颜色必须继承自这个设置了text-decoration属性的元素的color属性的值。即使装饰元素有不同的color属性，装饰品的颜色必须保持一致。

一些用户代理实现了text-decoration（通过传播到后代元素上的装饰品就像上边所描述的并不会一直保持不变的厚度和位置）。可以说这是因为CSS2中的宽松的措词而允许的。SVG1，CSS1-only，和CSS2-only的用户代理可能实现了老的模型但是却声称实现了CSS2.1的一部分。

_下边在HTML中的例子，作为超链接的a元素的文本内容会有下划线：_

```css
a:visited,a:link { text-decoration: underline }
```

_下边的样式表和文档片段：_

```
 blockquote { text-decoration: underline; color: blue; }
   em { display: block; }
   cite { color: fuchsia; }

   <blockquote>
    <p>
     <span>
      Help, help!
      <em> I am under a hat! </em>
      <cite> —GwieF </cite>
     </span>
    </p>
   </blockquote>
```

_blockquote元素的下划线会传播到一个围绕着span元素的匿名的行内元素上，导致文本"Help, help!"有下划线，颜色也是继承自blockquote元素上的。em块中的`<em>text</em>`同样也是带着下划线的，因为他是下划线传播到的一个在普通流中的块。文本最下边的线是fuchsia颜色的，但是他的下划线依旧是来自于匿名行内元素的蓝色。_

![示意图](http://www.w3.org/TR/CSS21/images/underline-example.png)

_这个示意图展示了上边例子中涉及到的盒。圆角的浅绿色的线代表队额是匿名的行内元素，圆角的蓝色的线代表了span元素，且橙色的线代表了块。_

### 字母和单词间距：letter-spacing和word-spacing属性