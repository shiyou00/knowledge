## 前言
> 所有文档元素都生成一个矩形框，这称为元素框(element box)，它描述了一个元素在文档布局中所占的空间大小。而且，每个框影响着其他元素框的位置和大小

![](https://img2018.cnblogs.com/blog/958602/201904/958602-20190402072451331-1108433296.png)
﻿
﻿
## 宽高
宽度width被定义为从左内边界到右内边界的距离，高度height被定义为从上内边界到下内边界的距离
﻿

在CSS中，可以对任何块级元素设置显式高度。如果指定高度大于显示内容所需高度，多余的高度会产生一个视觉效果，就好像有额外的内边距一样；如果指定高度小于显示内容所需高度，则会向元素添加一个滚动条。如果元素内容的高度大于元素框的高度，浏览器的具体行为取决于overflow属性
﻿

[注意]宽度和高度无法应用到行内非替换元素，且不能为负
﻿

【auto】
宽高和margin可以设置auto。

﻿
对于块级元素来说，宽度设置为auto，则会尽可能的宽。


元素宽度=包含块宽度—元素水平外边距-元素水平边距宽度-元素水平内边距；

﻿
高度设置为auto，则会尽可能的窄。详细来说，元素高度=恰好足以包含其内联内容的高度

﻿
[注意]如果没有显式声明包含块的height，则元素的百分数高度会重置为auto
﻿

【怪异盒模型】

IE6-浏览器的宽高定义的是可见元素框的尺寸，而不是元素框的内容区尺寸
﻿

【最大最小宽高】

设置最大最小宽高的好处是可以相对安全地混合使用不同的单位。使用百分数大小的同时，也可以设置基于长度的限制
﻿
```
min-width | min-height
值: <length> | <percentage> | inherit
初始值: 0
应用于: 块级元素和替换元素
继承性: 无
百分数: 相对于包含块的宽度(高度)
﻿
max-width | max-height
值: <length> | <percentage> | inherit
初始值: none
应用于: 块级元素和替换元素
继承性: 无
百分数: 相对于包含块的宽度(高度)
```


[注意]IE6-浏览器不支持min-width | min-height | max-width | max-height

[注意]当最小宽度(高度)大于最大宽度(高度)时，以最小宽高的值为准
﻿
## 内边距
相比于盒模型的其他属性(如在定位中经常使用负值的margin，因为CSS3的到来重获光彩的border等)，padding显得中规中矩了很多，没有什么兼容性，也没有一些特殊的问题
﻿
对于行内元素，左内边距应用到元素的开始处，右内边距应用到元素的结尾处，垂直内边距不影响行高，但会影响自身尺寸，加背景颜色可以看出
﻿
[注意]内边距不能是负值
﻿
padding
值:[<length> | <percentage>]{1,4} | inherit
初始值: 未定义
应用于: 所有元素
继承性: 无
百分数: 相对于包含块的width
﻿
【50%】
块级元素通过padding:50%可以实现正方形的效果，因为水平和垂直padding的百分比值都是相对于包含块的宽度决定的，常常用于移动端头图
﻿
如果是内联元素使用padding:50%，必须配合font-size:0，因为使用inline元素的垂直padding会出现"幽灵空白节点"，也就是规范中"strut"。所以通过font-size:0使其尺寸为0
﻿
[块元素效果展示](https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f1)

<iframe style="width: 100%; height: 150px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f1" frameborder="0"></iframe>

行内元素：选择font-size = 0; 去除“幽灵空白节点”
[效果展示](https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f2 "效果展示")
﻿
【表单】
1、所有浏览器input/textarea/button都内置padding
2、部分浏览器select下拉内置padding，firefox、IE8+可以设置padding
3、除IE10-以外的其他浏览器，radio/checkbox单选复选框无内置padding，且无法设置padding。IE10-浏览器的radio/checkbox单选复选框有内置padding，且可以设置padding
[注意]除IE10-以外的其他浏览器，radio/checkbox单选复选框无内置border，且无法设置border
﻿
【button兼容】
1、在firefox浏览器中，设置padding:0，按钮左右两侧依然有padding，这时需要使用firefox自有样式
```
button::-moz-focus-inner{padding:0;}
```
﻿
2、IE7-浏览器下文字越多，左右padding逐渐变大，设置overflow:visible可解决该问题
﻿
3、button按钮的padding与高度计算不兼容
```
button{
    line-height:20px;
    padding:10px;
    border:none;
}
﻿
//结果为：
IE7: 45px
firefox:42px
chrome/IE8+:40px
﻿
```
﻿
可以使用label标签来实现类似的效果，然后把按钮button进行可访问性隐藏即可
```
<button id="btn"></button>
<label for="btn">按钮</label>
﻿
label{
    display:inline-block;
    line-height:20px;
    padding:10px;
    border:none;
}
﻿
//结果为：
IE7: 40px
firefox:40px
IE8+:40px
chrome:40px
```
﻿
## 外边距
设置外边距margin会在元素外创建额外的空白，空白通常指不能放其他元素的区域，而且在这个区域中可以看到父元素的背景
﻿
外边距可以应用到行内元素，上下外边距对行高没有任何影响。由于上下外边距实际上是透明的，所以这个声明没有任何视觉效果。左外边距应用到元素开始处；右外边距应用到元素结束处
﻿
margin
值:[<length> | <percentage> | auto]{1,4} | inherit
初始值: 未定义
应用于: 所有元素
继承性: 无
百分数: 相对于包含块的width
﻿
[注意]对于普通元素来说，包含块就是块级父级元素，对于定位元素来说，包含块是定位父级。所以，普通元素的margin百分比相对于块级父级元素的width，定位元素的margin百分比相对于定位父级的width
﻿
[注意]margin负值的作用非常大
﻿
## 边框
元素外边距内就是元素的边框border，元素的边框是围绕元素内容的内边距的一条或多条线。边框由粗细、样式和颜色三部分组成
﻿
对于行内元素来说，边框实际上画在各行之外的下一个像素上，由于各行紧挨着，所以其边框会重叠。无论为行内元素的边框设置怎样的宽度，不会对行高有任何影响；但左右边框会分别显示在元素的开始处和结尾处
﻿
```
.d1{
    width: 200px;
    border: 1px solid red;
}    
.s1{
    border: 1px solid black;
    background-color: yellow;
    padding: 6px;    
    margin: 6px;
    font-size: 30px;
    line-height: 50px;
}
﻿
<div class="d1"><span class="s1">测试文字测试文字测试文字</span></div>
```
﻿
## box-sizing
> 在CSS中盒模型被分为两种，第一种是W3C的标准模型，第二种是IE怪异盒模型。
﻿
不同之处在于后者的宽高定义的是可见元素框的尺寸，而不是元素框的内容区尺寸。目前对于浏览器大多数元素都是基于W3C标准的盒模型，但对于表单form中的部分元素还是基于IE的怪异盒模型，如input里的radio、checkbox、button等元素，如果给其设置border和padding它们也只会往元素盒内延伸
﻿
在W3C的标准模型下，宽度和高度仅仅包含了内容宽度，除去了边框和内边距两个区域，这样为web设计师处理效果带来了不少麻烦。为了解决这个问题，CSS3新增了一个盒模型属性box-sizing，能够事先定义盒模型的尺寸解析方式
﻿
[注意]IE7-浏览器不支持
﻿
box-sizing
值: content-box | border-box | padding-box | inherit
初始值: content-box
应用于: 块级元素和替换元素
继承性: 无
﻿
[注意1]只有firefox浏览器支持padding-box属性值
[注意2]IE浏览器在getComputedStyle得到width/height是按照标准模式计算的，而不论box-sizing的取值
﻿
[效果展示](https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f3 "效果展示")