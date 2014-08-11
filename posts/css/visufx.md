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
	
	这个属性表明内容被裁剪，且不应该提供用户能够看到裁剪矩形意外的内容。

* __scroll__
	
	这个属性表明内容不会被裁剪，且如果用户代理使用了 滚动机制的话，那么在屏幕上时可以看的到的（例如滚动条），这种机制应该是不管有或者没有内容被裁剪了都是一直显示的。这避免了滚动条出现和消失的动态过程中出现的任何问题。当这个值指定了且目标媒体是print的话，超出的内容可以打印出来。

* __auto__
	
	auto的行为由用户代理决定，但是针对于超出盒的情况的话应该提供滚动机制。

即使overflow设置的是visible，内容还是可以通过本机操作环境将一个用户代理的文档窗口给截掉的。

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
