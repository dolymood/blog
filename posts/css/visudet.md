## CSS 可视化格式模型细节（Visual formatting model details）

### 包含块的定义

有时一个元素的盒的位置和尺寸的计算是相对于一个确定的矩形的，叫做这个元素的包含块。一个元素的包含块的按照如下定义：

1. 根元素所在的包含块是一个叫做初始包含块的矩形。针对于持续型媒体来说他和viewport一样的尺寸且固定在了画布的原点；针对于分页型媒体来说压实每一page区域。初始化包含块的direction属性的值是和根元素的一样。

1. 针对于其他元素，如果元素的position属性是relative或者static的话，，包含块就是最近的祖先块容器盒的内容区域的边界。

1. 如果元素是position：fixed的，那么其包含块就是，针对于持续媒体来说是由viewport创建的，针对于分页媒体来说就是page区域。

1. 如果元素是position：absolute的，包含块是由最近的position是absolute、relative或者fixed的祖先元素以下边的方式创建的：

	1. 如果祖先元素是行内元素，包含块的顶、左边就是祖先元素的第一个盒的顶、左padding边界，右、下边就是祖先元素生成的最后一个盒的右、下padding边界。

	1. 其他情况，包含块区域是祖先元素的padding边界。

	如果没有这样额祖先元素的话，包含块就是初始包含块。

<!--more-->

_PS: 这里来上一张来w3help的图：_

