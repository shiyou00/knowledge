## 前言
> line-height、font-size、vertical-align是设置行内元素布局的关键属性。这三个属性是相互依赖的关系，改变行间距离、设置垂直对齐等都需要它们的通力合作。

## 行高 line-height
> line-height行高是指文本行基线之间的距离。

行高line-height实际上只影响行内元素和其他行内内容，而不会直接影响块级元素，也可以为一个块级元素设置line-height，但这个值只是应用到块级元素的内联内容时才会有影响。

在应用到块级元素时，line-height定义了元素文本基线之间的最小距离，即最小行高

```
line-height
值: <length> | <percentage> | <number> | normal | inherit
初始值: normal(通常line-height:normal的值为font-size值的1.2倍)
应用于: 所有元素
继承性: 有
百分数: 相对于元素的字体大小font-size
```

![](./image/23.png)


[注意]如果块级元素中的某一个子级内联元素设置的“行高”比“最小行高”大，则行框以设置“行高”来渲染；如果小，则以“最小行高”来渲染。因为，每一个子级内联元素的行高都是行内框的高度，只有一行中所有的行内元素(包括代表父级元素的匿名文本)，最大的行内框高度才能成为整行的行高。

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f4" frameborder="0"></iframe>


## 重要概念

【内容区】
> 对于行内非替换元素或匿名文本某一部分，font-size确定了内容区的高度

![](./image/24.png)

【行内框】

内容区加上行间距等于行内框。如果一个行内非替换元素的font-size为15px，line-height为21px，则相差6px。用户代理将这6像素一分为二，将其一半分别应用到内容区的顶部和底部，这就得到了行内框

![](./image/25.png)

当line-height小于font-size时，行内框实际上小于内容区

![](./image/26.png)


【行框】
> 行框定义为行中最高行内框的顶端到最低行内框底端之间的距离，而且各行框的顶端挨着上一行行框的底端

![](./image/27.png)


【框属性】  
内边距、外边距和边框不影响行框的高度，即不影响行高
行内元素的边框边界由font-size而不是line-height控制
外边距不会应用到行内非替换元素的顶端和底端

margin-left、padding-left、border-left应用到元素的开始处；而margin-right、padding-right、border-right应用到元素的结尾处

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f5" frameborder="0"></iframe>

【替换元素】  
行内替换元素需要使用line-height值，从而在垂直对齐时能正确地定位元素。因为vertical-align的百分数值是相对于元素的line-height来计算的。对于垂直对齐来说，图像本身的高度无关紧要，关键是line-height的值

默认地，行内替换元素位于基线上。如果向替换元素增加下内边距、外边距或边框，内容区会上移。

替换元素的基线是正常流中最后一个行框的基线。除非，该替换元素内容为空或者本身的overflow属性值不是visible，这种情况下基线是margin底边缘

## 垂直对齐
> vertical-align用来设置垂直对齐方式，所有垂直对齐的元素都会影响行高

```
值: baseline | sub | super | top | text-top | middle | bottom | text-bottom | <length> | <percentage> | inherit
初始值: baseline
应用于: 行内元素、替换元素、表单元格
继承性: 无
百分数: 相对于元素的行高line-height
```

[注意]IE7-浏览器中vertical-align的百分比值不支持小数行高，且取baseline、middle、text-bottom等值时与标准浏览器在展示效果不一样，常用的解决办法是将行内元素设置display:inline-block 

```
vertical-align:baseline(元素的基线与父元素的基线对齐)
vertical-align:sub(降低元素的基线到父元素合适的下标位置)
vertical-align:super(升高元素的基线到父元素合适的上标位置)
vertical-align:bottom(把对齐的子元素的底端与行框底端对齐)
vertical-align:text-bottom(把元素的底端与父元素内容区域的底端对齐)
vertical-align:top(把对齐的子元素的顶端与行框顶端对齐)
vertical-align:text-top(把元素的顶端与父元素内容区域的顶端对齐)
vertical-align:middle(元素的中垂点与父元素的基线加1/2父元素中字母X的高度对齐)
vertical-align:(+-n)px(元素相对于基线上下偏移npx)
vertical-align:x%(相对于元素的line-height值)
vertical-align:inherit(从父元素继承vertical-align属性的值)
```

