## CSS 生成内容、自动编号以及列表

在某些情况下，作者可能想要用户代理渲染不是来自于文档树中的内容。一个类似的例子就是有序列表；作者不想单独列出这些数字，他想用户代理自动创建他们。简单的，作者可能想用户代理在图的标题之前插入标题单词，或者在第七章的标题之前加上第七章。特别对于音频或者盲文，用户代理应该能插入这些字符串。

在CSS2.1中，有两种机制来生成内容：

* content属性，和伪元素:before和:after结合使用。

* display的属性是list-item的元素。

<!--more-->

### :before和:after伪元素

作者可以指定:before和:after伪元素生成内容的样式以及的位置。如同他们的名字所表明的，:before和:after伪元素指定了这个元素文档树内容之前和之后的内容的位置。content属性和伪元素:before和:after结合，指定插入什么内容。

_例如，下边的规则在每一个有class属性是note的P元素内容之前插入字符串Note: _

```css
p.note:before { content: "Note: " }
```

一个元素生成的格式化对象包含生成的内容。因此，对于例子来说，改变上边的样式：

```css
p.note:before { content: "Note: " }
p.note        { border: solid green }
```

将会导致在一个绿色的border绕着这个段落渲染，包含初始的字符串。

:before和:after伪元素会继承他依附的在文档树中的元素的可继承的属性。

_例如，下边的规则在每一个Q元素之前插入一个open quote的标志。这个标志的颜色是红色red，但是font会和Q元素的一样：_

```css
q:before {
  content: open-quote;
  color: red
}
```

在:before和:after伪元素声明中，非继承的属性会使用他们的初始值。

_因此，对于例子来说，由于display的属性的初始值是inline，所以上一个例子中的quote时作为一个行内盒插入的。下一个示例明确的设置了display属性的值是block，所以插入的文本变成了一个块：_

```css
body:after {
    content: "The End";
    display: block;
    margin-top: 2em;
    text-align: center;
}
```

:before和:after伪元素和其他元素互相影响，如果他们是插入到他们关联的元素里真实的元素。

_例如，下边的文档片段和样式表：_

```
<p> Text </p>
p:before { display: block; content: 'Some'; }
```

_和下边的文档和样式表一样的效果：_

```
<p><span>Some</span> Text </p>  
span { display: block }
```

_类似的，下边的文档树和样式表：_

```
<h2> Header </h2>     
h2:after { display: block; content: 'Thing'; }
```

_和下边的文档树和样式表渲染的结果是一样的：_

```
h2> Header <span>Thing</span></h2> 
h2 { display: block; }
span { display: block; }
```

### content属性

* __content__
	
	_值：_  normal | none | [ <字符串string> | <通用资源标识符uri> | <计数器counter> | attr(<标识符identifier>) | open-quote | close-quote | no-open-quote | no-close-quote ]+ | inherit

	_初始化：_ normal

	_应用在：_ :before和:after伪元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 所有媒体

	_计算值：_ 在元素上，通常计算成normal。在:before和:after中，如果指定的是normal，计算成none。其他情况如果是URI值的话，就是绝对的URI；对于attr()值，就是结果字符串；其他的关键词和指定一样。

这个属性用来和:before和:after伪元素一起生产文档中的内容。他的值有如下含义：


* __none__
	
	伪元素不生成。

* __normal__
	
	对于伪元素来说生成的是none。

* __<字符串string>__
	
	文本内容。

* __<通用资源标识符uri>__
	
	uri指定一个外部的资源（例如图片）。如果用户代理不能显示这个资源的话他必须不管他就好像他没被指定或者显示指出这个资源不能显示出来。

* __<计数器counter>__
	
	可以通过两种不同的方法指定：counter()或者counters()。之前的这个有两中形式：counter(name)或者counter(name, style)。生成的文本就是这个伪元素所在的作用域中出现的name的最里边的次数的值；他是以指定的style格式化（默认decimal十进制）。后边的函数也有两种形式：counters(name, string)或者counters(name, string, style)。生成的文本就是这个伪元素所在的作用域中出现的name的总共的次数的值，按照string分隔从最外边到最里边。counters也是以指定的style格式化（默认decimal十进制）。请看下边的‘自动计数和编号’段落了解更多。name的值不能是none、inherit或者initial。这样的name会导致声明会忽略。

