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
	
	可以通过两种不同的方法指定：counter()或者counters()。之前的这个有两中形式：counter(name)或者counter(name, style)。生成的文本就是这个伪元素所在的作用域中出现的name的最里边的次数的值；他是以指定的style格式化（默认decimal十进制）。后边的函数也有两种形式：counters(name, string)或者counters(name, string, style)。生成的文本就是这个伪元素所在的作用域中出现的name的总共的次数的值，按照string分隔从最外边到最里边。counters也是以指定的style格式化（默认decimal十进制）。请看下边的‘自动计数和编号’段落了解更多。name的值不能是none、inherit或者initial。这样的name会导致声明被拒绝（无效）。

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

open-quote引用第一对quotes，close-quote引用第二对。