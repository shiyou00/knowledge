## 前言
项目中经常碰到需要实现水平垂直居中的样式。下面就总结几种常用的方法

## 水平对齐+行高
【思路一】text-align + line-height实现单行文本水平垂直居中
```
<style>
    .f10 .test{
        text-align: center;
        line-height: 100px;
    }
</style>
<div class="case-box f10" data-case="f10">
    <div class="test" style="background-color: lightblue;width: 200px;">测试文字</div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f10" frameborder="0"></iframe>

## 水平+垂直对齐
【思路二】text-align + vertical-align  
【1】在父元素设置text-align和vertical-align，并将父元素设置为table-cell元素，子元素设置为inline-block元素  
[注意]若兼容IE7-浏览器，将结构改为\<table>结构来实现table-cell的效果；用display:inline;zoom:1;来实现inline-block的效果

```
<style>
    .f11 .parent{
        display: table-cell;
        text-align: center;
        vertical-align: middle;
    }
    .f11 .child{
        width: 80px;
        display: inline-block;
    }
</style>
<div class="case-box f11" data-case="f11">
    <div class="parent" style="background-color: gray; width:200px; height:100px;">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```
效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f11" frameborder="0"></iframe>


【2】若子元素是图像，可不使用table-cell，而是其父元素用行高替代高度，且字体大小设为0。子元素本身设置vertical-align:middle
```
<style>
    .f12 .parent{
        line-height: 200px;
        text-align: center;
        font-size:0;
    }
    .f12 .child{
        vertical-align: middle;
    }
</style>
<div class="case-box f12" data-case="f12">
    <div class="parent" style="background-color: gray; width:200px;">
        <img class="child" src="../../img/head.jpg" style="width:50px;" alt="test">
    </div>
</div>
```
效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f12" frameborder="0"></iframe>

[相关资料](https://www.cnblogs.com/shiyou00/p/10641847.html#%E5%BA%94%E7%94%A8)


## margin + vertical-align
要想在父元素中设置vertical-align，须设置为table-cell元素；要想让margin:0 auto实现水平居中的块元素内容撑开宽度，须设置为table元素。而table元素是可以嵌套在tabel-cell元素里面的，就像一个单元格里可以嵌套一个表格

[注意]若兼容IE7-浏览器，需将结构改为\<table>结构

```
<style>
    .f13 .parent{
        display:table-cell;
        vertical-align: middle;
    }
    .f13 .child{
        display: table;
        margin: 0 auto;
    }
</style>
<div class="case-box f13" data-case="f13">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f13" frameborder="0"></iframe>


## 绝对定位
【1】利用绝对定位元素的盒模型特性，在偏移属性为确定值的基础上，设置margin:auto

```
<style>
    .f14 .parent{
        position: relative;
    }
    .f14 .child{
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        height: 50px;
        width: 80px;
        margin: auto;
    }
</style>
<div class="case-box f14" data-case="f14">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f14" frameborder="0"></iframe>

【2】利用绝对定位元素的偏移属性和translate()函数的自身偏移达到水平垂直居中的效果  
[注意]IE9-浏览器不支持

```
<style>
    .f15 .parent{
        position: relative;
    }
    .f15 .child{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
</style>
<div class="case-box f15" data-case="f15">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f15" frameborder="0"></iframe>


## flex
[注意]IE9-浏览器不支持

【1】在伸缩项目上使用margin:auto
```
<style>
    .f16 .parent{
        display: flex;
    }
    .f16 .child{
        margin: auto;
    }
</style>
<div class="case-box f16" data-case="f16">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f16" frameborder="0"></iframe>


【2】在伸缩容器上使用主轴对齐justify-content和侧轴对齐align-items
```
<style>
    .f17 .parent{
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
<div class="case-box f17" data-case="f17">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f17" frameborder="0"></iframe>


## grid
[注意]IE10-浏览器不支持  
【1】在网格项目中设置justify-self、align-self或者margin:  auto

```
<style>
    .f18 .parent{
        display: grid;
    }
    .f18 .child{
        align-self: center;
        justify-self: center;
    }
</style>
<div class="case-box f18" data-case="f18">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```
效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f18" frameborder="0"></iframe>

【2】在网格容器上设置justify-items、align-items或justify-content、align-content
```
<style>
    .f19 .parent{
        display: grid;
        /*align-items: center;*/
        /*justify-items: center;*/
        align-content: center;
        justify-content: center;
    }
</style>
<div class="case-box f19" data-case="f19">
    <div class="parent" style="background-color: lightgray; width:200px; height:100px; ">
        <div class="child" style="background-color: lightblue;">测试文字</div>
    </div>
</div>
```
效果展示：
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f19" frameborder="0"></iframe>