* __open-quote 和 close-quote__
	
	这些值会被下文的‘quotes’属性中的对应的字符串。

* __no-open-quote and no-close-quote__
	
	没有内容，但是会递增（或者递减）嵌套quotes的层级。

* __attr(X)__
	
	这个函数将一个选择符主体的属性X的值作为字符串返回。字符串不会被CSS处理器解析掉。如果主体选择符没有这样的属性X，返回空字符串。属性名是否敏感取决于文档语言。

display属性控制内容实在一个块或者行内盒中放置。

_下边的规则导致字符串Chapter: 在H1元素之前生成：_

```css
H1:before { 
  content: "Chapter: ";
  display: inline;
}
```

作者可以通过在content属性之后的一个字符串中写入"\A"转义序列来使得生成的内容中包含换行。这个新插入的换行依旧受white-space属性控制。关于"\A"转义序列的更多信息请看‘字符串’和‘字符和大小写’。

```css
h1:before {
    display: block;
    text-align: center;
    white-space: pre;
    content: "chapter\A hoofdstuk\A chapitre"
}
```

生成内容不会改变文档树。特别是他不会反馈到文档语言处理器。

### 引用语标记（引号）

在CSS2.1中，作者可以在样式敏感和上下文相关的规矩中指定用户代理如何渲染引用语标记。quotes属性指定了嵌入式引文的双引号层级。content属性可以访问这些引号使得他们和插入到一个引号之前和之后。

#### 指定通过quotes属性指定quotes（引号）

* __quotes__
	
	_值：_  [<字符串string> <字符串string>]+ | none | inherit

	_初始化：_ 取决于用户代理

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性指定了任意数量嵌套的引用语标记。他的值有如下含义：


* __none__
	
	content属性中的open-quote和close-quote的值不会产生任何的引用语标记。

* _[<字符串string> <字符串string>]+_

	content属性中的open-quote和close-quote的值来自于这个列表中的双引号。第一个（最左边）的那对代表引号的最外层，第二对就是嵌套的第一级等等。用户代理必须通过嵌套的等级来应用相应的一对引用语标记。

_例如，应用下边的样式表：_

```css
/* Specify pairs of quotes for two levels in two languages */
q:lang(en) { quotes: '"' '"' "'" "'" }
q:lang(no) { quotes: "«" "»" '"' '"' }

/* Insert quotes before and after Q element content */
q:before { content: open-quote }
q:after  { content: close-quote }
```

_到下边的HTML片段：_

```html
<HTML lang="en">
  <HEAD>
  <TITLE>Quotes</TITLE>
  </HEAD>
  <BODY>
    <P><Q>Quote me!</Q>
  </BODY>
</HTML>
```

_将会使得用户代理产生：_

```
"Quote me!"
```

_这样的HTML片段：_

```html
<HTML lang="no">
  <HEAD>
  <TITLE>Quotes</TITLE>
  </HEAD>
  <BODY>
    <P><Q>Trøndere gråter når <Q>Vinsjan på kaia</Q> blir deklamert.</Q>
  </BODY>
</HTML>
```

_将会产生：_

```
«Trøndere gråter når "Vinsjan på kaia" blir deklamert.»
```

#### 利用content属性插入quotes

引用语标记是使用content属性的open-quote和close-quote值来插入到文档中的适当的位置。每一个出现的open-quote或者close-quote都会被quotes的一个值代替，基于嵌套的深度。

open-quote引用第一对quotes，close-quote引用第二对。使用哪一对quotes取决于quotes嵌套的层级：在出现内容之前所有的生成文本open-quote出现的次数，减去close-quote出现的次数。如果深度为0，使用第一对，如果深度是1， 那么使用第二对，等等。如果深度比对总数还要大，最后的那对重复即可。一个close-quote或者no-close-quote可以使得深度是负值，这是错误的且会被忽略：深度保持在0，且没有引号渲染。

