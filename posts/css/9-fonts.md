## CSS fonts

### 简介

设置font属性会是样式表最常见的用途。不幸的是，不存在明确的和公认的分类法来分类font，且某一方面适用于一种font family字体名称序列不一定适用于其他的。例如，Italic通常用于标注斜体文本，但是斜体文本也可以通过Oblique, Slanted, Incline, Cursive或者Kursiv来标注。因此，将一个字体选择属性映射到一个特定的font上不是一个简单的问题。

### font匹配算法

因为font属性没有可接受的，公认的分类法，所以匹配font face的属性必须认真做。匹配属性需要明确的顺序来保证这个匹配的过程中跨域UA（用户代理）的结果尽可能的一致。

<!--more-->

1. 用户代理需要使用（访问）一个CSS2.1中所有的font相关的属性中哪些是UA认识的数据库。如果同一属性上有两种font，那么UA选择其一就好。

1. 一个给定的元素和这个元素中的每一个字符，UA集合可以应用到这个元素上的字体属性。使用完整的属性，UA使用font-family属性选择一个暂定的font family。剩余的属性通过每一个属性描述的匹配条件再次测试family。如果有匹配所有余下的属性，那么那就是给定元素或者字符匹配的font face。

1. 如果通过第2步中的font-family没有匹配的font face，且如果在font上有下一个供选择的font-family设置了，然后就用下一个供选择的fong-family来重复第2步。

1. 如果存在匹配的font face，但是他不包含当前字符的字形，且如果在font上有下一个供选择的font-family设置了，然后就用下一个供选择的fong-family来重复第2步。

1. 如果在family中没有可选择的font，那么就是用用户代理依赖的默认font-family重复第2步，在默认font中使用可获得最好的匹配。如果不分字符不能使用这个font显示，那么UA可以使用其他方法来决定适用于那个字符的可用字体font。UA应该映射每一个没有合适的font字符（UA选择的一个可见符号，最好是从UA的可用的font face中的一个“缺失的字符”字形）。

从上边的第2步中的每个属性匹配规则如下：

1. 首先尝试font-style。如果UA的font数据库中的face标记有CSS关键词italic（首选）或者oblique那么italic就是合适的。其他情况的值必须明确匹配或者font-style匹配失败。

1. 下一个尝试的就是font-variant。"Small-caps小型大写字母"匹配：

	1. 一个标有small-caps的font

	1. 在small caps中的font是合成的

	1. 小写字母被大写字母替代的font

	一种small-caps的font可以从一种正常的font以电子方式缩放大写字母来合成。normal匹配一种font的正常的（非small-caps）变体。一种font不能没有一个正常的变体。一种font仅作为small-caps可用，他应该是可选择为一个normal的face或者一个small-caps的face。

1. font-weight是下一个匹配的，他不会匹配失败。

1. font-size必须在一个UA依赖的容差范围内匹配（典型地，可缩放font的尺寸是四舍五入最接近的整像素，而对于位图字体来说它的容差可以大到20%。）。例如，进一步的计算，其他属性中的em值是以font-size的计算值为基础的。

### font family：font-family属性

* __font-family__
	
	_值：_  [[ < family-name > | < generic-family > ] [, < family-name >| < generic-family >]* ] | inherit

	_初始化：_ 取决于用户代理

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性的值是一个font family名字（generic family名字）优先级列表。不像其他大多数的CSS属性，用逗号分隔的复合值，他们是可供选择的：

```css
body { font-family: Gill, Helvetica, sans-serif }
```

