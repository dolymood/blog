## CSS fonts

### 简介

设置font属性会是样式表最常见的用途。不幸的是，不存在明确的和公认的分类法来分类font，且某一方面适用于一种font family字体名称序列不一定适用于其他的。例如，Italic通常用于标注斜体文本，但是斜体文本也可以通过Oblique, Slanted, Incline, Cursive或者Kursiv来标注。因此，这不是一个简单的问题将一个字体选择属性映射到一个特定的font上。

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
	
	_值：_  [[ \<family-name\> | \<generic-family\> ] [, <family-name>| \<generic-family\>]* ] | inherit

	_初始化：_ 取决于用户代理

	_应用在：_ 所有元素

	_可继承：_ 可以

	_百分比：_ 不可用

	_媒介：_ 可见媒体

	_计算值：_ 和指定值一样