一些在印刷商的样式需要open引号一直在每一个段落重复，跨越好几个段落，但是只有最后的段落以close引号结束。在CSS中，这可以通过插入"phantom"close引号获得。关键词no-close-quote可以降低引用层级，但是不会插入引号。

_下边的样式表给每一个blockquote下的段落加上了一个open引号，且在最后插入了一个close引号：_

```css
blockquote p:before     { content: open-quote }
blockquote p:after      { content: no-close-quote }
blockquote p.last:after { content: close-quote }
```

_最后的段落需要一个class是last做标记。_

为了对称，还有一个no-open-quote关键词，不会插入什么东西，但是会升高引用语深度加一。

### 自动计数和编号

在CSS2.1中自动编号由两个属性控制：counter-increment和counter-reset。这些属性定义的计数器在content属性中以counter()和counters()函数使用。


* __counter-reset__
	
	_值：_  [ <标识符identifier> <整数integer>? ]+ | none | inherit

	_初始化：_ none

	_应用在：_ 所有元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 所有媒体

	_计算值：_ 和指定值一样

* __counter-increment__
	
	_值：_  [ <标识符identifier> <整数integer>? ]+ | none | inherit

	_初始化：_ none

	_应用在：_ 所有元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 所有媒体

	_计算值：_ 和指定值一样

counter-increment属性接收一个或者多个计数器的名字（标识符），每一个后边可选的跟着一个整数。这个整数按照每一个出现的元素的计数器增加了多少。默认的增量是1。0和负整数也是可以的。

counter-reset属性页包含了一个或者多个计数器名字的列表，每一个后边可选的跟着一个整数。这个整数给定了每一个出现的元素设置的计数器的值。默认值是0。

关键词none、inherit和initial不能作为计数器名字。如果值是none，意味着没有计数器重置、增加。如果值是inherit，那么就意味着有他自身通常的含义。initial是为了未来使用而保留的。

_这个例子针线了一种计数章节和段落的方式。_

```css
BODY {
    counter-reset: chapter;      /* Create a chapter counter scope */
}
H1:before {
    content: "Chapter " counter(chapter) ". ";
    counter-increment: chapter;  /* Add 1 to chapter */
}
H1 {
    counter-reset: section;      /* Set section to 0 */
}
H2:before {
    content: counter(chapter) "." counter(section) " ";
    counter-increment: section;
}
```

如果一个元素increment/reset一个计数器，且也使用了他（在:before和:after伪元素的content属性中），计数器是在incremented/reset之后使用。

如果一个元素同时reset和increment一个计数器，这个计数器就先reset，然后incremented。

如果同样的计数器在counter-reset喝counter-increment属性值中被指定了多次，这个计数器每一个reset/increment都是按照指定的顺序处理。

_下边的例子会重置section计数器为0：_

```css
H1 { counter-reset: section 2 section }
```

_下边的例子会给chapter计数器增加3：_

```css
H1 { counter-increment: chapter chapter 2 }
```

counter-reset属性遵循层叠的规则。因此，由于层叠，下边的样式表：

```css
H1 { counter-reset: section -1 }
H1 { counter-reset: imagenum 99 }
```

只会重置imagenum。为了重置两个计数器，他们必须一起制定：

```css
H1 { counter-reset: section -1 imagenum 99 }
```

#### 嵌套计数器和作用域

计数器是"self-nesting自嵌套"的，从这个意义上来讲，重新在子元素或者伪元素上的计数器会自动创建一个新的计数器额实例。这是很重要的尤其是在HTML中的列表中，那些在他们自身之中可以嵌套任意深度的元素们。对于每一层级去创建唯一的命名计数器是不可能的。

_因此，下边的就满足了多层嵌套列表项的计数。这个结果和在li元素上这是display:list-item和list-style:inside很像：_

```css
OL { counter-reset: item }
LI { display: block }
LI:before { content: counter(item) ". "; counter-increment: item }
```

每一个计数器的作用域在文档中的第一个元素开始，这个元素有这个计数器的counter-reset且包含了这个元素的后代和他的后代之后的同级元素们。然而，他不包含一个计数器（和之后这个元素的同级兄弟元素或者之后这个 由创建counter-reset的具有相同名字的计数器）的作用域中其他任何元素。

