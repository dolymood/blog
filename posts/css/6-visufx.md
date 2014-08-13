## CSS 可视化效果（Visual effects）

### overflow 和 clipping 

一般一个块盒的内容是限制在这个盒的内容边界的。在某些情况下，一个盒可能溢出overflow，意味着他的内容会部分或者完全的在这个盒的外边。例如：

* 不能换行的行，导致这个行盒会比块盒还要大。

* 对于一个包含块来说一个块级盒是超大的。当一个元素的width属性的值导致生成的块盒溢出包含块边界的时候就会发生。

* 一个元素的高要比包含块的显式指定的高还要高（例如，包含块的高是由height属性决定的，不是内容高）。

* 子盒设是绝对定位的，且部分在盒的外边。这样的盒并不是总是被他们的祖先的overflow属性截掉；特殊情况下，他们不会被在他们自身和他们的包含块之间的任何的祖先的overflow截掉。

* 子盒有负margin，导致他部分定位到了盒子的外边。

* text-indent属性导致一个行内盒和块盒的左边界或者右边界拉开一定距离。

<!--more-->

当溢出发生时，overflow属性指定了一个盒是否从其padding边界裁掉，如果是的话，这个时候就提供滚动机制允许用户访问任何的截掉的内容。

#### 溢出：overflow属性

* __overflow__
	
	_值：_  visible | hidden | scroll | auto | inherit

	_初始化：_ visible

	_应用在：_ 块容器

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

这个属性指定了一个块容器元素在其溢出这个元素的盒的时候，内容是否裁掉。他影响这个元素的所有的内容除了包含块（或者祖先）是viewport的子元素。overflow的值有如下含义：


* __visible__
	
	这个属性表明内容不会被裁剪，他可能超出块盒渲染。

* __hidden__
	
	这个属性表明内容被裁剪，且不应该提供用户能够看到裁剪区域意外的内容。

* __scroll__
	
	这个属性表明内容不会被裁剪，且如果用户代理使用了 滚动机制的话，那么在屏幕上时可以看的到的（例如滚动条），这种机制应该是不管有或者没有内容被裁剪了都是一直显示的。这避免了滚动条出现和消失的动态过程中出现的任何问题。当这个值指定了且目标媒体是print的话，超出的内容可以打印出来。

* __auto__
	
	auto的行为由用户代理决定，但是针对于超出盒的情况的话应该提供滚动机制。

即使overflow设置的是visible，内容还是可以通过本机操作环境将一个用户代理截到文档窗口的。

用户代理必须对viewport的根元素设置overflow属性。当根元素是一个HTML中的‘HTML’元素或者一个XHTML中的html元素，且这个元素有一个HTML的BODY元素或者XHTML的body子元素时，如果根元素上的overflow是visible的话，用户代理必须代替第一个这样的子元素上的overflow属性应用到viewport上（也就是将overflow用到viewport上）。用到viewport上的overflow必须是auto值。针对于这个传递的元素必须有一个使用值overflow：visible。

这种情况下在元素的边界会有一个滚动条出现，他应该是插在内border边界到外padding边界之间。滚动条占据这个元素的生成的包含块的空间。

_考虑下边的示例：_

```html
<div>
<blockquote>
<p>I didn't like the play, but then I saw
it under adverse conditions - the curtain was up.</p>
<cite>- Groucho Marx</cite>
</blockquote>
</div>
```

_下边的是应用的样式：_

```css
div { width : 100px; height: 100px;
      border: thin solid red;
      }

blockquote   { width : 125px; height : 100px;
      margin-top: 50px; margin-left: 50px; 
      border: thin dashed black
      }

cite { display: block;
       text-align : right; 
       border: none
       }
```

_overflow的初始值是visible，所以blockquot是不会被裁切的，像这样：_

