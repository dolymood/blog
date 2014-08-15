## CSS 文本text

text相关的属性会在下边的段落中定义，他们影响了字符characters，空格spaces，单词words以及段落paragraphs的可视化表现。

### 缩进：text-indent熟悉

* __text-indent__
	
	_值：_  <长度length> | <百分比percentage> | inherit

	_初始化：_ 0

	_应用在：_ 块容器

	_可继承：_ 可以

	_百分比：_ 相对于包含块的宽度width

	_媒介：_ 可见媒体

	_计算值：_ 指定的百分比或者绝对长度

<!--more-->

这个属性指定了再一个块容器中第一行文本的缩进值。更详细来说，他指定了流入块的第一个行盒的第一个盒的缩进值。这个盒根据行盒的左边界（对于由右到左的布局来说就是右边界）来缩进的。用户代理必须以空白的空格渲染缩进。

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