如果一个元素或者伪元素上的counter-increment或者content引用了一个不在任何任何counter-reset作用域上的计数器的话，实现上的行为应该和在这个元素或者伪元素上设置了counter-reset为0一样。

在上边的例子中，OL会创建一个计数器，他的所有的孩子都会应用那个计数器。

_如果我们用item[n]表示item计数器的第n<sup>th</sup>个实例，且用{和} 表示这个作用域的开始和结束的话，那么下边的HTML片段会使用指示的计数器（我们假设样式表和上边的一样）。_

```html
<OL>                    <!-- {item[0]=0        -->
  <LI>item</LI>         <!--  item[0]++ (=1)   -->
  <LI>item              <!--  item[0]++ (=2)   -->
    <OL>                <!--  {item[1]=0       -->
      <LI>item</LI>     <!--   item[1]++ (=1)  -->
      <LI>item</LI>     <!--   item[1]++ (=2)  -->
      <LI>item          <!--   item[1]++ (=3)  -->
        <OL>            <!--   {item[2]=0      -->
          <LI>item</LI> <!--    item[2]++ (=1) -->
        </OL>           <!--                   -->
        <OL>            <!--   }{item[2]=0     -->
          <LI>item</LI> <!--    item[2]++ (=1) -->
        </OL>           <!--                   -->
      </LI>             <!--   }               -->
      <LI>item</LI>     <!--   item[1]++ (=4)  -->
    </OL>               <!--                   -->
  </LI>                 <!--  }                -->
  <LI>item</LI>         <!--  item[0]++ (=3)   -->
  <LI>item</LI>         <!--  item[0]++ (=4)   -->
</OL>                   <!--                   -->
<OL>                    <!-- }{item[0]=0       -->
  <LI>item</LI>         <!--  item[0]++ (=1)   -->
  <LI>item</LI>         <!--  item[0]++ (=2)   -->
</OL>                   <!--                   -->
```

_下边的是另外一个例子，展示了当计数器使用在了不嵌套的元素上时作用域scope是怎样工作的。这展示了上边的计数章节和段落的样式表是如何应用给定标记的。_

```html
                     <!--"chapter" counter|"section" counter -->
<body>               <!-- {chapter=0      |                  -->
  <h1>About CSS</h1> <!--  chapter++ (=1) | {section=0       -->
  <h2>CSS 2</h2>     <!--                 |  section++ (=1)  -->
  <h2>CSS 2.1</h2>   <!--                 |  section++ (=2)  -->
  <h1>Style</h1>     <!--  chapter++ (=2) |}{ section=0      -->
</body>              <!--                 | }                -->
```

counters()函数生成一个由作用域中的所有的具有相同名字的计数器组成的字符串，按给定的字符串分隔。

_下边的样式表计数了嵌套列表项，像1, 1.1, 1.1.1这样。_

```css
OL { counter-reset: item }
LI { display: block }
LI:before { content: counters(item, ".") " "; counter-increment: item }
```

#### 计数器样式

默认，计数器被格式化为十进制数字，但是对于list-style-type属性可用的值也同样适用于计数器的样式。该表示法是：

	counter(name)

对于默认样式，或者：

	counter(name, <列表样式类型list-style-type>)

所有的样式都是允许的，包括disc，circle，square和none。

```css
H1:before        { content: counter(chno, upper-latin) ". " }
H2:before        { content: counter(section, upper-roman) " - " }
BLOCKQUOTE:after { content: " [" counter(bq, lower-greek) "]" }
DIV.note:before  { content: counter(notecntr, disc) " " }
P:before         { content: counter(p, none) }
```

#### display:none元素的计数器

一个不能显示的（display:none）的元素不能increment增加或者reset重置一个计数器。

_例如，使用下边的样式表，带有class属性是secret的H2元素不会增加count2_

```css
H2.secret {counter-increment: count2; display: none}
```

不能生成的伪元素同样不能增加或者重置一个计数器。

_例如，下边的就不会增加heading_

```css
h1::before {
    content: normal;
    counter-increment: heading;
}
```

