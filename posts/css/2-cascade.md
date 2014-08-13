##  CSS 层叠 Cascading

### 指定值、计算值和实际值

一旦用户代理（一般指浏览器）把一个文档解析然后生成文档树，他就必须对树上的每一个元素根据目标媒介类型所使用的每一个属性指定一个值。

得到某个属性的最终值需要经过下面的4步计算：

* 通过指定值确定这个值

* 将这个值分解为一个可以用来继承的值，也就是计算后的值，计算值

* 在需要的情况下，将计算值转换成一个绝对的值，也就是使用值

* 最终，根据本地环境的限制，把值转换为实际值

<!--more-->

#### 指定值

用户代理们给每一个属性去指定一个值，也就是指定值，必须要经过下面的3步（按照优先顺序排列）：

1. 如果层叠的结果是一个值，那么就使用这个值。
1. 其他情况：如果那个属性的值是继承而来的，并且当前元素不是文档树中的根元素，此时使用的是其父元素的计算值。
1. 其他情况就使用设定的初始值。每一个属性的初始值是在其定义的时候指定的值。

#### 计算值

指定值是在层叠的过程中被解析成计算值。例如，URI会被解析成绝对地址，'em'和'ex'等单位会被计算成pixel'像素'或者绝对长度。计算一个值是不需要用户代理（浏览器）来渲染文档的。


如果UA不能解析一个URI为绝对地址的话，那么这个URI的计算值就是初始值。

一个属性的计算的值是在其在这个属性定义‘计算值’一行所标明的值。如果指定值是'inherit'继承的时候，请参见下边的关于继承部分。

如果一个属性不合适元素的时候，计算值其实还是存在的。然而，一个元素的某个属性的计算值适不适用可能取决于当前元素的其他属性。简单例子就是left top等值的计算值适不适用，可能取决于position属性的值。

#### 使用值

在处理计算值的时候，文档还是没被格式化的，因此有些值可能是无法确定的。例如，一个元素的宽度设置成了相对于其包含块的百分比的话，这个宽度的值就需要等到其包含块确定了才能够确定。使用值是将计算值和有依赖关系的值最终转化成绝对的值。

#### 实际值

此时使用的值基本上就是渲染所需要的值。用户代理可能无法在给定的环境中使用这些值。例如，在某些代理上只能渲染整数值的border，但是计算出的值可能是小数，所以就不能按照小数来显示；或者某些代理可能被迫只能使用黑白色，而不是显示全彩色。所以实际值可能是使用值经过一定的取舍才能使用。

### 继承

就像之前所描述的，在文档树中的一个元素的子元素们的一些值可能是继承自该元素的。每一个属性定义值取决于他是否是继承的。

_假设现在有一个H1元素，他内部包含着一个EM元素：_

``` html
<H1>The headline <EM>is</EM> important!</H1>
```

_如果EM元素没有指定color属性的话，EM中的‘is’就会继承自他的父元素H1，所以说如果H1元素的属性color是蓝色的话，EM元素的color也会是蓝色的。_

继承是基于计算值继承的。所以父元素的计算值就成了她的子元素们的指定值和计算值。

_例如，看下边的样式：_

```css
body { font-size: 10pt }
h1 { font-size: 130% }
```

_然后文档片段是这样的：_

```html
<BODY>
  <H1>A <EM>large</EM> heading</H1>
</BODY>
```

_H1元素的'font-size'属性的实际值就是'13pt'。这是因为'font-size'的计算值是可继承的，EM元素的计算值同样也是'13pt'。如果当前代理对于font中的13pt不可用的话，那么H1和EM的'font-size'的实际值都可能是'12pt'（仅仅只是示例）。_

#### 'inherit'值

每一个属性都可能有一个级联的值'inherit'，这也就意味着对于一个给定的元素，这个属性的指定值和其父元素的这个属性值是一样的。'inherit'值可以用来实现继承值，也可以用到一些特殊的继承上。

如果'inherit'值被设定在了根元素上，那么属性值就是他的初始值。

