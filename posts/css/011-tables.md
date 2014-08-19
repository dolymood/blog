## CSS 表格

### 表格table简介

本章定义了CSS中table的处理模型。处理模型的一部分是布局。对于布局，本章介绍了两种算法：第一，很好定义了的固定table布局算法；第二，自动table布局算法，在本规范中还没有完全的定义好。

对于自动table布局算法，一些已发布的实现者已经了相对接近的互操作性。

table布局可以用来变现数据之间的表格式的关系。作者在文档语言中指定这些关系，且可以使用CSS2.1指定他们的表现。

在可见媒体上，CSS表格table也可以用于实现特殊的布局。在这种情况下，作者不应该在文档语言中使用table相关的元素，而应该应用CSS到相关的结构元素上来实现想要的布局。

作者可以指定一个table的可视化格式（作为一个矩形的网格单元格）。单元格的行和列可以组织成为行组和列组。行、列、行组、列组以及单元格的周围可以有border（在CSS2.1中有2中border模型）。作者可以将一个单元格中的数据水平或者垂直对齐，且可以将一行或者一列中的所有的单元格中的数据对齐。

<!--more-->

_这里有一个简单的3行3列的表格：_

```html
<TABLE>
<CAPTION>This is a simple 3x3 table</CAPTION>
<TR id="row1">
   <TH>Header 1  <TD>Cell 1  <TD>Cell 2
<TR id="row2">
   <TH>Header 2  <TD>Cell 3  <TD>Cell 4
<TR id="row3">
   <TH>Header 3  <TD>Cell 5  <TD>Cell 6
</TABLE>
```

_上边的代码创建了一个table，3行，3个表头单元格，以及6个数据单元格。注意这个例子中的3列是这样指定实现的：在table中有很多列是作为表头和数据单元格而需要的。_

_下边的CSS规则使得表头单元格中的文本水平居中且font weight是bold：_

```css
th { text-align: center; font-weight: bold }
```

_下边的规则指定了最上边的那一行会有3px的solid样式的蓝色border，每一行将会有一个1px的solid的黑色border：_

```css
table   { border-collapse: collapse }
tr#row1 { border: 3px solid blue }
tr#row2 { border: 1px solid black }
tr#row3 { border: 1px solid black }
```

_注意，然而在行接触的地方他们的border是覆盖的。我们将在后端的‘ 边框冲突解决方案’段落讨论。_

_下边的规则将table的标题放在table之上：_

```css
caption { caption-side: top }
```

前面的示例展示了CSS如何和HTML4元素一起工作的；在HTML4中，各种各样的table元素（TABLE, CAPTION, THEAD, TBODY, TFOOT, COL, COLGROUP, TH以及TD）的语义都是很好定义过的。在其他的文档语言中（例如XML应用）就可能没有预定义的table元素。因此，CSS2.1中允许作者通过display属性来“映射”文档语言元素为table元素。例如，下边的规则使得FOO元素像是HTML的TABLE元素且BAR元素像是CAPTION元素：

```css
FOO { display : table }
BAR { display : table-caption }
```

我们将会在下边的段落中讨论table元素语义。在本规范中，table元素术语指的是创建table涉及到的任何的元素。一个内部的table元素是指产生一个行、行组、列、列组或者单元格的元素。

### CSS表格模型

CSS表格模型是基于HTML4的表格模型的，在HTML4的表格模型中，一个表格的结构和表格视觉的布局是很接近的。在这种模型中，一个表格由一个可选的标题和任意单元格行。这个表格模型称为“行优先”因为作者在文档语言中明确的指定了行，不是列。只有所有的行都指定完了以后列才会由此推断出———每一行的第一个单元格属于第一列，第二个单元格属于第二列等等———。行和列可以按结构分组，且分组会反映在表现上（例如，一组行的周围可能会有一个border）。

因此，表格模型由tables表格、captions标题、rows行、row groups行组（包含表头header groups和表尾footer groups）、columns列、 column groups列组和cells单元格组成。

CSS模型不需要文档语言包含和那些组件中的每一个都相应的元素。对于没有预定义的table元素的文档语言（例如XML应用），作者必须映射文档语言元素到table元素；这可以通过display属性来做。下边的display的值指定了任意元素的表格格式化规则：