另一方面，元素设置了visibility:hidden，是会增加计数器的。

### 列表

CSS2.1中提供了基本的可视化列表。一个元素的display:list-item就会为元素的内容生成一个主块盒，且会根据list-style-type和list-style-image可能会生成一个标记盒作为一个列表项元素的可见的表示。

list属性描述了列表的基本的可视格式化：他们允许样式表指定标记的样子（image，glyph或者number），且标记的位置是相对于主盒的（在他外边或者在他里边但是在内容之前）。他们不允许作者对列表标记指定独特样式（color，font等）或者调整他相对主盒的位置；这些可能从主盒得到。

background属性只能应用到主盒上；一个outside的标记盒是透明的。

#### 列表：list-style-type，list-style-image，list-style-position和list-style属性

* __list-style-type__
	
	_值：_  disc | circle | square | decimal | decimal-leading-zero | lower-roman | upper-roman | lower-greek | lower-latin | upper-latin | armenian | georgian | lower-alpha | upper-alpha | none | inherit

	_初始化：_ disc

	_应用在：_ display:list-item的元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性指定了列表项标记（如果list-style-image是none或者图片的URI不能显示）的外观。none指定的是没有标记，其他情况他们有三种标记类型：glyphs字形, numbering systems计数系统和alphabetic systems字母系统。

字形可以用disc，circle和square指定。他们实际上渲染的样子取决于用户代理。

计数系统可以用如下的值指定：

* __decimal__

	十进制数字，以1开始。

* __decimal-leading-zero__

	由初始的0填充的十进制数字（01, 02, 03, ..., 98, 99）。

* __lower-roman__

	小写的罗马数字（i, ii, iii, iv, v等）。

* __upper-roman__

	大写的罗马数字（I, II, III, IV, V等）。

* __georgian__

	传统的格鲁吉亚编号（an, ban, gan, ..., he, tan, in, in-an, ...）。

* __armenian__
	
	传统的大写亚美尼亚编号。

字母系统可以用如下值指定：

* __lower-latin或者lower-alpha__
	
	小写的ascii字母（a, b, c, ... z）。

* __upper-latin或者upper-alpha__
	
	大写的ascii字母（A, B, C, ... Z）。

* __lower-greek__

	小写的古典希腊 alpha， beta， gamma...（α, β, γ, ...）。

本规范没有定义字母系统怎样包装末尾的字母。例如，在26个列表项之后，小写拉丁文渲染时未定义的，对于常列表，我们建议作者指定真实数字。