效果展示：
<iframe style="width: 100%; height: 150px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f6" frameborder="0"></iframe>

【inline-block底部空隙】

inline-block元素在块级元素中留空隙就是因为图像的默认垂直对齐方式是基线对齐(基线对齐在原理上相当于图像底边与匿名文本大写英文字母X的底边对齐)；而匿名文本是有行高的，继承父级元素设置的行高，默认为normal(即font-size的1.2倍)，所以X的底边距离行框有一段距离，这段距离就是图像留出的空隙

于是，解决这个问题有以下几个解决办法

2、父级的line-height: 0  
这样使匿名文本与行框的距离为0

3、vertical-align: top/middle/bottom

内联块元素底部空隙消除演示：
<iframe style="width: 100%; height: 160px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f7" frameborder="0"></iframe>


## 应用

【1】单行文本水平垂直居中
```
div{
    line-height: 100px;
    width: 100px;
    text-align: center;
    border: 1px solid black;
}
```

[注意]好多地方都写着单行文本垂直居中是将高度和行高设置成一样的值，但高度其实是没有必要设置的。仅仅设置行高就可以，文字在一行中本身就是垂直居中显示的

效果展示：
<iframe style="width: 100%; height: 120px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f8" frameborder="0"></iframe>


【2】图片近似垂直居中
```
div{
    line-height: 200px;
    text-align: center;
}
img{
    vertical-align: middle;
}
<div>
    <img src="#" alt="#">
</div>
```
由于字符X在em框中并不是垂直居中的，且各个字体的字符X的高低位置不一致。所以，当字体大小较大时，这种差异就更明显

效果展示：
<iframe style="width: 100%; height: 160px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f9" frameborder="0"></iframe>


【3】图片完全垂直居中
在方法2的基础上设置块级元素的font-size为0，则可以设置图片完全垂直居中
```
div{
    line-height: 200px;
    text-align: center;
    font-size: 0;
}
img{
    vertical-align: middle;
}
<div>
    <img src="#" alt="#">
</div>
```
效果展示：
<iframe style="width: 100%; height: 160px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f10" frameborder="0"></iframe>



【4】多行文本水平垂直居中  
由于方法3设置font-size为0的局限性，块级元素里面无法放置文本。方法4主要通过新增元素来实现垂直居中效果，该方法也可用于图片的水平垂直居中

```
div{
    height: 100px;
    width: 200px;
    background-color: pink;
    text-align: center;
}
span{
    display:inline-block;
    vertical-align: middle;
    line-height: 20px;
    width: 100px;
}    
i{
    display: inline-block;
    height: 100%;
    vertical-align: middle;
}
 <div>
        <i></i><span>我是特别长的特别长的特别长的特别长的多行文字</span>
    </div> 
```
效果展示：
<iframe style="width: 100%; height: 300px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f11" frameborder="0"></iframe>



【5】图标和文本对齐

1、使用长度负值
```
img{
    vertical-align: -5px;
}
```
根据实践经验，20*20像素的图标后面跟14px的文字，vertical-align设置为-5px可以达到比较好的对齐效果

2、使用文本底部对齐
```
img{
    vertical-align: text-bottom;
}
```
使用baseline会使图标偏上；使用top/bottom会受到其他行内元素影响造成定位偏差；使用middle需要恰好的字体大小且兼容性不高；使用text-bottom较合适，不受行高及其他内联元素影响

效果展示：
<iframe style="width: 100%; height: 150px;" src="https://shiyou00.github.io/lion/dist/html/css-box-model/box.html?case=f12" frameborder="0"></iframe>