![包含块判断](http://www.w3help.org/zh-cn/kb/008/008/CB4.png)

_如果没有定位，那么在接下来的文档中的包含块（C.B.）：_

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
   <HEAD>
      <TITLE>Illustration of containing blocks</TITLE>
   </HEAD>
   <BODY id="body">
      <DIV id="div1">
      <P id="p1">This is text in the first paragraph...</P>
      <P id="p2">This is text <EM id="em1"> in the 
      <STRONG id="strong1">second</STRONG> paragraph.</EM></P>
      </DIV>
   </BODY>
</HTML>
```

_结果见下表：_

| 生成盒的元素 | 被谁创建的包含块 |
| ------------ | ---------------- |
| html         | 初始包含块       |
| body         | html             |
| div1         | body             |
| p1           | div1             |
| p2           | div1             |
| em1          | p2               |
| strong1      | p2               |

_如果我们设置div1：_

```css
#div1 { position: absolute; left: 50px; top: 50px }
```

_他的包含块就不再是body了，变成了初始包含块。_

_如果我们将em1也定位：_

```css
#div1 { position: absolute; left: 50px; top: 50px }
#em1  { position: absolute; left: 100px; top: 100px }
```

_那么包含块就变成了下表这样：_

| 生成盒的元素 | 被谁创建的包含块 |
| ------------ | ---------------- |
| html         | 初始包含块       |
| body         | html             |
| div1         | 初始包含块       |
| p1           | div1             |
| p2           | div1             |
| em1          | div1             |
| strong1      | em1              |

_通过给em1定位，他的包含块就成了最近的定位的祖先盒了。_

### 内容宽：width属性

* __width__
	
	_值：_  <长度length> | <百分比percentage> | auto | inherit

	_初始化：_ auto

	_应用在：_ 一切元素除了非替换行内元素，table rows和row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比和auto或者绝对的值

这个属性指定了一个盒的content width（内容宽）。

这个属性不能应用到非替换的行内元素上。一个非替换行内元素的内容宽是由其渲染的内容的宽。想起来：行内盒是布局在行盒中的。行盒的宽是由他的包含块给定的，但是可能会由于浮动而缩短了。

width的值有如下意思：


* __<长度length>__
	
	使用一个绝对单位的值指定内容区域的宽度。

* __<百分比percentage>__
	
	使用一个百分比宽。百分比的值是根据他的包含块的宽计算的。如果包含块的块取决于当前元素的宽的话，那么在CSS2.1中布局出来的结果就是undefined。

* __auto__
	
	这个宽取决于其他属性的值。看下边的段落。

width的值不能是负的。

_例如，设置一个段落的宽是100像素：_

```css
p { width: 100px }
```

### 计算width和margin

一个元素用于布局的width, margin-left, margin-right, left和right属性的值取决于他们自身所生成的盒的类型。实际上，使用值是和计算值一样的，用auto代替不可用的值，切百分比的值的计算都是以包含块为基础的，但是也有例外。下边的这些情况是需要区别对待的：

1. 行内，非替换元素

1. 行内，替换元素

1. 普通流中的块级，非替换元素

1. 普通流中的块级，替换元素

1. 浮动，非替换元素

1. 浮动，替换元素

1. 绝对定位，非替换元素

1. 绝对定位，替换元素

1. 普通流中的inline-block，非替换元素

1. 普通流中的inline-block，替换元素

对于1——6，9——10点，相对定位元素的left和right的值根据上一章中相对定位的规则确定。

#### 行内，非替换元素

width不起作用。如果margin-left和margin-right是auto的话，计算值就是使用值0

#### 行内，替换元素

如果margin-left和margin-right是auto的话，计算值就是使用值0。

如果width和height的计算值都是auto，且这个元素有固有宽，那么固有宽就是width的使用值。

如果width和height的计算值都是auto，且这个元素没有固有宽，但是有固有高或者固有比例；或者width的计算值是auto，height有其他的计算值，且这个元素有固有比例；那么width的使用值就是：

	height的使用值 * 固有宽高比（固有比例）

如果width和height的计算值都是auto，且这个元素有固有比例，但是没有固有的宽、高，那么在CSS2.1中width的使用值就是undefined。然而，建议是这样，如果包含块宽本身宽不依赖于这个替换元素的话，那么width的使用值就是和在普通流中的块级非替换元素的宽是相等的。

其他情况，如果width的计算值是auto，且这个元素有固有宽，那么width的使用值就是固有宽的值。

其他情况，如果width的使用值是auto，且没有什么条件的话，那么width的使用值就变成了300px。如果对于设备来说300px太大了，那么用户代理就应该用一个具有2:1的比例的且适应设备的最大矩形的宽来代替。

#### 普通流中的块级，非替换元素

下边的条件必须控制在其他属性的使用值之间：

	'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' + 'border-right-width' + 'margin-right' = 包含块的宽

如果width不是auto，'border-left-width' + 'padding-left' + 'width' + 'padding-right' + 'border-right-width'的值比包含块的宽还要大的话，那么针对于下边的规则来说，margin-left和margin-right的值是auto的话就作为0处理。

如果上边的所有的计算值其中之一不是auto，值称为“超出条件over-constrained”，且其中一个的使用值会和他的计算值不同。如果包含块的direction属性的值是ltr的话，margin-right的指定值会被忽略，且这个值会是为了使得表达式相等而计算出来的值。如果direction是rtl的话，margin-left代替margin-right。

如果恰好只有一个值被指定为了auto，他的使用值就是使得上边的条件相等的值。

如果width被设置为了auto，其他的auto值以0对待，然后通过条件计算width的值。

如果margin-left和margin-right是auto，他们的使用值是相等的。这个元素会相对于包含块边缘水平居中。

#### 在普通流中的块级，替换元素

width的使用值用‘行内替换元素’中的规则确定。然后用‘非替换块级元素’中的规则确定margin的值。

#### 浮动，非替换元素

如果margin-left或者margin-right的值计算成了auto，他们的使用值就是0.

如果width的值计算成了auto，使用值就是“收缩以适应shrink-to-fit”宽。

计算“收缩以适应”的宽和计算使用自动table布局的table cell的宽的算法类似。简要而言：通过将内容（没有换行除非有显示换行）格式化来计算最佳宽，且同时会通过尝试所有的换行符来计算最佳最小宽等等。CSS2.1中没有确切定义这个算法。第三，找到可用宽：在这种情况下，包含块的宽减去'margin-left', 'border-left-width', 'padding-left', 'padding-right', 'border-right-width', 'margin-right'的使用值以及任何滚动条的宽。

然后“收缩以适应”的宽就是：min(max(最佳最小宽preferred minimum width, 可用宽available width), 最佳宽preferred width)

#### 浮动，替换元素

如果margin-left和margin-right的值计算为auto，他们使用值就是0。width的使用值通过‘行内替换元素’规则来确定。

#### 绝对定位，非替换元素

在这个以及下个段落中，static position静态位置，意思就是这个元素在普通流中的位置。更准确的来说：

* 静态位置包含块（static-position containing block）就是一个假设盒（一个元素的position的值是static，他的float的值是none的第一个盒）的包含块。

* 静态位置的left就是包含块的左边界到假设盒（一个元素的position的值是static，他的float的值是none的第一个盒）的左margin边界的距离。如果假设盒在包含块的左边的的话，这个值就是负的。

* 静态位置的right就是包含块的右边界到和之前一样的假设盒的右margin边界的距离。如果假设盒在包含块的左边的的话，这个值就是正的。

但是相比较实际去计算这个假设盒的尺寸，用户代理更愿意去随意的猜测他的最佳位置。

确定这些元素的使用值的条件是：
	
	'left' + 'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' + 'border-right-width' + 'margin-right' + 'right' = 包含块的宽

如果left，width和right都是auto：首先设置所有的值是auto的margin-left和margin-right的值为0.然后如果这个元素创建的静态位置包含块的direction属性是ltr的话就设置left是静态位置然后应用下边的第3条规则；其他情况，设置right是静态位置然后应用下边的第1条规则。

如果他们三个都不是auto：如果margin-left和margin-right都是auto，利用两个margin都相等的约束条件来计算等式，除非他们计算出来的值都是负的，这种情况下当包含块的direction是ltr（rtl）的话，设置margin-left（margin-right）为0然后计算margin-right（margin-left）。如果margin-left和margin-right其中之一auto，那么就利用等式计算他的值。如果值超出条件了，忽略left（包含块的direction是rtl的情况下）的值或者right（包含块的direction是ltr的情况下）的值且计算那个值。

其他情况，设置值是auto的margin-left和margin-right的值为0，且应用下边的6条规则中的一条。

1. left和width是auto，且right不是auto，那么width收缩以适应。然后计算left。

1. left和right是auto，且width不是auto，然后如果元素生成的静态位置包含块的direction是ltr的话，设置left为静态位置，否则设置right为静态位置。然后计算left（direction是rtl）或者right（direction是ltr）。

1. width和right是auto，且left不是auto，然后宽度收缩以适应。然后计算right。

1. left是auto，width和right不是auto，就计算left。

1. width是auto，left和right不是auto，就计算width。

1. right是auto，left和width不是auto，然后计算right。

计算“收缩以适应”的宽和计算使用自动table布局的table cell的宽的算法类似。简要来说：通过将内容（没有换行除非有显示换行）格式化来计算最佳宽，且同时会通过尝试所有的换行符来计算最佳最小宽等等。CSS2.1中没有确切定义这个算法。第三，找到可用宽：通过设置left（情况1）或者right（情况3）为0之后来计算width。

#### 绝对定位，替换元素

在这种情况下，同样是上边（绝对定位，非替换元素）的那个等式，但是上边余下的内容会被如下规则替换掉：

1. 通过‘行内替换元素’中的规则确定width。如果margin-left或者margin-right设置为auto，那么他的使用值就由下边的规则确定。

1. 如果left和right都是auto，那么如果这个元素创建的静态位置包含块的direction属性是ltr的话，设置left为静态位置，其他如果direction是rtl的话，就设置right为静态位置。

1. 如果left和right是auto，将margin-left或者margin-right是auto的值用0代替。

1. 这时候如果margin-left和margin-right依旧是auto的话，在两个margin必须相等的条件下计算等式，除非他们的结果是负的，这种情况下当包含块的direction是ltr（rtl）的话，设置margin-left（margin-right）为0，然后计算margin-right（margin-left）。

1. 这时候如果还有谁的值是auto，那么就利用等式计算他的值。

1. 这时候如果值超出条件了，忽略left（direction是rtl）或者right（direction是ltr）的值，且计算那个值。

#### 普通流中的inline-block，非替换元素

如果width是auto，那么使用值就利用浮动元素中的收缩以适应来计算。

计算值是auto的margin-left或margin-right的值变为使用值是0。

#### 普通流中的inline-block，替换元素

和‘行内替换元素’一样。

### 最小和最大宽：min-width和max-width

* __min-width__
	
	_值：_  <长度length> | <百分比percentage> | inherit

	_初始化：_ 0

	_应用在：_ 一切元素除了非替换行内元素，table-rows以及row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值

* __max-width__
	
	_值：_  <长度length> | <百分比percentage> | none | inherit

	_初始化：_ none

	_应用在：_ 一切元素除了非替换行内元素，table-rows以及row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值或者none

这两个属性允许作者使得内容宽在一个确定的范围中。他们的值有如下意思：

* __<长度length>__
	
	为使用的宽指定一个固定的最小或者最大值。

* __<百分比percentage>__
	
	指定百分比。百分比的值是根据他生成盒的包含块的宽计算的。如果包含块的宽是负值的话，那么使用值就是0.如果包含块的宽取决于元素的宽的 话，那么在CSS2.1中布局出来的结果就是undefined。

* __none__
	
	（只能用在max-width上）这个盒没任何宽度限制。

min-width，max-width如果是负值的话是不合法的。

在CSS2.1中，在table, inline tables, table cells, table columns, 和column groups上的min-width和max-width效果是未定义的。

下边的算法描述了这两个属性是怎么影响width属性的值的：

1. 暂时的使用的宽是根据上面的‘计算width和marin’的规则计算出来的（没有min-width和max-width）。

1. 如果暂时的使用的宽比max-width要大，再次应用上边的规则，但是这次width的计算值要使用max-width的计算值。

1. 如果结果宽比min-width要小的话，再次应用上边的规则，但是这次width的计算值要使用min-width的值。

然而，对于有固有比例且width和height都是auto的替换元素来说，算法是这样的：

从下边的table中选择可以适当的违反约束constraint violation来计算height和width的值。就拿max-width和max-height为max(min,max)，控制min ≤ max。在表中，w和h是代表忽略min-width,min-height, max-width和max-height的属性的情况下计算的width和height的结果。通常会有固有的width和height，但是他们可能不在替换元素且有固有比例的这种情况下。

![表](https://github.com/dolymood/blog/raw/master/pics/002.png)

然后利用计算出的width的值应用之前的‘计算width和marin’的规则。

### 内容高：height属性

* __height__
	
	_值：_  <长度length> | <百分比percentage> | auto | inherit

	_初始化：_ auto

	_应用在：_ 一切元素除了非替换行内元素，table rows和row groups

	_可继承：_ 不可以

	_百分比：_ 看下文

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比和auto或者绝对的值

这个属性指定了一个盒的content height（内容高）。

这个属性不能应用到非替换的行内元素上。请看下面的‘非替换行内元素中计算height和margin段落’段落。

height的值有如下意思：


* __<长度length>__
	
	使用一个绝对单位的值指定内容区域的高度。

* __<百分比percentage>__
	
	使用一个百分比宽。百分比的值是根据他的包含块的高计算的。如果包含块的高没有明确指定的话，且这个元素不是绝对定位的，计算值将是auto。根元素上的百分比的高的计算是相对于初始包含块。_注意：针对于其包含块是基于块级元素的绝对定位元素来说，百分比的计算是相对于那个元素的padding盒的高度的。_

* __auto__
	
	这个宽取决于其他属性的值。看下边的段落。

width的值不能是负的。

_例如，设置一个段落的高是100像素：_

```css
p { height: 100px }
```
_段落中的内容高于100像素的话就会溢出overflow，取决于根据overflow属性。_

### 计算height和margin

为了计算top, margin-top, height, margin-bottom和bottom的值，我们就必须针对于如下类型的盒进行详细的介绍：

1. 行内，非替换元素

1. 行内，替换元素

1. 普通流中的块级，非替换元素

1. 普通流中的块级，替换元素

1. 浮动，非替换元素

1. 浮动，替换元素

1. 绝对定位，非替换元素

1. 绝对定位，替换元素

1. 普通流中的inline-block，非替换元素

1. 普通流中的inline-block，替换元素

对于1——6，9——10点，top和bottom的使用值根据上一章中相对定位的规则确定。

#### 行内，非替换元素

height不起作用。当前内容的高需要以font为基础的，但是在本规范中没有规定是怎样的。

一个行内、非替换盒的垂直方向上的padding，border以及margin从内容区域的顶部和底部开始的，且和line-height没有任何关系。但是在计算行盒的高度的时候只有line-height是有用的。

如果使用了多种字体，内容区域的高度不在本规范中定义。

#### 行内替换元素，普通流中块级替换元素，普通流中inline-block替换元素以及浮动替换元素

如果margin-top或者margin-bottom是auto的话，使用值就是0。

如果width和height的计算值都是auto，且这个元素有固有高，那么height的使用值就是固有高。

其他，如果height的计算值是auto，元素有固有比例，那么height的使用值就是：

	(used width使用width) / (intrinsic ratio固有比例)

其他，如果height的计算值是auto，且这个元素有固有的高，那么height的使用值就是固有高。

其他，如果height的计算值是auto，但是社么固有值都没有，那么height的使用值就必须是一个具有2:1比例的最大的矩形（高度不大于150px，且宽度不大于设备宽）的高。

#### 当overflow的计算值不是visible时普通流中块级非替换元素

这段也可应用到普通流中块级非替换元素（overflow不等于visible，但是已经传递到了viewport）上。

如果margin-top或者margin-bottom是auto的话，使用值就是0。如果height是auto，那么height就取决于这个元素有仍和的块级孩子和他是否有padding或者border：

这个元素的高是从他的内容的顶部边界到下边第一个适用的 距离：

1. 最后一个行盒的下边界，如果一个一行或者多行的盒创建了新的IFC。

1. 他的在普通流中最后一个孩子的bottom margin（可能折叠了）的下边界，如果这个孩子的bottom margin不和元素的bottom margin折叠的话。

1. 他的在普通流中最后一个孩子的border的下边界，如果这个孩子的top margin不元素的bottom margin折叠的话。

1. 0， 其他情况。

只有在普通流中的孩子考虑在内（浮动盒和绝对定位盒被忽略，不考虑他们偏移的相对定位盒子）。注意子盒可能是匿名块盒。

#### 绝对定位，非替换元素

在这个以及下个段落中，static position静态位置，意思就是这个元素在普通流中的位置。更准确的来说，静态位置的top是从包含块的顶边界到假设盒（一个元素的position的值是static，他的float的值是none，clear属性的值是none的第一个盒）的top margin边界的距离。如果假设盒在包含块之上的话，这个值就是负的。

但是相比较实际去计算这个假设盒的尺寸，用户代理更愿意去随意的猜测他的可能位置。

为了计算出最佳的静态位置，固定定位元素的包含块是初始包含块而不是viewport。

对于绝对定位元素来说，垂直方向上的使用值的大小必须符合下边的条件：

	'top' + 'margin-top' + 'border-top-width' + 'padding-top' + 'height' + 'padding-bottom' + 'border-bottom-width' + 'margin-bottom' + 'bottom' = 包含块的height

如果top。height，以及bottom这三个都是auto的话，设置top是静态位置且应用下边的第3条规则。

如果他们三个都不是auto：如果margin-top和margin-bottom都是auto，那么在两个margin相等的条件下通过等式计算值。如果margin-top或者margin-bottom其中之一是auto，那么利用等式计算另一个值。如果值超出条件了，忽略bottom的值然后计算那个值。

其他，应用下边6条规则中的一条：

1. top和height是auto，且bottom不是auto，那么height就以下边的‘BFC根的auto高’内容，设置值为auto的margin-top和margin-bottom的值为0，然后计算top。

1. top和bottom是auto，且height不是auto，然后设置top为静态位置，设置值为auto的margin-top和margin-bottom的值为0，然后计算bottom。

1. height和bottom是auto，且top不是auto，然后就以下边的‘BFC根的auto高’内容，设置值为auto的margin-top和margin-bottom的值为0，然后计算bottom。

1. top是auto，height和bottom不是auto，然后就设置值为auto的margin-top和margin-bottom的值为0，计算top。

1. height是auto，top和bottom不是auto，然后就设置值为auto的margin-top和margin-bottom的值为0，计算height。

1. bottom是auto，top和height不是auto，然后就设置值为auto的margin-top和margin-bottom的值为0，计算bottom。

#### 绝对定位，替换元素

在这种情况和上一个类似，除了元素有一个固有高。现在替换的顺序是这样的：

1. 通过‘行内替换元素’中的规则确定height。如果margin-top或者margin-bottom设置为auto，那么他的使用值就由下边的规则确定。

1. 如果top和bottom都是auto，把top替换为这个元素的静态位置。

1. 如果bottom是auto，将margin-top或者margin-bottom是auto的值用0代替。

1. 这时候如果margin-top和margin-bottom依旧是auto的话，在两个margin必须相等的条件下计算等式。

1. 这时候只有一个auto值，那么就利用等式计算他的值。

1. 这时候如果值超出条件了，忽略bottom的值，且计算那个值。

#### 复杂情况

这个段落应用在：

* 在普通流中的块级，非替换元素，且其overflow的值不是visible（除了overflow属性已经传递到了viewport）。

* inline-block，非替换元素。

* 浮动，非替换元素。

如果margin-top或者margin-bottom是auto，他们的使用值就是0。如果height是auto，那么他取决于‘BFC根的auto高’。

对于inline-block元素来说，当计算行盒的高的时候使用margin盒。

#### BFC根的auto高

在一定的情况下（例如前几段中涉及到的），创建了BFC的元素的高按照如下计算：

如果他只有行内级的孩子，那么height就是最顶的行盒的的顶部和最底部的行盒的低部之间的距离。

如果他只有块级的孩子，那么height就是最顶的块级子盒的margin边界的顶部和最下的块级子盒的margin边界的底部之间的距离。

绝对定位的孩子是被忽略的，且相对定位的盒是在忽略偏移情况下考虑的。注意子盒可能是一个匿名块盒。

额外的，如果这个元素有任何的浮动子孙（他们的bottom margin的边界在元素的bottom content边界之下）的话，那么height就会增加到包含这些边界。只有参与到这个BFC内的盒子才考虑在内，在一个绝对定位的子孙中的浮动是不考虑在内的。

### 最小和最大高：min-height和max-height

他们对于控制元素的高在一个确定的范围内有时候是很有用的。

* __min-height__
	
	_值：_  <长度length> | <百分比percentage> | inherit

	_初始化：_ 0

	_应用在：_ 一切元素除了非替换行内元素，table columns以及column groups

	_可继承：_ 不可以

	_百分比：_ 参见其他

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值

* __max-height__
	
	_值：_  <长度ength> | <百分比percentage> | none | inherit

	_初始化：_ none

	_应用在：_ 一切元素除了非替换行内元素，table columns以及column groups

	_可继承：_ 不可以

	_百分比：_ 参见其他

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值或者none

这两个属性允许作者使得内容高在一个确定的范围中。他们的值有如下意思：

* __<长度length>__
	
	指定一个固定的最小或者最大的计算的高。

* __<百分比percentage>__
	
	指定百分比。百分比的值是根据他生成盒的包含块的高计算的。如果包含块的高没有明确指定的话，且这个元素不是绝对定位的，那么百分比的值就为0（针对于min-height）或者none（针对于max-height）。

* __none__
	
	（只能用在max-height上）这个盒没任何高度限制。

min-height，max-height如果是负值的话是不合法的。

在CSS2.1中，在tables, inline tables, table cells, table rows, 和 row groups上的min-height和max-height效果是未定义的。

下边的算法描述了这两个属性是怎么影响height属性的值的：

1. 暂时的使用的高是根据上面的‘计算height和marin’的规则计算出来的（没有min-height和max-height）。

1. 如果暂时的使用的高比max-height要大，再次应用上边的规则，但是这次height的计算值要使用max-height的值。

1. 如果结果高比min-height要小的话，再次应用上边的规则，但是这次height的计算值要使用min-height的值。

然而，对于width和height都是auto的替换元素来说，使用上边‘最小和最大宽’中的算法来找到使用的width和height。然后，应用上边‘计算height和margin’中的规则，使用结果中的width和height作为他们的计算值。

### 行高计算：line-height和vertical-align属性

如同在‘IFC’段落中描述的那样，用户代理将行内级盒排列到一个垂直的行盒的层叠中。一个行盒的高按照如下来确定：

1. 每一个在行盒中的行内级的盒的高都是要计算的。对于替换元素，inline-block元素以及inline-table元素来说，就是他们的margin盒的高；对于行内盒，就是他们的line-height。

1. 行内级盒在垂直方向的对齐取决于他们的vertical-align属性的。在他们以top或者bottom对齐的情况下，他们必须对齐的使得行盒的高最小。如果这样的盒够高的话，没有更多的解决方案且CSS2.1中没有定义行盒的baseline的位置。

1. 行盒的高是最上边盒的顶部到最下边盒的底部之间的距离。

空的行内元素会生成一个空的行内盒，但是这些盒依旧会有margin，padding，border和行高，就好像他们有内容那样来计算。

#### 行间距leading和半行间距half-leading

CSS声明了每一个font都有font度量，用来指定一个字符在baseline之上高和在baseline之下的的深。在这段中我们使用A代表那个高（针对一个给定font且给定尺寸了）且用D代表那个深。我们同时定义`AD = A + D`，顶部到底部的距离。注意这些是font的整体的度量标准，不需要对任何的字形做上伸ascender或者下伸descender。

用户代理将一个非替换行内元素和其他的每一个相关的baseline对齐。然后对于每一种字形，确定A和D。注意一个单独元素的字形可能来自不同的font，因此不需要他们有相同的A和D。如果行内盒没有包含任何字形，就考虑他包含了一个这个元素的第一个字形的A和D的strut支柱（具有零宽度的可见字形）。

对于每一个字形，确定其行间距L，`L = line-height - AD`。半行间距就是A之上加上D之下的另一半，给定的字形行间距就是A的总高`= A + L/2`加上D的总深`= D + L /2`。

_注意L可能是负值。_

所有的字形的行内盒的高和他们每一边的半行间距之和才是真正的line-height。子元素盒不会影响这个高。

尽管一个非替换元素的margin，border和padding不参与行盒的计算，他们仍然围绕着行内盒渲染。这意味着如果指定的line-height比容器盒的内容高要低的话，padding的background和border的color可能会“流出bleed”到毗邻的行盒中。用户代理应该按照文档中的顺序渲染盒。这回导致后续行的border渲染到那些border之上以及在之前行的文本之上。

_注意，CSS2.1中没有定义一个行内盒的内容区域，因此不同的用户代理可能在不同的地方渲染background和border。_

* __line-height__
	
	_值：_  normal | <数字number> | <长度length> | <百分比percentage> | inherit

	_初始化：_ normal

	_应用在：_ 一切元素

	_可继承：_ 是

	_百分比：_ 根据元素自身的font size

	_媒介：_ 可见媒介

	_计算值：_ 对于<长度length>和<百分比percentage>来说就是绝对的值；其他和指定值一样

对于一个内容由行内级元素组成的块容器元素来说，line-height指定了这个元素内行盒的最小高。最小高由baseline之上的最小高和baseline之下的最小深构成，就像每一个行盒都是以一个零宽度的行内盒，这个元素的font以及行高属性开始的。我们称之为虚盒“支柱”。

指定一个font的baseline之上的高和baseline之下的深的度量标准是在一个font中包含着的。

对于非替换行内元素，line-height指定了计算行盒高所使用的高。

这个属性的值有如下意思：

* __normal__
	
	告诉用户代理根据这个元素的font的“合理的reasonable”值作为使用值。这个值和<数字number>同样的含义。我们建议‘normal’的使用值在1.0到1.2之间。计算值是normal。

* __<长度length>__
	
	指定计算行盒高使用的值。负值是不合法的。

* __<数字number>__
	
	这个属性的使用值是当前元素的font size和这个值number的乘积。负值是不合法的。计算值和指定值一样。

* __<百分比percentage>__
	
	这个属性的计算值是当前元素的fong size和百分比的乘积。负值是不合法的。

_下边的三条规则，计算出的行高是一样的：_

```css
div { line-height: 1.2; font-size: 10pt }     /* number */
div { line-height: 1.2em; font-size: 10pt }   /* length */
div { line-height: 120%; font-size: 10pt }    /* percentage */
```
当一个元素内包含的内容由多种字体渲染的时候，用户代理可以通过最大的font size来决定normal的line-height值。

_注意：在一个块容器盒子中所有的行内盒的line-height只有一个值且他们的font是相同的（他们是非替换元素，inline-block元素等等），上述将确保连续行的baseline正好是分开的line-height。这在多列不同的font然后还要对齐的布局中是很重要的，例如在table中。_

* __vertical-align__
	
	_值：_  baseline | sub | super | top | text-top | middle | bottom | text-bottom | <百分比percentage> | <长度length> | inherit

	_初始化：_ baseline

	_应用在：_ 行内级和table-cell元素

	_可继承：_ 不可继承

	_百分比：_ 根据元素自身的line-height

	_媒介：_ 可见媒介

	_计算值：_ 对于<长度length>和<百分比percentage>来说就是绝对的值；其他和指定值一样

这个属性影响了在一个由行内级元素生成的行盒中的垂直定位。

_注意在table上下文中，这个属性有不同的含义。_

下边的这些值只对一个父级的行内元素或者父级块容器元素的‘支柱’有用。

接下来的那些定义中，是针对于行内非替换元素，这个盒和高是line-height的盒（包含着盒的字形和每一边的半行间距，上边有介绍）对齐。对于其他的所有元素来说，这个盒是和margin盒对齐的。

* __baseline__
	
	这个盒的baseline和父盒的baseline对齐。如果这个盒没有baseline，就拿他的bottom margin边界和父盒的baseline对齐。

* __middle__
	
	这个盒的垂直的中间点和父盒的baseline加上父盒的x-height的一半对齐。。

* __sub__
	
	这个盒的baseline降低到父盒的合适的下标的位置。（这个值对于这个元素文本的font size没有影响。）

* __super__
	
	这个盒的baseline升高到父盒的合适的上标的位置。（这个值对于这个元素文本的font size没有影响。）

* __text-top__
	
	这个盒的top和父盒的内容区域的top对齐。

* __text-bottom__
	
	这个盒的bottom和父盒的内容区域的bottom对齐。

* __<百分比percentage>__
	
	将这个盒子升高（正值）或者降低（负值）这个距离（line-height的百分比）。这个值如果是0%的话，那么就和baseline一样。

* __<长度length>__

	将这个盒子升高（正值）或者降低（负值）这个距离。这个值如果是0cm的话，那么就和baseline一样。

接下来的这两个值是元素相对于行盒对齐的。由于这个元素可能会有相对于他对齐的孩子（反过来可能会有子孙相对于他们对齐），这些值使用对齐了的子树（aligned subtree）的边界。一个行内元素的对齐了的子树包含着那个元素以及他所有的vertical-align的计算值不是top或者bottom的所有行内元素孩子的对齐了的子树。对齐了的子树的top就是在这个子树中的最高的top值，bottom的值也是类似的。

* __top__
	
	对齐了的子树的top和行盒的top对齐。

* __bottom__
	
	对齐了的子树的bottom和行盒的bottom对齐。

一个inline-table的baseline就是这个table的第一行的baseline。

一个inline-block的baseline就是在普通流中的最后一个行盒的baseline，除非他没有在普通流中的行盒或者他的overflow的属性的计算值不是visible，在这种情况下baseline就是bottom margin的边界。