CSS2.1没有定义列表计数如何重置或者增加的。希望可以在CSS的列表模块定义[CSS3LIST](http://www.w3.org/TR/CSS21/refs.html#ref-CSS3LIST)。

_例如，下边的HTML片段：_

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
   <HEAD>
     <TITLE>Lowercase latin numbering</TITLE>
     <STYLE type="text/css">
          ol { list-style-type: lower-roman }   
     </STYLE>
  </HEAD>
  <BODY>
    <OL>
      <LI> This is the first item.
      <LI> This is the second item.
      <LI> This is the third item.
    </OL>
  </BODY>
</HTML>
```

_可以产生如下样子：_

```
  i This is the first item.
 ii This is the second item.
iii This is the third item.
```

_列表标记对齐（这里 右对齐）取决于用户代理。_

* __list-style-image__
	
	_值：_  <统一资源标识符uri> | none | inherit

	_初始化：_ none

	_应用在：_ display:list-item的元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 绝对的URI或者none

这个属性指定了列表项标记使用的图片。当图片可用时，他就会代替list-style-type中的标记。

图片尺寸计算遵循如下规则：

1. 如果图片有固有的宽和高，那么使用的宽和高就是固有的宽和高。

1. 其他情况，如果图片有一个固有的比例且有固有的高或者宽，使用的宽/高和提供的宽/高相同，且使用的丢失的那个尺寸通过尺寸和固有比例计算出来。

1. 其他情况，如果图片有固有比例，使用宽是1em，且使用的高是利用这个宽和固定比例计算出来的。如果这个计算出的高比1em大的话，那么使用的高设置为1em，然后计算宽。

1. 其他情况，如果他有固有宽就用固有宽或者1em。图片的使用的高就是如果他有固有的高的话就使用它或者1em.

_下边的例子设置了每一个列表项之前的标记是图片ellipse.png_

```css
ul { list-style-image: url("http://png.com/ellipse.png") }
```

* __list-style-position__
	
	_值：_  inside | outside | inherit

	_初始化：_ outside

	_应用在：_ display:list-item的元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

这个属性指定了标记盒相对于主盒的位置。他的值有如下含义：

* __outside__

	标记盒在主盒之外。

* __inside__

	标记盒以主盒中的第一个行内盒放置，在元素内容之前且在任何伪元素之前。

_例如：_

```html
<HTML>
  <HEAD>
    <TITLE>Comparison of inside/outside position</TITLE>
    <STYLE type="text/css">
      ul         { list-style: outside }
      ul.compact { list-style: inside }
    </STYLE>
  </HEAD>
  <BODY>
    <UL>
      <LI>first list item comes first
      <LI>second list item comes second
    </UL>

    <UL class="compact">
      <LI>first list item comes first
      <LI>second list item comes second
    </UL>
  </BODY>
</HTML>
```
_上边的示例可以格式化为：_

![示意图](http://www.w3.org/TR/CSS21/images/list-inout.png)

_在由右到左的文本中，标记就会在文本的右边_

* __list-style__
	
	_值：_  [ <'list-style-type'> || <'list-style-position'> || <'list-style-image'> ] | inherit

	_初始化：_ 参见单独的属性

	_应用在：_ display:list-item的元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 参见单独的属性

list-style是一个简写属性，可以在样式表的同一个地方设置三个属性'list-style-type', 'list-style-image' 和 'list-style-position'。 

```css
ul { list-style: upper-roman inside }  /* Any "ul" element */
ul > li > ul { list-style: circle outside } /* Any "ul" child 
                                             of an "li" child 
                                             of a "ul" element */
```

尽管作者可以直接在列表项元素（在HTML中的li）上指定list-style信息，他们应该小心的这样做。下边的规则看起来类似，但是第一个声明了一个后代选择符且第二个声明了一个子选择符。

```css
ol.alpha li   { list-style: lower-alpha } /* Any "li" descendant of an "ol" */ 
ol.alpha > li { list-style: lower-alpha } /* Any "li" child of an "ol" */
```

只用了后代选择符的作者可能得到的不是他们想要的结果。看下边的规则：

```html
<HTML>
  <HEAD>
    <TITLE>WARNING: Unexpected results due to cascade</TITLE>
    <STYLE type="text/css">
      ol.alpha li  { list-style: lower-alpha }
      ul li        { list-style: disc }
    </STYLE>
  </HEAD>
  <BODY>
    <OL class="alpha">
      <LI>level 1
      <UL>
         <LI>level 2
      </UL>
    </OL>
  </BODY>
</HTML>
```

希望的渲染时有一个层级1的带着lower-alpha标签的列表项，且层级2的带着disc标签的列表项。然而，层叠次序会导致第一个样式规则覆盖掉第二个。接下来的规则通过替换为子选择符解决了这个问题：

```css
ol.alpha > li  { list-style: lower-alpha }
ul li   { list-style: disc }
```

另一个解决方案就是仅仅给列表类型元素指定list-style：

```css
ol.alpha  { list-style: lower-alpha }
ul        { list-style: disc }
```

LI元素会从OL和UL元素哪里继承list-style的值。这是指定列表样式信息推荐的做法。

_一个URI的值可以和其他值合并的，像：_

```css
ul { list-style: url("http://png.com/ellipse.png") disc }
```

_上边的例子中，当这个图片不可用的时候就会使用disc。_

在list-style中有none的值，不管设置到了list-style-type还是list-style-image，就不会有其他的值指定为none。然而，如果另外的也指定了，那么这个声明就是错误的（因此被忽略）。

_例如，list-style属性的none将list-style-type和list-style-image都设置为了none：_

```css
ul { list-style: none }
```

_结果就是不会显示任何的列表项标记。_