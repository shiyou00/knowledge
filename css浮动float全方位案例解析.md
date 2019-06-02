## 前言
浮动最早的使用是出自`<img src="#" align="right" />`，用于文本环绕图片的排版处理。当然也是一种常用的布局方式。

## float 浮动
> 浮动元素脱离普通流，然后按照指定方向，向左或者向右移动，碰到父级边界或者另外一个浮动元素停止

```
值: left | right | none | inherit(继承)
初始值: none
应用于: 所有元素
继承性: 无
```


## 浮动特性
【浮动流】  
正常流中元素一个接一个排列；浮动元素也构成浮动流

效果展示：
<iframe style="width: 100%; height: 132px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f1" frameborder="0"></iframe>

默认是display:block 独占一行，当使用float的时候可以看到形成了一个浮动流。

【块级框】  
浮动元素自身会生成一个块级框，而不论这个元素本身是什么，使浮动元素周围的外边距不会合并

效果展示：
<iframe style="width: 100%; height: 216px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f2" frameborder="0"></iframe>

【包裹性】  
浮动元素的包含块是指其最近的块级祖先元素，后代浮动元素不应该超出包含块的上、左、右边界。若不设置包含块的高度，包含块若浮动，则包含块会延伸，进而包含其所有后代浮动元素；若不设置包含块的宽度，包含块若浮动，则包含块宽度由后代浮动元素撑开。

通俗的理解就是：子元素若不设置浮动，父元素如果没有设置高度的话，则是由子元素撑开，如果子元素设置了浮动，父元素的高度则不会由子元素撑开。除非父元素也是浮动或者清除浮动，都可以让父元素包裹住子元素

效果展示：
<iframe style="width: 100%; height: 260px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f3" frameborder="0"></iframe>

【破坏性】  
浮动元素脱离正常流，并破坏了自身的行框属性，使其包含块元素的高度塌陷，使浮动框旁边的行框被缩短，从而给浮动框留出空间，行框围绕浮动框重新排列

效果展示：
<iframe style="width: 100%; height: 176px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f4" frameborder="0"></iframe>

浮动的主要特性就是以上几点，动起手来多点点案例，就会对这个特性更加熟悉。

效果展示：
<iframe style="width: 100%; height: 230px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f5" frameborder="0"></iframe>