* __table （在HTML中：TABLE）__

	指定一个元素定义块级表格：他是参与在一个BFC中的一个矩形块。

* __inline-table （在HTML中：TABLE）__

	指定一个元素定义行内级表格：他是参与在一个IFC中的一个矩形块。

* __table-row （在HTML中：TR）__

	指定一个元素是多个单元格的组成的行。

* __table-row-group （在HTML中：TBODY）__

	指定一个元素是一个或者多个行组成的组。

* __table-header-group （在HTML中：THEAD）__

	和table-row-group很像，但是对于可视格式化，这个行组总是在其他所有的行、行组之前和任何的顶部标题之后显示。如果一个表格包含了多个display是table-header-group的元素，只有第一个是以头header来渲染的；其他的以table-row-group来渲染。

* __table-footer-group （在HTML中：TFOOT）__

	和table-row-group很像，但是对于可视格式化，这个行组总是在其他所有的行、行组之后和任何的低部标题之	前显示。如果一个表格包含了多个display是table-footer-group的元素，只有第一个是以尾footer来渲染的；其他的以table-row-group来渲染。

* __table-column （在HTML中：COL）__

	指定一个元素描述了多个单元格组成的列。

* __table-column-group （在HTML中：COLGROUP）__

	指定一个元素由一个或者多个列形成的组。

* __table-cell （在HTML中：TD, TH）__

	指定一个元素代表一个表格单元格。

* __table-caption （在HTML中：CAPTION）__

	指定一个表格的标题。所有的display是table-caption的元素必须渲染，如同下边‘可视化格式模型中的表格’章节所描述的那样。

用这些display值替换的元素在布局期间以他们给定的display类型对待。例如，一个图片设置了display:table-cell将会填充可用的单元格空间，且他的尺寸可能对于表尺寸的算法有贡献，就如同一个普通的单元格。

display设置为了table-column或者table-column-group的元素不会渲染，但是他们是有用的，因为他们可以有引起一个他们所代表的列的确定样式的属性。

HTML4默认的样式表会使用如下值：

```css
table    { display: table }
tr       { display: table-row }
thead    { display: table-header-group }
tbody    { display: table-row-group }
tfoot    { display: table-footer-group }
col      { display: table-column }
colgroup { display: table-column-group }
td, th   { display: table-cell }
caption  { display: table-caption }
```

对于HTML表格元素，用户代理可以忽略这些display属性，因为HTML表格可以使用其他向后兼容的呈现算法来渲染。然而，这并不意味着阻止在HTML中的非表格元素使用display:table。

### 列columns

表格单元格可能属于两个上下文：行和列。然而，在源文档中的单元格是行的后代，永远不是列的后代。不过一些方面的单元格能受列属性的影响。

下边应用到列和列组元素上的属性：

* __border__

	只有当表格元素的border-collapse属性的值是collapse的时候各式各样的border属性才会应用到列上。在那种情况下，设置在列和列组上的border通过选择每一个单元格边界的border样式的‘冲突解决方案算法’解决。

* __background__

	background背景属性设置了在列中的单元格的背景，但是只有单元格和行的背景是透明的情况下。请看下文的‘表格层和透明’。

* __width__
	
	width属性给定了这个列的最小宽。

* __visibility__
	
	如果列的visibility属性设置为了collapse，在列中的任何单元格都不会渲染，且跨越到其他列的单元格会被裁切掉。另外，表格的宽width是由列的占用的宽决定的。请看下边的‘动态效果’。其他的visibility的值没有任何效果。

_这里有一些设置列属性的样式表的例子。前2个规则一起实现了HTML4中值为cols的“规则”属性。第3个规则使得totals列背景是蓝色的，最后的2个规则展示了怎样通过‘固定布局算法’来制作一个固定尺寸列宽。_

```css
col { border-style: none solid }
table { border-style: hidden }
col.totals { background: blue }
table { table-layout: fixed }
col.totals { width: 5em }
```

### 可视化格式模型中的表格

在可视化格式模型中，一个表格可以表现得像一个块级或者行内级元素。

在两种情况下，表格生成了一个主要的块盒称为，表格包裹盒table wrapper box（他包含了表格盒自身以及任何的标题盒）。表格盒是一个包含着表格的内部的表格盒的块级盒。标题盒是保留他们自己的内容content，padding，margin以及border区域，且以表格包裹盒内部普通块盒来渲染的块级盒。标题盒放在表格盒之前或者之后是由caption-side属性决定的。

