## 前言
之所以要清除浮动，就是解决浮动元素造成的一些问题，如包含块高度塌陷，浮动流造成的布局混乱等问题。

## clear清除
```
值: left | right | both | none | inherit
初始值: none
应用于: 块级元素(块级元素指block元素，不包括inline-block元素)
继承性: 无
```

```
left:左侧不允许存在浮动元素
right:右侧不允许存在浮动元素
both:左右两侧不允许存在浮动元素
none:允许左右两侧存在浮动元素
```

[注意]设置clear属性的元素并不能改变浮动元素，而只能改变自身

CSS2.1引入了一个清除区域，清除区域是在元素上外边距之上增加的额外间隔，不允许任何浮动元素进入这个范围，这意味着元素设置clear属性时，它的外边距不改变

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f6" frameborder="0"></iframe>

对于标准浏览器来说，清浮动其实就两种方法:
  
一种是在浮动元素下面添加新元素设置clear属性;  

另一种是触发包含块的`BFC`，使其包含浮动元素。而对于IE7-浏览器，则用到其特有属性`haslayout`

【clear属性】
```
[1]<div style="clear:both"></div>

[注意]并不是很适用，若包含块为<ul>，则子元素只能为<li>，则在<li>后面添加<div>元素不合适

[2]<br style="clear:both">

[注意]虽然clear属性只应用于块级元素，但在除IE7-以外的其他浏览器都可以将clear属性应用于<br>元素

[3]为浮动元素的after伪元素设置clear属性
.clear:after{content:""; display: block; clear: both;}
```

[注意]IE7-浏览器不支持after伪元素

【BFC】
触发条件：
```
[1]float: left/right

[2]position:absolute/fixed

[3]display:inline-block/table-cell/table-caption/flex

[4]overflow:hidden/scroll/auto
```

【IE7-】
关于IE7-浏览器有一个其特有的属性`haslayout`，当触发包含块的`haslayout`时，浮动元素被`layout`元素自动包含
触发条件：
```
[1]display:inline-block

[2]height/width:除auto外

[3]float: left/right

[4]position: absolute

[5]writing-mode: tb-rl

[6]zoom: 除normal外
```

## 兼容写法

【版本1】
```
.clear:after{content:""; display: block; clear: both;}
.clear{zoom: 1;} // IE6、7兼容
```

【版本2】
```
.clearfix:before,.clear:after{content:"";display:table;} // 因为display: table会产生一些匿名盒子，这些匿名盒子的其中一个（display值为table-cell）会形成Block Formatting Context。
.cleafixr:after{clear:both;}
.clearfix{zoom:1} // IE6、7兼容
```

效果展示：
<iframe style="width: 100%; height: 364px;" src="https://shiyou00.github.io/lion/dist/html/css-float/float.html?case=f7" frameborder="0"></iframe>