_在下边的这个例子中，在BODY元素上有'color'和'background'属性。其他的元素，'color'的值就是继承而来的，而background则是透明的了。如果这些规则是用户代理的一部分的话，整个文档看起来都是白底黑字。_

```css
body {
  color: black !important; 
  background: white !important;
}
* {
  color: inherit !important; 
  background: transparent !important;
}
```

### @import规则

@import规则允许用户从其他样式表中导入样式规则。在CSS2.1中，任何的@import规则必须在其他规则之前（除了@charset）。当用户代理必须忽略@import规则的详情可以参见之后关于内容解析部分。@import这个关键字之后必须是需要包含进来的样式表的URI。字符串也是可以包含在内的，如果url(...)这样的是会被解析的。

_下边的这几行就是@import规则的示例（一个是url(...)一个是字符串）：_

```css
@import "mystyle.css";
@import url("mystyle.css");
```

所以用户代理对于不支持的媒介类型就可以不去获取资源了，用户可以根据不同的媒介类型指定@import。可以根据URI之后的按照,分割的媒介类型的条件去导入。

_下边的规则就阐述了在不同的媒介类型下@import的规则：_

```
@import url("fineprint.css") print;
@import url("bluish.css") projection, tv;
```

如果一个import没有指定任何的媒介类型的话，就可以认为此import是无条件的。指定'all'也有同样的效果。只有目标媒介和媒介列表匹配的时候导入才会生效。

当同一个样式表在多个地方被导入的时候，用户代理就必须把每一个链接处理成为单独的样式表。

### 层叠

样式表可能有3种不同的来源：作者、用户和用户代理。

* 作者：源文档中指定的样式表，也就是开发者。

* 用户：也就是使用者或者说叫阅读者；某些情况下，用户是可以去指定一些自定义的样式的。

* 用户代理：一般指浏览器，用户代理都会有默认的样式表。

这三种来源的样式表可能在范围上由重叠，他们会根据层叠的规则起作用。

CSS层叠会对每一个样式规则指定一个权重。当应用多个规则的时候，具有最大权重的具备优先权。

默认，作者样式规则比用户样式规则的权重要大。不过，对于"!important"规则，优先级就反了。所有的用户和作者规则的权重都比代理默认样式表中规则的权重大。

#### 层叠次序

为了找到元素/属性组合的值，用户代理必须应用如下的排列顺序：

1. 对于目标媒介类型，找到所有有疑问的应用到当前元素上的声明。如果相关联的选择符复合有疑问的元素并且目标媒介匹配包含声明和样式表的路径在所有链接上一致的@media规则中所有的媒介列表的话就应用声明。

1. 按照重要性（normal和important）和来源（作者、用户、代理）排序。按照如下规则排序：

	1. 用户代理声明

	1. 用户normal声明

	1. 作者正常声明

	1. 作者important声明

	1. 用户important声明

1. 拥有相同的重要性和来源的规则按照‘CSS特殊性’排序：多的特殊性选择符就会覆盖多个一般的选择符，伪元素和伪类是分别被计算到正常的元素和类的数目中的。

1. 最后，按照先后顺序来排序：如果两个声明具有相同的权重、源和CSS特殊性的话，后边的就会胜出。导入的样式表中的规则被认为出现在样式表中所有规则之前。

除了对单个声明具有"!important"，这种策略给作者的样式表比阅读器的权重更高。用户代理必须通过一个下拉菜单给与用户选择关掉作者的特殊样式表等的能力。

#### !important规则

CSS尝试创建一个介于作者和用户样式表之间的均衡的能力。默认，在作者中的样式将会覆盖在用户代理中的样式。

然而，为了平衡，一个"!important"的声明将会比一个正常的声明更优先。在作者和用户的样式表中都可能包含"!important"声明，用户的"!important"规则将会覆盖作者"!important"规则。这个CSS特性提高了用户应对特殊需求控制优先级的能力。

声明一个简写属性（复合属性）例如background具有"!important"等同于给他的每一个子属性加了"!important"。

