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
	
	_值：_  <length> | <percentage> | auto | inherit

	_初始化：_ auto

	_应用在：_ 一切元素除了非替换行内元素，table rows和row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比和auto或者绝对的值

这个属性指定了一个盒的content width（内容宽）。

这个属性不能应用到非替换的行内元素上。一个非替换行内元素的内容宽是由其渲染的内容的宽。想起来：行内盒是布局在行盒中的。行盒的宽是由他的包含块给定的，但是可能会由于浮动而缩短了。

width的值有如下意思：


* __<length>__
	
	使用一个绝对单位的值指定内容区域的宽度。

* __<percentage>__
	
	使用一个百分比宽。百分比的值是根据他的包含块的宽计算的。如果包含块的块取决于当前元素的宽的话，那么在CSS2.1中布局出来的结果就是undefined。

* __auto__
	
	这个宽取决于其他属性的值。看下边的段落。

width的值不能是负的。

_例如，设置一个段落的宽是100像素：_

```css
p { width: 100px }
```

### 计算宽和margin

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

如果width和height的计算值都是auto，且这个元素有原始宽，那么原始宽就是width的使用值。

如果width和height的计算值都是auto，且这个元素没有原始宽，但是有原始高或者原始比例；或者width的计算值是auto，height有其他的计算值，且这个元素有原始比例；那么width的使用值就是：

	height的使用值 * 原始宽高比（原始比例）

如果width和height的计算值都是auto，且这个元素有原始比例，但是没有原始的宽、高，那么在CSS2.1中width的使用值就是undefined。然而，建议是这样，如果包含块宽本身宽不依赖于这个替换元素的话，那么width的使用值就是和在普通流中的块级非替换元素的宽是相等的。

其他情况，如果width的计算值是auto，且这个元素有原始宽，那么width的使用值就是原始宽的值。

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

1. 这时候如果值超出条件了，忽略left（direction是rtl）或者right（direction是ltr）的值，且计算另一个值。

#### 普通流中的inline-block，非替换元素

如果width是auto，那么使用值就利用浮动元素中的收缩以适应来计算。

计算值是auto的margin-left或margin-right的值变为使用值是0。

#### 普通流中的inline-block，替换元素

和‘行内替换元素’一样。

### 最小和最大宽：min-width和max-width

* __min-width__
	
	_值：_  <length> | <percentage> | inherit

	_初始化：_ 0

	_应用在：_ 一切元素除了非替换行内元素，table-rows以及row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值

* __max-width__
	
	_值：_  <length> | <percentage> | none | inherit

	_初始化：_ 0

	_应用在：_ 一切元素除了非替换行内元素，table-rows以及row groups

	_可继承：_ 不可以

	_百分比：_ 根据包含块的宽

	_媒介：_ 可见媒介

	_计算值：_ 指定的百分比或者是绝对的值或者none

这两个属性允许作者使得内容宽在一个确定的范围中。他们的值有如下意思：

* __<length>__
	
	为使用的宽指定一个固定的最小或者最大值。

* __<percentage>__
	
	指定百分比。百分比的值是根据他生成盒的包含块的宽计算的。如果包含块的宽是负值的话，那么使用值就是0.如果包含块的宽取决于元素的宽的 话，那么在CSS2.1中布局出来的结果就是undefined。

* __none__
	
	（只能用在max-width上）这个盒没任何宽度限制。

min-width，max-width如果是负值的话是不合法的。

在CSS2.1中，在table, inline tables, table cells, table columns, 和column groups上的min-width和max-width效果是未定义的。

下边的算法描述了这两个属性是怎么影响width属性的值的：

1. 暂时的使用的宽是根据上面的‘计算宽和marin’的规则计算出来的（没有min-width和max-width）。

1. 如果暂时的使用的宽比max-width要打，再次应用上边的规则，但是这次width的计算值是max-width的计算值。

1. 如果结果宽比min-width要小的话，再次应用上边的规则，但是这次width的计算值是min-width的计算值。

然而，对于有原始比例且width和height都是auto的替换元素来说，算法是这样的：

从下边的table中选择可以适当的违反约束constraint violation来计算height和width的值。就拿max-width和max-height为max(min,max)，控制min ≤ max。在表中，w和h是代表忽略min-width,min-height, max-width和max-height的属性的情况下计算的width和height的结果。通常会有原始的width和height，但是他们可能不在替换元素且有原始比例的这种情况下。

![表](https://github.com/dolymood/blog/raw/master/pics/002.png)