## 前言
> 说起自适应布局方式，单列定宽单列自适应布局是最基本的布局形式。比如斗鱼的直播间，后台管理系统都是常用的

我们将从 float, inline-block, table, absolute, flex, grid 这几个布局方式来实现这种效果

## float

【float + margin】

> 将定宽的一列使用float，而自适应的一列使用计算后的margin

```
<style>
    .f1 .parent{overflow: hidden;zoom: 1;} // 触发bfc和haslayout来闭合浮动
    .f1 .left{position: relative;float: left;width: 100px;}
    // width: 100% 以免浮动后内容撑开宽度。 margin-left: -100px; 由于宽度设置100%这会挤到下一行去了，利用margin 负值特性来规避
    .f1 .rightWrap{float: left;width: 100%;margin-left: -100px;} 
    .f1 .right{margin-left: 120px;}
</style>
<div class="case-box f1" data-case="f1">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="rightWrap" style="background-color: pink;">
            <div class="right"  style="background-color: lightgreen;">
                <p>right</p>
                <p>right</p>
            </div>
        </div>
    </div>
</div>
```

效果展示：
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f1" frameborder="0"></iframe>

【float + margin + calc】  
[注意]IE8-、android4.3-、IOS5.1-不支持，android4.4+只支持加减运算

```
<style>
    .f2 .parent{overflow: hidden;zoom: 1;}
    .f2 .left{float: left;width: 100px;margin-right: 20px;}
    .f2 .right{float: left;width: calc(100% - 120px);} // 利用calc可以不同长度单位进行动态计算的特性
</style>
<div class="case-box f2" data-case="f2">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```

效果展示:
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f2" frameborder="0"></iframe>

【float + overflow】  
还可以使用overflow属性来触发bfc，来阻止浮动造成的文字环绕效果。由于使用`overflow`不会改变元素的宽度属性，所以不需要重新设置宽度。由于设置`overflow:hidden`并不会触发IE6-浏览器的`haslayout`属性，所以需要设置zoom:1来兼容IE6-浏览器

```
<style>
    .f3 .parent{overflow: hidden;zoom: 1;}
    .f3 .left{float: left;width: 100px;margin-right: 20px;}
    .f3 .right{overflow:hidden;zom:1;} // 触发bfc和haslayout
</style>
<div class="case-box f3" data-case="f3">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f3" frameborder="0"></iframe>

## inline-block
inline-block内联块布局的主要缺点是需要设置垂直对齐方式vertical-align，则需要处理换行符解析成空格的间隙问题。IE7-浏览器不支持给块级元素设置inline-block属性，兼容代码是display:inline;zoom:1;

【inline-block + margin + calc】  
一般来说，要解决inline-block元素之间的间隙问题，要在父级设置font-size为0，然后在子元素中将font-size设置为默认大小

[注意]IE8-、android4.3-、IOS5.1-不支持，android4.4+只支持加减运算 

```
<style>
    .f4 .parent{font-size:0;}
    .f4 .left{
        display: inline-block;
        vertical-align: top;
        width: 100px;
        margin-right: 20px;
        font-size: 16px;
    }
    .f4 .right{
        display: inline-block;
        vertical-align: top;
        width: calc(100% - 120px);
        font-size: 16px;
    }
</style>
<div class="case-box f4" data-case="f4">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```

效果展示:
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f4" frameborder="0"></iframe>


【inline-block + margin + margin负值】
```
<style>
    .f5 .parent{
        font-size: 0;
    }
    .f5 .left{
        position: relative;
        display: inline-block;
        vertical-align: top;
        width: 100px;
        font-size:16px;
    }
    .f5 .rightWrap{
        display: inline-block;
        vertical-align: top;
        width: 100%;
        margin-left: -100px;
        font-size: 16px;
    }
    .f5 .right{margin-left: 120px;}
</style>
<div class="case-box f5" data-case="f5">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="rightWrap" style="background-color: pink;">
            <div class="right"  style="background-color: lightgreen;">
                <p>right</p>
                <p>right</p>
            </div>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f5" frameborder="0"></iframe>

## table
使用table布局的缺点是元素被设置为table后，内容撑开宽度，所以需要设置width:100%。若要兼容IE7-浏览器，需要改为\<table>结构。由于table-cell元素无法设置margin，若需要在元素间设置间距，需要增加结构

```
<style>
    .f6 .parent{display:table;width: 100%;table-layout: fixed;}
    .f6 .left,.f6 .rightWrap{display:table-cell;}
    .f6 .left{width: 100px;}
    .f6 .right{margin-left: 20px;}
</style>
<div class="case-box f6" data-case="f6">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="rightWrap" style="background-color: pink;">
            <div class="right"  style="background-color: lightgreen;">
                <p>right</p>
                <p>right</p>
            </div>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 80px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f6" frameborder="0"></iframe>


## absolute
absolute布局的缺点是由于父元素需要设置为relative，且子元素设置为absolute，所以父元素的高度并不是由子元素撑开的，需要单独设置。

```
<style>
    .f7 .parent{
        position: relative;
        width: 100%;
        height:40px;
    }
    .f7 .left{
        position: absolute;
        left:0;
        width:100px;
    }
    .f7 .right{
        position: absolute;
        left:120px;
        right:0;
    }
</style>
<div class="case-box f7" data-case="f7">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f7" frameborder="0"></iframe>

## flex
flex弹性盒模型是非常强大的布局方式。但由于其性能消耗较大，适合于局部小范围的布局

[注意]IE9-浏览器不支持

[flex布局教程](CSS布局flex布局.md)
```
<style>
    .f8 .parent{
        display: flex;
    }
    .f8 .left{
        width:100px;
        margin-right: 20px;
    }
    .f8 .right{
        flex:1 1 auto;
    }
</style>
<div class="case-box f8" data-case="f8">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f8" frameborder="0"></iframe>

## grid
 使用栅格布局grid实现  
[注意]IE10-浏览器不支持

[grid布局详细教程](CSSGrid基于网格的二维布局系统详细教程.md)
```
<style>
    .f9 .parent{
        display: grid;
        /* 子项目分别按照 100px固定宽度 以及1fr的栅格宽度分配 */
        grid-template-columns: 100px 1fr;
        /* 子项目间隙 */
        grid-gap:20px
    }
</style>
<div class="case-box f9" data-case="f9">
    <div class="parent" style="background-color: lightgrey;">
        <div class="left" style="background-color: lightblue;">
            <p>left</p>
        </div>
        <div class="right"  style="background-color: lightgreen;">
            <p>right</p>
            <p>right</p>
        </div>
    </div>
</div>
```
效果展示:
<iframe style="width: 100%; height: 240px;" src="https://shiyou00.github.io/lion/dist/html/css-layout/layout.html?case=f9" frameborder="0"></iframe>
