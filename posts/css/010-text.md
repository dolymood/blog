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

这个属性指定了在一个块容器中第一行文本的缩进值。更详细来说，他指定了流入块的第一个行盒的第一个盒的缩进值。这个盒根据行盒的左边界（对于由右到左的布局来说就是右边界）来缩进的。用户代理必须以空白的空格渲染缩进。

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

* __letter-spacing__
	
	_值：_  normal | <长度length> | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ normal或者绝对长度

这个属性指定了文本字符见的间距。他的值有如下含义：

* __normal__

	对于当前font来说间距是正常的间距。这个值允许用户代理调整字符间的空间用来两端对齐文本。

* __<长度length>__
	
	这个值表示除了字符间默认的空间之外的字符间距。值可以是负的，但是这里可能会有一些实现上的特殊限制。用户代理为了两端对齐文本不能无限的增加或者减少字符间间隔。

字符间距算法是由用户代理决定的。

_这个例子中，在BLOCKQUOTE元素中的字符之间的间隔增加到了0.1em_

```css
blockquote { letter-spacing: 0.1em }
```

_在下边的例子中，用户代理不允许改变字符间间隔：_

```css
blockquote { letter-spacing: 0cm }   /* Same as '0' */
```

当两个字符之间的结果间隔和默认的间隔不一样时，用户代理不应该使用连字。

* __word-spacing__
	
	_值：_  normal | <长度length> | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ normal的话就是0；其他情况绝对长度

这个属性指定了单词之间的间距的表现。他的值有如下含义：

* __normal__

	正常的单词间间隔，由当前font以及（或者）UA定义。

* __<长度length>__

	这个值表示除了单词间默认的间隔之外的单词间间隔。值可以是负的，但是也有一些实现上的特殊限制。

单词间距算法是由用户代理决定的。单词间距也会受text-align的属性justify的影响。单词间距影响每一个空格space（U+0020）以及不间断空格non-breaking space（U+00A0），文本左边空白之后应用处理的规则。这个属性在其他的单词分隔word-separator字符上的效果是未定义的。然而一般环境中，零超前宽度zero advance width（例如用U+200B空格的零）的字符和固定宽fixed-width空格（例如U+3000和U+2000到U+200A）不受影响。

_这个例子中，在H1元素中的每个单词之间的word-spacing增加了1em。_

```css
h1 { word-spacing: 1em }
```

### 大小写：text-transform属性

* __text-transform__
	
	_值：_  capitalize | uppercase | lowercase | none | inherit

	_初始化：_ none

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个值控制一个元素中的文本的大小写效果。值有如下含义：

* __capitalize__

	将每一个单词的第一个字符大写；其他不受影响。

* __uppercase__

	将每一个单词的每一个字符大写。

* __lowercase__

	将每一个单词的每一个字符小写。

* __none__

	没有任何效果。

实际上每一种情况的变换取决于书写的语言的。

_在这个例子中，在H1中所有的文本都被转换为大写文本。_

```css
h1 { text-transform: uppercase }
```

### 空格：white-space属性

* __white-space__
	
	_值：_  normal | pre | nowrap | pre-wrap | pre-line | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性声明了元素内部的空格如何处理。他的值有如下含义：

* __normal__

	这个值直接由用户代理合并空格序列，且在充满行盒的时候必要的换行。

* __pre__

	这个值阻止了用户代理合并空格序列。行只会在保留的换行字符那里断掉。

* __nowrap__

	这个值和normal一样合并空格，但是抑制文本内的换行。

* __pre-wrap__

	这个值阻止了用户代理合并空格序列。行会在保留的换行字符那里以及充满行盒的时候必要的断掉。

* __pre-line__

	这个值直接由用户代理合并空格序列。行会在保留的换行字符那里以及充满行盒的时候必要的断掉。

源文档中的换行能以回车（U+000D）、换行（U+000A）或者全部（U+000D U+000A）或者其他的标示文档开始和结尾段（例如SGML RECORD-START and RECORD-END符号）的机制代表。CSS的white-space处理模型假定所有的换行都被标准化了。识别其他的换行表现的UA必须应用空格的处理规则就好像他是正常化的。如果文档语言没有指定新的换行规则的话，在文档文本中的每一个回车（U+000D）和CRLF序列（U+000D U+000A）都被当做单独的换行符。这个默认的标准化的规则也应用于生成的文本。