如果表格式块级的，那么表格包裹盒就是一个块盒；如果是行内级的话就是一个inline-block盒。表格包裹盒生成一个BFC。当对于inline-table的垂直对齐的时候使用表格盒。表格包裹盒的宽度就是在其内部的表格盒的border边界的宽。表格上的width和height属性是相对于表格包裹盒的包含块的，而不是表格包裹盒自身。

TABLE元素的position，float，margin-*，top，right，bottom和left属性的计算值是使用在表格包裹盒上而不是表格盒；其他非继承的属性值则不是表格包裹盒。（表格TABLE元素值不使用在TABLE表格上也不使用在表格包裹盒上，使用的是他们的初始值。）

![表格示意图](http://www.w3.org/TR/CSS21/images/table_container.png)

#### 标题位置和对齐

* __caption-side__
	
	_值：_  top | bottom | inherit

	_初始化：_ top

	_应用在：_ table-caption元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

这个属性指定了标题盒对于表格盒的位置。他的值有如下含义：

* __top__

	把标题盒定位到表格盒之上。

* __bottom__

	把标题盒定位到表格盒之下。

标题盒子中的标题内容的水平对齐使用text-align属性。

_在这个例子中，caption-side属性使得标题放置在了表格下边。这个标题将会和表格的父元素一样宽，且标题文本是左对齐的。_

```css
caption { caption-side: bottom; 
          width: auto;
          text-align: left }
```

### 表格内容的可视化布局

内部的表格元素生成带有content和border的矩形盒。单元格一样有padding。内部的表格元素没有margin。

这些盒的可视化布局是由一个矩形（不规则的行和列网格）管理的。每一个盒占据所有的网格单元格，有下边的规则确定。这些规则不会应用到HTML4或者HTML早起的版本；HTML利用他自身的行和列跨度的限制。

1. 每一个表格行盒占据网格单元格一行。所有的表格行盒一起按照源文档中的他们出现的顺序填满表格的顶部到底部。

1. 行组占据着他包含的表格行相同的网格单元格。

1. 列盒占据一个或者多个网格单元格的列。列盒按照他们出现的顺序一个挨着一个放置。第一个列盒可能在左边也可能在右边，这取决于表格上的direction属性的值。

1. 列组盒占据他包含着的表格列相同的网格单元格。

1. 单元格可以跨越几行或者几列。每一个单元格是一个矩形盒，一个或者多个网格单元格的宽和高。这个行中矩形的顶行通过这个单元格的父级决定。这个矩形必须离左边尽可能的远，但是在第一列中的部分单元格占据空间必须不能和其他的单元格盒重叠，且这个单元格必须在同一行中的所有的单元格（在源文档中之前更早的）的右边。如果这个位置会导致一个多列单元格重叠一个之前的多行单元格，CSS没有定义结果：实现者可以覆盖单元格也可以移动之后的单元格到右边来避免这样的重叠。（如果表格的direction属性值是ltr的情况下该约束有用，如果direction是rtl的话，将之前两句中的left和right互换。）

1. 一个单元格盒不能扩展的超过一个表格或者一个行组的最后一个表格行盒；用户代理必须简短他直至他适合了。

在‘折叠的border模型’中的行，列，行组和列组的边界和假设的单元格的border为中心的网格行一致。在‘分开的border模型中’，这个边界和单元格的border边界一致。

_这里有一个说明规则5的示例。下边的(X)HTML：_

```html
<table>
<tr><td>1 </td><td rowspan="2">2 </td><td>3 </td><td>4 </td></tr>
<tr><td colspan="2">5 </td></tr>
</table>
```

_用户代理可以随意的可见覆盖单元格，如同左边的图形，或者移动这个单元格避免可见重叠，如同右边的图形。_

![示意图](http://www.w3.org/TR/CSS21/images/table-overlap.png)

#### 表格层和透明

为了达到找到每一个单元格背景的目的，不同的表格元素可以认为是在6层的叠加层上。在一个层中一个元素设置的背景只有当在其之上的层有透明背景的时候才可见。

![层](http://www.w3.org/TR/CSS21/images/tbl-layers.png)

1. 最下边的层是一个单独的平面，代表的是表格盒自身。像其他盒一样，他可以是透明的。

1. 下一层包含着列组。每一个列组拓展自顶行的顶部单元格到底行的底部的单元格的且从他的最左边的列的左边界到最右边的列的右边界。背景background覆盖在这个列组中的所有的单元格的完全的整个区域，即使他们跨度超出了列组，但是在区域中的不同之处在于不会影响背景图的定位。

1. 列组的上边是代表着列盒的区域。每一个列金额；列组一样高和列中的普通单元格一样宽。背景覆盖在列中出现的所有的单元格的完整区域，即使他们的跨度超出了列，但是在区域中的不同之处在于不会影响背景图的定位。

1. 下一个就是包含着行组的层。每一个行组拓展自从第一列的最左边单元格的左上角到最后的列的最下边的右下角。

1. 下一个就是包含着表格行的层。每一个行和行组是一样宽的且和在行中的普通单元格一样高。和列一样，背景覆盖在行中出现的所有的单元格的完整区域，即使他们的跨度超出了行，但是在区域中的不同之处在于不会影响背景图的定位。

1. 最顶的层包含着单元格自身。如同图形所表现的，尽管所有的行包含着同样数目的单元格，并非每一个单元格有指定的内容。在‘分开的border模型’中，如果empty-cells属性的值是hide的话，这些空的单元格就会透明穿过单元格、行、行组、列和列组的背景，让表格背景透过展示。

“丢失的单元格”是在行或者列网格中不被一个元素或者伪元素占用的单元格。丢失的单元格的渲染如同有一个匿名的table-cell盒占用了他们在网格中的位置。

_下边的例子中，第一行包含了4个非空的单元格，但是第二行仅仅只包含了一个非空的单元格，因此表格背景透过来了，除了从第一行中跨进这一行的单元格。下边的HTML代码和样式规则_

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
  <HEAD>
    <TITLE>Table example</TITLE>
    <STYLE type="text/css">
      TABLE  { background: #ff0; border: solid black;
               empty-cells: hide }
      TR.top { background: red }
      TD     { border: solid black }
    </STYLE>
  </HEAD>
  <BODY>
    <TABLE>
      <TR CLASS="top">
        <TD> 1 
        <TD rowspan="2"> 2
        <TD> 3 
        <TD> 4 
      <TR>
        <TD> 5
        <TD>
    </TABLE> 
  </BODY>
</HTML>
```

_可能格式化为：_

![示意图](http://www.w3.org/TR/CSS21/images/tbl-empty.png)

注意如果表格有border-collapse: separate，通过border-spacing属性给定的这个区域的背景往往是表格元素的背景。参见‘分开的border模型’。

#### 表格宽度算法：table-layout属性

由于CSS没有定义表格“最佳的”布局由于在很多种情况下，什么是最佳的是品味的问题。CSS没有定义当布局表格时用户代理必须遵守的约束条件。用户代理可以使用任何的他们希望这样做的算法，且可以自由的更喜欢渲染速度，超过精度，除了当选择了“固定布局算法”。

注意本段覆盖了[之前章节](http://blog.aijc.net/~posts/css/005-visudet.md)‘计算width和margin’中描述的计算宽的规则。尤其是，如果表格的margin是0，且width是auto，表格是不会自动的将尺寸填充到他的包含块的。然而，一旦表格的width的计算值找到了，那么就应用之前章节中描述的规则。因此，例如，一个表格能通过设置margin的left和right的值是auto来实现居中。

未来CSS的更新可能会介绍一些使得表格自动填充他们的包含块的方式。

* __table-layout__
	
	_值：_  auto | fixed | inherit

	_初始化：_ auto

	_应用在：_ table和inline-table元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

table-layout属性控制用于布局表格单元格、行、和列的算法。他的值有如下含义：

* __fixed__

	使用固定的表格布局算法。

* __auto__

	使用任何自动表格布局算法。

这两种算法在下边描述。

##### 固定表格布局

用这种（快速的）算法，表格水平方向的布局不依赖于单元格内容；他仅取决于表格的宽度，列的宽以及border或者单元格边框间的距离。

表格的宽可以通过width属性明确的指定。值如果是auto，那么就意味着使用自动表格布局算法。然而，如果表格是在普通流中块级的table，用户代理可以（非必须）使用[之前章节](http://blog.aijc.net/~posts/css/005-visudet.md)‘计算width和margin’下的‘普通流中的块级，非替换元素’中描述来计算宽且应用到固定布局的表格上即使指定的宽是auto。

_如果当width是auto时用户代理支持固定表格布局的话，下边的规则创建一个表格，宽度比他的包含块小4em：_

```css
table { table-layout: fixed;
        margin-left: 2em;
        margin-right: 2em }
```

在固定表格布局算法中，每一列的宽度按如下规则确定：

1. 一个列元素的width不是auto，设置列的宽度就是设置的宽。

1. 其他，这个列中第一行的一个单元格的宽width不是auto，那么就使用它。如果单元格跨越了多列，宽度就会按列平分。

1. 任何剩余的列等于平分下剩余的水平表格空间（最小的border或者单元格间距）。

然后表格的宽大于表格元素的width属性的值和所有列的宽度之和。如果表格比列要宽，剩余的空间应该平分到列中。

如果一个之后的行有多列，且比table-column决定的元素数和第一行决定的数目要大，那么额外的列可以不渲染。如果他们渲染了，CSS2.1中没有定义列和表格的宽。当使用table-layout: fixed时，作者不应该省略第一行的列。

在这种方式中，一旦第一行完整的收到后用户代理就可以布局表格了。之后的行中的单元格不影响列宽。任何的内容溢出了的单元格使用overflow属性决定内容是否被裁切。

##### 自动表格布局

在这种算法中，表格的宽是由他的列给定的。这种算法影响了几个流行的HTML用户代理写本规范的行为。在table-layout是auto的情况下，用户代理不需要实现这个算法来决定表格布局。即使结果又不同的表现，他们也能使用其他的算法。

自动表格布局必须仅仅包括包含块和表格内容以及任何的他的后代的宽度。

这个算法可能是效果低的因为在确定最终的布局之前他用户代理需要使用表格中所有内容且可能会不止一次。

列宽的确定按如下规则：

1. 计算每一个单元格的最小内容宽（MCW）：格式化后的内容可能会跨好几行但是不会溢出这个单元格盒。如果这个单元格指定的宽（W）比MCW大，那么W就是最小单元格的宽。值如果是auto，意味着MCW是最小单元格宽。
	
	同样也计算每一个单元格的最大单元格宽：不换行的格式化内容除非有明确的换行符。

1. 对于每一列，从只跨那一列的单元格中确定最大和最小列宽。最小的就是最小单元格宽中的那个最大的值。最大值就是最大单元格宽。

1. 对于跨多列的每一个单元格，一起增加他跨越的列的最小宽，他们必须和单元格一样宽。对最大宽做同样的事情。如果可能的话，近似的平均增加所有的跨越的列。

1. 对于每一个宽width不是auto的列组元素来说，增加跨越列的最小宽以致于他， 加起来的宽最少和列组的宽width一样。

这给出了每一列最小和最大宽。

最小的标题宽（CAPMIN）由计算每一个标题的最小外宽outer width作为假设的包含着标题格式化为display:block的表格单元格的MCW确定。最小外宽中的最大值就是CAPMIN。

列和标题宽通过如下规则影响最终的表格宽：

1. 如果table或者inline-table元素的width属性的计算值（W）不是auto，使用值就是W，CAPMIN和所有列单元格宽之和（MIN）中的较大的值。如果使用值比MIN大，那么剩余的宽应该平分到列中。

1. 如果table或者inline-table元素的width值是auto，使用值是表格包含块宽，CAPMIN和MIN中的最大值。然而，如果CAPMIN或者所有列单元格宽之和（MAX）比包含块宽小，使用max(MAX, CAPMIN)。

一个列百分比宽相对于表格宽的。如果表格宽是auto，百分比代表了这个列宽的一个约束（UA应该尽量满足）。（显然，这并不是总是可能的：如果列的宽是110%，那么这个约束不能满足。）

#### 表格高算法

表格的高通过table或者inline-table元素的height属性给定。如果值是auto，那么就意味着高就是所有行高之和加上任何的单元格间隙或者border。其他任何值都以最小高对待。CSS2.1没有定义当height属性引起表格的高比他其他情况应该的高要大时，额外的空间怎么分配。

一个table-row元素盒的高只在用户代理拿到这一行所有的单元格都可用之后计算一次：他是这一行计算的height，这一行中每一个单元格的计算的height和单元格所需的最低的高（MIN）之中的最大值。如果一个table-row的height的值是auto，那么就意味着用于布局的行高就是MIN。MIN取决于单元格盒的高和单元格盒的对齐方式。CSS2.1没有定义当表格单元格和表格行的高是百分比的时候怎样计算高。CSS2.1没有定义行组的高的具体含义。

在CSS2.1中，一个单元格盒的高是内容所需的最小高。表格单元格的height属性能影响这一行的高，但是他不能增加单元格盒的高。

CSS2.1没有指定除了所有的行高之和必须足够包含跨行的单元格之外跨越多行的单元格怎样影响行高的计算。

每一个表格单元格的vertical-align属性决定了他相对于行的对齐方式。每一个单元格的内容都有一个baseline基线，一个top，一个middle和一个bottom，和行自身一样。在表格上下文中，vertical-align的值有如下含义：

* __baseline__

	一个单元格的baseline就是他跨越的第一行的baseline的同样的高的位置。

* __top__

	这个单元格盒的top和他跨越的第一行的top对齐。

* __bottom__

	这个单元格盒的bottom和他跨越的最后一行的bottom对齐。

* __middle__

	这个单元格盒的中心center和他跨越的中间行的中心center对齐。

* __sub, super, text-top, text-bottom, <长度length>, <百分比percentage>__

	这些值不应用到单元格上；单元格对齐用baseline代替。

一个单元格的baseline是在这个单元格中第一个在普通流中的行盒或者这个单元格中第一个在普通流中的table-row的baseline，这就要看他们谁先出现。如果没有那样的行盒或者table-row，那么baseline就和这个单元格盒的内容content边界的底部对齐。为了达到找到baseline的目的，带有滚动机制的普通流中的盒必须以他滚动到他原始位置来考虑。注意一个单元格的baseline可以在他的下border之下结束，看下边的例子。

一个单元格盒的top和通过所有的有vertical-align: baseline的单元格的baseline之间的最大距离用来设置这一行的baseline。下边的例子：

![表格baseline](http://www.w3.org/TR/CSS21/images/cell-align.png)

单元格盒1和2是和他们的baseline对齐的。单元格盒2有着在baseline之上的最大高，因此他决定了行的baseline。

如果以个表格行没有任何的单元格盒和其baseline对齐，那一行的baseline就是这一行中最下边的单元格的内容content边界的底部。

为了避免模糊不清的方案，单元格对齐按照如下顺序：

1. 首先定位和他们的baseline对齐的单元格。这回创建行的baseline。接下来定位vertical-align: top的单元格。

1. 行现在有一个top，或许还有一个baseline和一个截止到目前为止的定位了的单元格的top到最底部之间的距离的暂时的高。

1. 如果剩余的和bottom或者middle对齐单元格的高比当前行的高还要高的话，当前行的高会通过降低bottom来增加到那些单元格的最大值。

1. 最后定位剩余的单元格。

比行高小的单元格盒会扩大padding的top或者bottom。

_在这个例子中的单元格的baseline低于他的bottom border：_

```
div { height: 0; overflow: hidden; }

<table>
 <tr>
  <td>
   <div> Test </div>
  </td>
 </tr>
</table>
```

#### 列中的水平对齐

一个单元格盒中的行内级内容的水平对齐能通过这个单元格的text-align属性指定。

#### 动态行和列效果

行，行组，列和列组元素的visibility属性的值是collapse。这个值导致整个行或者列不显示，且行或者列正常情况占据的空间可以被其他内容占用。和折叠的行或者列相交的跨越多行和多列的内容会被裁切掉。然而，行或者列的抑制不会对表格布局有其他影响。这允许了移除表格中的行或者列（不强制表格重新布局）的动态效果，为了说明列约束的潜在变化。

### 边框borders

在CSS中有两种不同的设置表格单元格边框border的模型。一种是所谓的最适合分离单个单元格周围的边框。另一种就是最适合一个表格到另一个之间连续的边框。很多的边框样式能通过他们的模式实现，所以他往往都是先品尝然后才决定使用谁。

* __border-collapse__
	
	_值：_  collapse | separate | inherit

	_初始化：_ separate

	_应用在：_ table和inline-table元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

这个属性选择表格边框模式。separate值选择的是分离边框模式。collapse值选择的是折叠边框模式。这些模式在下边描述。

#### 分离边框模式

* __border-spacing__
	
	_值：_  <长度length> <长度length>? | inherit

	_初始化：_ 0

	_应用在：_ table和inline-table元素（注意frameset元素也可以应用border-spacing属性）

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 两个绝对的长度

长度指定了毗邻的单元格的分离的边框的距离。如指定了一个长度，他指定的就是水平和垂直的间隔。如果两个都指定了，那么第一个指定的就是水平间隙而第二个就是垂直间隙。长度不可以是负的。

表格边框和表格中边界单元格的边框之间的距离就是那一边的表格的padding加上相关的边框间距。例如，在右手边，这个距离就是padding-right + 水平的border-spacing。

表格的宽就是从左内padding边界到右内padding边界（包括border spacing但是除去padding和border）的距离。

然而，在HTML和XHTML1中，< table > 元素的宽确实左border边界到右border边界的距离。

在这种模式中，每一个单元格都有一个独立的边框。border-spacing属性指定的就是毗邻的单元格的border边框之间的距离。在这段距离内，row行, column列, row group行组, 和column group列组的背景background是不可见的，允许表格背景透过显现。行，列，行组和列组不能有边框。

_如下的样式表的结果和下边的图中的表格很像：_

```css
table      { border: outset 10pt; 
             border-collapse: separate;
             border-spacing: 15pt }
td         { border: inset 5pt }
td.special { border: inset 10pt }  /* The top-left cell */
```

![表格](http://www.w3.org/TR/CSS21/images/tbl-spacing.png)

##### 空单元格周围的边框和背景：empty-cells属性

* __empty-cells__
	
	_值：_  show | hide | inherit

	_初始化：_ show

	_应用在：_ table-cell元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

在分离边框模式中，这个属性控制了没有可见内容的单元格周围的边框和背景的渲染。空单元格和visibility:hidden的单元格都是没有可见内容的。除非单元格包含了如下的一条或者多条内容，否则的话这个单元格就是空的：

* 浮动内容（包含空的元素），

* 普通流中的不是被white-space属性处理成折叠的空格的内容（包含空的元素）。

当这个值是show时，边框和背景在空单元格周围（或者背后）绘制。

如果值是hidden意味着在空单元格周围（或者背后）没有边框和背景。此外，如果一行中所有的单元格的值都是hidden且没有可见内容，那么这一行的高就是0且只有这一行的一边有垂直的border-spacing。

_下边的规则导致在所有的单元格周围都有边框和背景：_

```css
table { empty-cells: show }
```

#### 折叠边框模式

在折叠边框模式中，在所有或者部分的单元格，行，行组，列和列组的周围指定边框。HTML中的rules属性的边框可以通过这样的方式指定。

边框是在所有的单元格之间的网格行的中心。用户代理必须找到一个一致的规则来四舍五入分离的单位的奇数。

下边的公式展示了表格宽，边框宽，padding和单元格宽之间是怎样相互影响的。他们之间的关系通过如下等式，使用于表格的每一行：

	row-width = (0.5 * border-width0) + padding-left1 + width1 + padding-right1 + border-width1 + padding-left2 +...+ padding-rightn + (0.5 * border-widthn)

n就是行中单元格数，padding-left(i)和padding-right(i)和单元格i相关的left padding和right padding，同理，border-width也一样。

UA必须通过审查表格的第一行的第一个和最后一个单元格来计算表格的初始的left和right的边框的宽。表格的左边框的宽就是第一个单元格的合并了的左边框的一半，右边框框就是最后一个单元格的合并了的右边框的一半。如果之后的行有更大的合并了的左和右边框的话，然后超过的就涌进这个表格的margin区域。

表格的top边框宽通过审查所有的合并了他们的top边框和表格的top边框。表格的top边框宽等于合并了top边框的最大值的一半。表格的bottom边框宽通过审查所有的合并了他们的border边框和表格的border边框。bottom边框宽等于合并了bottom边框的最大值的一半。

当决定表格溢出一些祖先的时候才考虑任何涌进margin的边框。

![表格宽](http://www.w3.org/TR/CSS21/images/tbl-width.png)

注意这种模式中，表格宽包含了表格边框的一半。同样在这种模式中表格是没有padding的（但是有margin）。

CSS2.1没有定义表格元素的背景边界放在什么位置。

##### 边框冲突解决方案

在折叠边框模式中，每一个单元格的每一遍的边框可以通过多种挨着边界的元素（单元格，行，行组，列，列组合表格自身）border属性指定，且这些边框可能有不同的宽，样式style和颜色color。经验规则是在每一边选择最“引人注目的”的边框样式，除了有样式hidden无条件的关掉了border。

下边的规则决定了再冲突情况下哪一个边框样式胜出：

1. border-style是hidden的边框优于其他冲突边框。任何的有这个值的边框会抑制在这个位置的所有边框。

1. 如果边框样式是none，具有最低的优先级。除非在这个边遇见的所有元素的border属性都是none，那么边框就会省略掉（但是注意边框样式的默认值是none）。

1. 如果没有任何的样式是hidden，且最少一个不是none，那么狭窄的边框会被丢弃，取而代之的是更宽的边框。如果有多个相同的border-width，那么样式按如下顺序优先：'double', 'solid', 'dashed', 'dotted', 'ridge', 'outset', 'groove' 和最低的'inset'

1. 如边框的样式仅仅在颜色上由区别，那么单元格上的样式会战胜行上的，行上的样式战胜行组，列，列组和最后的表格。当两个元素有相同的类型type冲突时，离左边远的和；离顶部远的胜出。

_下边的例子阐述了这些优先规则的应用。样式表：_

```css
table          { border-collapse: collapse;
                 border: 5px solid yellow; }
*#col1         { border: 3px solid black; }
td             { border: 1px solid red; padding: 1em; }
td.cell5       { border: 5px dashed blue; }
td.cell6       { border: 5px solid green; }
```

_HTML：_

```html
<TABLE>
<COL id="col1"><COL id="col2"><COL id="col3">
<TR id="row1">
    <TD> 1
    <TD> 2
    <TD> 3
</TR>
<TR id="row2">
    <TD> 4 
    <TD class="cell5"> 5
    <TD class="cell6"> 6
</TR>
<TR id="row3">
    <TD> 7
    <TD> 8
    <TD> 9
</TR>
<TR id="row4">
    <TD> 10
    <TD> 11
    <TD> 12
</TR>
<TR id="row5">
    <TD> 13
    <TD> 14
    <TD> 15
</TR>
</TABLE>
```

_结果：_

![边框冲突](http://www.w3.org/TR/CSS21/images/tbl-border-conflict.png)

_这里有一个隐藏折叠（合并）边框的例子：_

![隐藏折叠（合并）边框](http://www.w3.org/TR/CSS21/images/CSStbl3.png)

_HTML：_

```html
<TABLE style="border-collapse: collapse; border: solid;">
<TR><TD style="border-right: hidden; border-bottom: hidden">foo</TD>
    <TD style="border: solid">bar</TD></TR>
<TR><TD style="border: none">foo</TD>
    <TD style="border: solid">bar</TD></TR>
</TABLE>
```

#### border边框样式

在表格中border-style的一些值和其他元素比有一些不同的含义。下边的列表中用星号标记处来了。

* __none__

	没有边框。


* __*hidden__
	
	和none很像，但是在折叠边框模式中，也会阻止其他边框。

* __dotted__

	边框是一系列的点。

* __dashed__

	边框是一系列的短线段。

* __solid__

	边框是条实心线段。

* __double__

	边框有两条实心线。两条线和他们之间的间隙之和等于border-width。

* __groove__

	边框看起来像是刻进了画布上一样。

* __ridge__

	和groove相反：边框看起来像是从画布中出来一样。

* __*inset__

	在分离边框模式中，边框使得整个盒看起来像是嵌入在画布中一样。在折叠边框模式中，和ridge一样的效果。

* __*outset__
	
	在分离边框模式中，边框使得整个盒看起来像是从画布中出来一样。在折叠边框模式中，和groove一样的效果。