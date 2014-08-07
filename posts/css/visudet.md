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