UA必须识别换行（U+000A）当做换行符。UA可以选择性的把其他的强制中断符当做UAX14换行符。

_下边的；例子展示了PRE和P元素以及HTML中有nowrap属性元素期望的空格表现。_

```css
pre        { white-space: pre }
p          { white-space: normal }
td[nowrap] { white-space: nowrap }
```

_额外的，一个HTML中的带有非标准的wrap属性的PRE元素的效果是通过如下的例子表明的：_

```css
pre[wrap]  { white-space: pre-wrap }
```

#### white-space处理模型

对于每一个行内元素来说（包括匿名的行内元素），执行下边的步骤：

1. 如果white-space设置为了normal、nowrap或者pre-line的话就移除每一个tab（U+0009）、回车（U+000D）或者换行（U+000A）符附近的空格（U+0020）。

1. 如果white-space设置为了pre或者pre-wrap，任何的空格（U+0020）序列不会在元素边界断掉就好像他们是不间断（non-breaking）空格。然而，对于pre-wrap，一个行的序列的末端可能会断掉。

1. 如果white-space设置为了normal或者nowrap，换行符为了渲染目的通过用户代理特殊的基于内容脚本的算法转换为接下来的字符：一个空格符，一个零宽空格符（U+200B）或者没有字符（不渲染）。

1. 如果white-space设置为了normal、nowrap或者pre-line，
	
	1. 每一个tab（U+0009）被转换为一个空格（U+0020）

	1. 紧接另一个空格（U+0020）的任何的空格（U+0020）——即使是在行内之前的空格（如果空格的white-space设置为了normal、nowrap或者pre-line）——会被移除。

然后，布局块容器的行内盒。行内盒布局完了以后，将双向bidi重排序考虑在内，且依照指定的white-space的属性值包装。当包装的时候，基于上边的空白合并的步骤之前的文本来确定换行的机会。

至于每一行的布局，

1. 如果空格（U+0020）在一行（white-space设置为了'normal',、'nowrap'或者'pre-line'）的开始位置，就移除他。

1. 所有的tab（U+0009）以水平的移位（以下一个字形的边界开始到下一个tab边界结束）来渲染。tab停止在从块的内容边界开始到以这个块的font渲染8倍空格（U+0020）宽的这个点上。

1. 如果空格（U+0020）在行（white-space设置为了'normal',、'nowrap'或者'pre-line'）的末尾，他也被移除。

1. 如果空格（U+0020）或者tab（U+0009）在行（white-space设置为了pre-wrap）的末尾，用户代理可以直接合并他们。

浮动和绝对定位的元素不会采用换行的机会。

#### 合并双向空白的例子

给定下边的标记片段，注意特殊的空格（用不同的背景和border来强调识别）：

![标记片段](https://github.com/dolymood/blog/raw/master/pics/003.png)

ltr元素代表了由左到右嵌入的内容，rtl元素代表了由右到左嵌入的内容，且假设white-space属性是normal，上边的处理模型会是下边的结果：

* B之前的空格将会和A之后的空格合并。

* C之前的空格将会和B之后的空格合并。

这样就会留下两个空格，一个在A之后（由左到右的嵌入级别），和B之后的那个（由右到左的嵌入级别）。这是然后通过Unicode双向算法渲染的，结果是这样的：

![标记片段](https://github.com/dolymood/blog/raw/master/pics/004.png)

注意这里在A喝B之间有两个空格，在B喝C之间没有。这有时候可以通过使用自然的双向字符代替明确的嵌入级别来避免。这对于避免空格紧接tag内部开始和结束也是好的。

#### 控制合并字符细节

控制不是U+0009 (tab), U+000A (line feed换行), U+0020 (space空格), 和 U+202x (bidi formatting characters双向格式化字符) 的字符的渲染和任何普通的字符的渲染是一样的。

合并字符应该视为字符和他们应该被合并的一部分。例如：如果你有类似的内容：`o<span>&#x308;</span>`的话，:first-letter的样式会应用到整个字形上；他不仅仅匹配基本的字符。