![示意图](http://www.w3.org/TR/CSS21/images/overflow1.png)

_设置div的overflow的值是hidden，这样就导致了blockquote被div裁掉了：_

![示意图](http://www.w3.org/TR/CSS21/images/overflow2.png)

_scroll值告诉用户代理支持可见的滚动机制来显示布局，这样用户就能看到被裁切的内容了。_

最后，考虑绝对定位元素和其overflow的父元素复合的情况。

样式表：

```css
container { position: relative; border: solid; }
scroller { overflow: scroll; height: 5em; margin: 5em; }
satellite { position: absolute; top: 0; }
body { height: 10em; }
```

文档片段：

```html
<container>
 <scroller>
  <satellite/>
  <body/>
 </scroller>
</container>
```

在这个例子中，scroller元素不会滚动satellite元素，因为latter的包含块是溢出要被裁切和滚动的元素的外边。

#### 裁切：clip属性

一个裁切的区域定义了一个元素的border盒的那部分是可见的。默认，元素不会被裁切掉。然而，裁切区域可以被clip属性指定。

* __clip__
	
	_值：_  <形状shape> | auto | inherit

	_初始化：_ auto

	_应用在：_ 绝对定位元素

	_可继承：_ 不可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 如果指定的是auto就是auto，其他情况是一个有4个值（每一个值来说，如果是auto，就是auto，否则的话就是指定的值）的矩形。

clip属性仅仅对于绝对定位元素有用。他的值有如下含义：

* __auto__
	
	元素不裁切。

* __<形状shape>__
	
	在CSS2.1中，仅仅可用的<形状shape>是:rect(top, right, bottom, left)，top和bottom指定了到这个盒的border边界的顶部的偏移量，right和left则是指定了这个盒的border边界的左边界的偏移量。作者应该按逗号分隔这几个偏移值。用户代理必须支持逗号分隔，但是也可以支持不用逗号分隔，因为本规范之前的规范是模棱两可的。

	top、right、bottom和left可以是一个长度length也可以是auto。负的长度length是不可以的。值auto意味着裁切的区域大小和这个元素生成的border盒的边界一样。

	当坐标接近像素级别坐标时，应该注意的是当left和right的值相同时没有像素是可见的（或者top和bottom的值是一样的），且相反的当这些值是auto时这个元素的border盒中的像素都是可见的。

一个元素的裁切区域会裁掉这个元素在裁切区域之外的任何的方面（例如，内容，孩子，background，border，文本装饰，outline以及可视的滚动机制等任何的东东）。被裁切掉的内容不会导致overflow。

这个元素的祖先也可以裁切掉他们内容的一部分（例如，通过他们的clip属性，如果他们的overflow属性不是visible）；渲染的就是累积的交集。

如果裁切区域溢出了用户代理的文档窗口的边界，内容还是可以通过本机操作环境截到窗口的。

_例子：下边的两条规则：_

```css
p#one { clip: rect(5px, 40px, 45px, 5px); }
p#two { clip: rect(5px, 55px, 45px, 5px); }
```

_且假设p都是50 * 55像素的，会各自创建如下的dashed线确定的裁切区域的矩形：_

![示意图](http://www.w3.org/TR/CSS21/images/clip.png)

### 可见性：visibility属性

* __visibility__
	
	_值：_  visible | hidden | collapse | inherit

	_初始化：_ visible

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒介

	_计算值：_ 和指定值一样

visibility属性指定了一个元素创建的盒是否可见。不可见的盒子仍然影响布局。他的值有如下含义：

* __visible__
	
	生成的盒可见。

* __hidden__
	
	生成的盒不可见。

* __collapse__
	
	请查阅在table中的‘动态行和列效果’段落。如果使用的元素不是rows、row groups、columns或者column groups，collapse和hidden是一个意思。

这个属性可以用在利用脚本来创建动态效果。

_在下边的例子中，按下form中的任何的button都会触发一个作者定义的导致相应的盒可见或者不可见的脚本函数。由于这些盒有相同的尺寸和位置，效果上来说就是一个代替另一个。_

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
<HEAD><TITLE>Dynamic visibility example</TITLE>
<META 
 http-equiv="Content-Script-Type"
 content="application/x-hypothetical-scripting-language">
<STYLE type="text/css">
<!--
   #container1 { position: absolute; 
                 top: 2in; left: 2in; width: 2in }
   #container2 { position: absolute; 
                 top: 2in; left: 2in; width: 2in;
                 visibility: hidden; }
-->
</STYLE>
</HEAD>
<BODY>
<P>Choose a suspect:</P>
<DIV id="container1">
   <IMG alt="Al Capone" 
        width="100" height="100" 
        src="suspect1.png">
   <P>Name: Al Capone</P>
   <P>Residence: Chicago</P>
</DIV>

<DIV id="container2">
   <IMG alt="Lucky Luciano" 
        width="100" height="100" 
        src="suspect2.png">
   <P>Name: Lucky Luciano</P>
   <P>Residence: New York</P>
</DIV>

<FORM method="post" 
      action="http://www.suspect.org/process-bums">
   <P>
   <INPUT name="Capone" type="button" 
          value="Capone" 
          onclick='show("container1");hide("container2")'>
   <INPUT name="Luciano" type="button" 
          value="Luciano" 
          onclick='show("container2");hide("container1")'>
</FORM>
</BODY>
</HTML>
```