_在下边的例子中，用户样式表的第一个规则包含了"!important"声明，所以就会覆盖作者的样式表。第二个声明一样会胜出因为有"!important"标记。然而，用户样式表中的第三个规则不含有"!important"，所以就输给了作者样式表中的第二条规则（简写属性）。同样的，第三条规则也会败给作者规则，因为他有"!important"。这证明了"!important"声明在用户样式表中依旧起作用的能力。_

```css
/* 用户样式表 */
p { text-indent: 1em ! important }
p { font-style: italic ! important }
p { font-size: 18pt }

/* 作者样式表 */
p { text-indent: 1.5em !important }
p { font: normal 12pt sans-serif !important }
p { font-size: 24pt }
```

#### 计算特殊性 - CSS特殊性

按照下边的步骤计算一个选择符的特殊性：

* 如果这个声明是来自于属性样式中的选择符，那么就记为1，否则是0。（=a）

* 计算选择符中的ID属性的个数。（=b）

* 计算选择符中的其他属性以及伪类的个数。（=c）

* 计算选择符中的元素名和伪元素出现的个数。（=d）

特殊性仅仅是由选择符的形式。特殊的，一个选择符的形式"[id=p33]"将会被计算成一个属性选择符（a=0,b=0,c=1,d=0），虽然他包含了ID。

把四个数字连在一起就能够得到其特殊性。

_一些示例：_

```
*             {}  /* a=0 b=0 c=0 d=0 -> specificity = 0,0,0,0 */
li            {}  /* a=0 b=0 c=0 d=1 -> specificity = 0,0,0,1 */
li:first-line {}  /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */
ul li         {}  /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */
ul ol+li      {}  /* a=0 b=0 c=0 d=3 -> specificity = 0,0,0,3 */
h1 + *[rel=up]{}  /* a=0 b=0 c=1 d=1 -> specificity = 0,0,1,1 */
ul ol li.red  {}  /* a=0 b=0 c=1 d=3 -> specificity = 0,0,1,3 */
li.red.level  {}  /* a=0 b=0 c=2 d=1 -> specificity = 0,0,2,1 */
#x34y         {}  /* a=0 b=1 c=0 d=0 -> specificity = 0,1,0,0 */
style=""          /* a=1 b=0 c=0 d=0 -> specificity = 1,0,0,0 */
<HEAD>
<STYLE type="text/css">
  #x97z { color: red }
</STYLE>
</HEAD>
<BODY>
<P ID=x97z style="color: green">
</BODY>
```

_在上面的示例中，P元素的颜色将会是绿色的。在"style"中的声明覆盖掉了样式中的。_

#### 非CSS显示的优先级

在一个HTML源文档中，用户代理（浏览器）应该选择显示属性。如果这样的话，这些属性将会被转换成非CSS特殊性为0的规则，并且会以他们插入在样式表中的顶端来对待。他们可能被后续的样式规则所覆盖。

对于HTML来说，任何不属于以下列表中的属性都需要以显示属性对待：abbr, accept-charset, accept, accesskey, action, alt, archive, axis, charset, checked, cite, class, classid, code, codebase, codetype, colspan, coords, data, datetime, declare, defer, dir, disabled, enctype, for, headers, href, hreflang, http-equiv, id, ismap, label, lang, language, longdesc, maxlength, media, method, multiple, name, nohref, object, onblur, onchange, onclick, ondblclick, onfocus, onkeydown, onkeypress, onkeyup, onload, onload, onmousedown, onmousemove, onmouseout, onmouseover, onmouseup, onreset, onselect, onsubmit, onunload, onunload, profile, prompt, readonly, rel, rev, rowspan, scheme, scope, selected, shape, span, src, standby, start, style, summary, title, type (except on LI, OL and UL elements), usemap, value, valuetype, version

_下边的例子当中的用户样式表将会覆盖在所有文当中的'b'元素的font weight，并且会覆盖XML文档中的带有color属性的'font'元素的颜色。在HTML中，他不会影响到任何带有color属性的'font'元素：_

```css
b { font-weight: normal; }
font[color] { color: orange; }
```

_然而，接下来的示例中，在所有的文档中都覆盖font元素的颜色：_

```css
font[color] { color: orange ! important; }
```