尽管很多的font提供了“缺失的字符”字形（典型的是一个开放的盒子），就像他的名字蕴含的当在这个font不能找到字符对应的匹配时才会考虑使用。（然而，他应该考虑匹配[U+FFFD](http://www.fileformat.info/info/unicode/char/0fffd/index.htm)，“缺失的字符”的字符的编码位置）。

font family的名字有两种类型：

* __< family-name >__

	一个选择的font family的名字。前面的例子中"Gill" and "Helvetica"是font family。

* __< generic-family >__

	前面的例子中sans-serif就是一个generic family名字。generic families定义如下：

	* 'serif' (例如, Times)

	* 'sans-serif' (例如, Helvetica)

	* 'cursive' (例如, Zapf-Chancery)

	* 'fantasy' (例如, Western)

	* 'monospace' (例如, Courier)

	样式表设计者鼓励在最后提供一个可选择的generic font family（通用字体）。generic font family的名字是关键词且绝对不能用引号包着。

font family的名字可以用引号包着作为字符串，或者不用引号作为一系列的标识符。这意味着大多数的在每一个token开始位置的标点字符和数字必须转义成不带引号的font family名字。

例如，下边的声明都是不可用的：

```css
font-family: Red/Black, sans-serif;
font-family: "Lucida" Grande, sans-serif;
font-family: Ahem!, sans-serif;
font-family: test@foo, sans-serif;
font-family: #POUND, sans-serif;
font-family: Hawaii 5-0, sans-serif;
```

如果一个font family的名字是一系列的标识符的话，这个名字的计算值就是转换为将序列中的标识符以单独空格连接在一起。

为了避免转义错误，建议包含空格、逗号或者连接符以外的标点字符的font family的名字加上引号：

```
body { font-family: "New Century Schoolbook", serif }

<BODY STYLE="font-family: '21st Century', fantasy">
```

font family的名字如果碰巧和关键词值（'inherit', 'serif', 'sans-serif', 'monospace', 'fantasy', 'cursive'）一样的话，必须用引号引起来，这样才能阻止同名的关键词混淆。关键词'initial'和'default'是保留着为了未来使用的，所以也必须用引号包着font名字。UA不能考虑将这些关键词来匹配'<font-family>'类型。

#### generic font families 通用字体

generic font families 是一种备用机制，一种保留样式表中没有选择任何的font的最坏的情况下的作者意图。对于最佳的版式控制，部分命名的font应该在样式表中使用。

所有的五个generic font families定义存在于所有的CSS实现中（他们没有必要映射这五个不同的实际font）。用户代理应该对generic font families提供合理的默认选择；这在底层技术允许的范围内尽可能的表达了每一个family的特点。

用户代理支持允许用户为generic fonts选择备选项。

_PS：主要有serif、sans-serif、cursive、fantasy、monospace这几个值。_

_PS：详细部分请参考[w3c css2.1 fonts](http://www.w3.org/TR/CSS21/fonts.html#generic-font-families)。_

### font样式：font-style属性

* __font-style__

	_值：_  normal | italic | oblique | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性在一种font family中选择normal，italic和oblique之间的face。

normal值选择了一种在UA的font数据库中分类是normal的字体，而oblique则是选择了一种标签是oblique的字体。italic的值选择了一种标签是italic或者如果那个不可用的话，标签是oblique的字体。

在UA的font数据库中标签是oblique的字体实际上通过以电子的方式倾斜normalfont生成。

名字中有Oblique, Slanted 或者 Incline的font在UA的font数据库中会贴上oblique的标签。名字中有Italic, Cursive 或者 Kursiv的font中在UA的数据库中会贴上italic的标签。

```css
h1, h2, h3 { font-style: italic }
h1 em { font-style: normal }
```

上边的例子，在H1中的em元素中的文本将会以normal的face出现。

### Small-caps：font-variant属性

* __font-variant__

	_值：_  normal | small-caps | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

一种font family中的另一种可用类型就是small-caps小型大写字母。在小型大写字母font中小写的字母和大写的字母很像，但是
会更小的尺寸以及稍微不同的比例。font-variant属性选择那种font。

_下边的例子中，H3元素中的文本是small-caps的，任何的EM元素中的文本是oblique的，且在H3中的EM中的元素的是oblique且small-caps的。_

```css
h3 { font-variant: small-caps }
em { font-style: oblique }
```

在一种font family中可能也有其他的变体variants（old-style numerals, small-caps numerals, condensed 或者 expanded letters），但是在CSS2.1中没有属性选择他们。

### 字体粗细：font-weight属性

* __font-weight__

	_值：_  normal | bold | bolder | lighter | 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900 | inherit

	_初始化：_ normal

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 依本文而定

font-weight属性选择了font的重要性weight。从100到900这个有序的序列，每一个数字表示一个weight，一个比一个更黑dark。关键词normal和400相同，bold和700相同。其他不是normal和bold的关键词往往和font名字和数字比例有关，但都是从100到900这9个列表中选择出的。

```css
p { font-weight: normal }   /* 400 */
h1 { font-weight: 700 }     /* bold */
```

bolder和lighter值选择了相对于继承自父元素的font的weight的值计算的weight粗细。

```css
strong { font-weight: bolder }
```

### font尺寸：font-size属性

* __font-size__

	_值：_  <绝对尺寸absolute-size> | <相对尺寸relative-size> | <长度length> | <百分比percentage> | inherit

	_初始化：_ medium

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 根据继承而来的font尺寸

	_媒介：_ 可见媒体

	_计算值：_ 绝对长度

font尺寸对应着一个em广场，这是排版中的一个概念。注意某一个字形可以移除他们的em广场之外。他的值有如下含义：

* __<绝对尺寸absolute-size>__
	
	一个绝对尺寸关键词是UA保持和计算的font尺寸的表格中的顺序。可能的值是：

	[ xx-small | x-small | small | medium | large | x-large | xx-large ]

	下边的表格给用户代理对绝对尺寸映射到HTML标题和绝对的font-size提供了指导方针。他们的值都是相对于medium这个值的。

	| CSS绝对尺寸值 | xx-small | x-small | small | medium | large | x-large | xx-large | 其他 |
	| ------------- | -------- | ------- | ----- | ------ | ----- | ----- | -------- | ---- |
	| HTML中font尺寸 | 1 |   | 2 | 3 | 4 | 5 | 6 | 7 |

	CSS实现者应该构建一个的表格，用来将绝对尺寸的关键词相对于medium的font尺寸（部分设备和他的特征）缩放的因子。

	不同的媒介需要不同的缩放因子。同样，当计算这个表的时候UA应该将font的质量和可用性考虑在内。这个表格可能对于不同的font family是不一样的。

* __<相对尺寸relative-size>__

	一个相对尺寸关键词的解释是相对于表格中的font尺寸和父元素的font尺寸的。值有[ larger | smaller ]。

在计算元素的font尺寸时，不应该将length长度和percentage百分比考虑到font尺寸表格之中。

负值是不允许的。

其他属性，em和ex的长度值是根据当前元素的font尺寸的计算值的。对于font-size属性，这些值的单位是根据父元素的font尺寸的计算值的。

例子：

```css
p { font-size: 16px; }
@media print {
	p { font-size: 12pt; }
}
blockquote { font-size: larger }
em { font-size: 150% }
em { font-size: 1.5em }
```

### 简写字体属性：font属性

* __font__

	_值：_  [ [ <'font-style'> || <'font-variant'> || <'font-weight'> ]? <'font-size'> [ / <'line-height'> ]? <'font-family'> ] | caption | icon | menu | message-box | small-caption | status-bar | inherit

	_初始化：_ 参见单独属性

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 参见单独属性

	_媒介：_ 可见媒体

	_计算值：_ 参见单独属性

font属性（除了下边‘almost’部分描述的）是在样式表中同一位置设置'font-style', 'font-variant', 'font-weight', 'font-size', 'line-height'和'font-family'的简写属性。这个属性的语法是基于传统的为了设置和font相关的多个属性的排版简写记法。

所有font相关的属性首先被设置为他们各自的初始值，前边的段落中有介绍。然后，在font简写属性中明确的给定了那些属性的值。对于一个允许的和初始的定义值，请看之前的段落。

```css
p { font: 12px/14px sans-serif }
p { font: 80% sans-serif }
p { font: x-large/110% "New Century Schoolbook", serif }
p { font: bold italic large Palatino, serif }
p { font: normal small-caps 120%/120% fantasy }
```

第二条规则，font尺寸百分比（80%）根据父元素的font尺寸。第三条规则。line-height百分比根据元素自身的font尺寸。

前面的前3条规则，font-style，font-variant和font-weight都没有明确的涉及到，那也就意味着他们三个的值分别是他们的初始值（normal）。第4条规则设置了font-weight为bold，font-style为italic且暗中设置了font-variant的值为normal。

第5条规则设置了font-variant（small-caps），font-size（父元素font的120%），line-height（font尺寸的120%）以及font-family（fantasy）。他根据关键词normal，应用到两个剩余的属性上：font-style和font-weight。

下边的值根据系统font：

* __caption__

	用于控制标题的font。

* __icon__

	用于标记图标icon的font。

* __menu__

	菜单中使用的font。

* __message-box__

	对话框中使用的font。

* __small-caption__

	用于控制标签的font。

* __status-bar__

	用于窗口状态栏的font。

系统font只允许全局设置；也就是font family, size, weight, style等等要同时设置。如果要求的话这些值是可以之后单独改变。如果在给定的设备上没有指定特征的font的话，用户代理应该要么聪明的替代要么替换为用户代理默认的font。对于普通的font而言（如果对于系统font），任何的单独的属性都不是系统可用的首选项的一部分，那些属性应该设置为他们的初始值。

这也就是这个属性‘almost’是一个简写属性：系统font只能紧紧指定这个属性，不是和font-family自身一起，因此font允许作者做的比他的子属性的总和要更多。

然而，单独的属性例如font-weight依旧从系统font中取值，这是可以单独改变的。

```css
button { font: 300 italic 1.3em/1.7em "FB Armada", sans-serif }
button p { font: menu }
button p em { font-weight: bolder }
```

_如果font碰巧用于特定系统上下拉菜单，例如，9像素的Charcoal，600的weight，那么这个时BUTTON后代的P元素将会显示为如同下边的规则一样的效果：_

```css
button p { font: 600 9px Charcoal }
```

_因为font是一个简写属性，重置了其他任何的没有明确给出值的属性的值为初始值，这和如下的声明有相同的效果：_

```css
button p {
  font-family: Charcoal;
  font-style: normal;
  font-variant: normal;
  font-weight: 600;
  font-size: 9px;
  line-height: normal;
}
```