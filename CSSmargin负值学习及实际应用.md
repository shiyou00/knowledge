## 前言
margin属性在实际中非常常用，也是平时踩坑较多的地方。margin折叠部分相信不少人都因为这样那样的原因中过招。margin负值也是很常用的功能，很多特殊的布局方法都依赖于它。

## 表现
虽然margin可以应用到所有元素，但display属性不同时，表现也不同

【1】block元素可以使用四个方向的margin值

【2】inline元素使用上下方向的margin值无效

【3】inline-block使用上下方向的margin负值看上去无效

[注意]inline-block使用上下方向的margin负值只是看上去无效，这与其默认的vertical-align:baseline有关系，当垂直对齐的属性值为其他值时，则会显示不同的视觉效果

效果展示：
<iframe style="width: 100%; height: 380px;" src="https://shiyou00.github.io/lion/dist/html/css-margin/margin.html?case=f7" frameborder="0"></iframe>

## 重叠
margin负值并不总是后面元素覆盖前面元素，它与元素display属性有关系

【1】两个block元素重叠时，后面元素可以覆盖前面元素的背景，但无法覆盖其内容

【2】当两个inline元素，或两个line-block元素，或inline与inline-block元素重叠时，后面元素可以覆盖前面元素的背景和内容

【3】当inline元素(或inline-block元素)与block元素重叠时，inline元素(或inline-block元素)覆盖block元素的背景，而内容的话， 后面的元素覆盖前面的元素

综上所述，个人理解，在普通流布局中，浏览器将页面布局分为内容和背景，内容的层叠显示始终高于背景。block元素分为内容和背景，而inline元素或inline-block元素，它本身就是内容(包括其背景等样式设置)

效果展示：
<iframe style="width: 100%; height: 550px;" src="https://shiyou00.github.io/lion/dist/html/css-margin/margin.html?case=f8" frameborder="0"></iframe>

## 应用

【1】水平垂直居中
如果要居中的元素的宽/高是不变的或者说是确定的，比如width/height=100px，那么设置absolute的top/left=50%，然后margin-left/margin-top=-50px即可

如果要居中的元素的宽/高是不确定的，这时margin负值就不能使用具体的px了，可以使用百分比。但由于margin的百分比都是相对于包含块的宽度，所以这里限制了只能设置宽高相同的居中元素。包含块的宽度如何获得呢？利用absolute的包裹性，在需要居中的元素外面套一个空的`<div>`元素即可
```
.box{
    position:relative;
    width: 200px;
    height: 200px;
    background-color: lightgreen;
    border: 2px solid black;
}
.out{
    position: absolute;
    left: 50%;
    top: 50%;
}    
.in{
    height: 100px;
    width: 100px;
    background-color: pink;
    margin-left: -50%;
    margin-top: -50%;
}

<div class="box">
    <div class="out">
        <div class="in">测试内容</div>    
    </div>    
</div>

```

效果展示：
<iframe style="width: 100%; height: 260px;" src="https://shiyou00.github.io/lion/dist/html/css-margin/margin.html?case=f9" frameborder="0"></iframe>


【2】列表项两端对齐

比如外层元素宽度为200px，内层3个元素，宽度为60px，margin-right为10px。这里，正常流中块级元素框的水平总和总共为210px，超过了父元素的宽度200px，则第三个元素会被挤下来。当然可以给第三个元素设置margin-right=0。但，这种方法不优雅，为布局而布局，第三个元素并没有什么特殊的，却被设置了特殊的样式

优雅的方法应该是内层元素和外层元素之间包一层元素，设置margin-right=-10px，使块级元素框的水平总和总共为210px - 10px = 200x ，等于父元素的宽度即可

<b class="pri"> [注意]设置overflow:hidden用于清除浮动 </b>

```
ul{
    margin: 0;
    padding: 0;
    list-style:none;
}
.box{
    width: 200px;
    background-color: pink;    
}
.list{
    overflow: hidden;
    margin-right: -10px;
}
.in{
    float: left;
    width: 60px;
    height: 100px;
    background-color: lightgreen;
    margin-right: 10px;
}

<div class="box">
    <ul class="list">
        <li class="in">1</li>
        <li class="in">2</li>
        <li class="in">3</li>
    </ul>    
</div>
```

效果展示：
<iframe style="width: 100%; height: 150px;" src="https://shiyou00.github.io/lion/dist/html/css-margin/margin.html?case=f10" frameborder="0"></iframe>

【3】三栏自适应布局
中间的主体使用双层标签，外层\<div>宽度100%显示，并且浮动，内层\<div>为真正的主体内容，含有左右110px的margin值。左栏和右栏都采用margin负值。左栏左浮动，margin-left为-100%，正好使左栏位于页面左侧。右栏左浮动，大小为其本身的宽度100px

```
html,body{
    height: 100%;
}
body{
    margin: 0;
}
.main{
    width: 100%;
    height: 100%;
    float: left;
}
.main .in{
    margin: 0 110px;
    background-color: pink;
    height: 100%;
}
.left,.right{
    height: 100%;
    width: 100px;
    float: left;
    background-color: lightgreen;
}
.left{
    margin-left: -100%;
}
.right{
    margin-left: -100px;
}

<body>
<div class="main">
    <div class="in"></div>
</div>
<div class="left"></div>
<div class="right"></div>
</body>
```
效果展示：
<iframe style="width: 100%; height: 720px;" src="https://shiyou00.github.io/lion/dist/html/css-margin/margin.html?case=f11" frameborder="0"></iframe>
