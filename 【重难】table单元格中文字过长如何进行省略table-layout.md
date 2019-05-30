## 前言
最近在项目中碰到了，需要在表格样式中对td立面进行过长省略处理，但是表格又是100%自适应的，td又是百分比的宽度，一时显得无从下手了，这篇文章我们主要讲解下如何处理这类问题
﻿
## 遇到的场景

```
<table border="1" style="width:600px;">
    <tr>
        <td>这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字</td>
        <td>row cell row cell row cell row cell</td>
    </tr>
</table>
```
![](https://www.showdoc.cc/server/api/common/visitfile/sign/6d500dd1f171b0c28e19f6808e029649?showdoc=.jpg)

是不是非常难看，在我们一般的布局中都会给td一个半分比，从而不让它这么肆无忌惮。

```
<table border="1" style="width:600px;">
    <tr>
        <td style="width:40%;">这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字这是一段特别长的文字</td>
        <td style=width:60%;>row cell row cell row cell row cell</td>
    </tr>
</table>
```

再来看下效果：
![](https://www.showdoc.cc/server/api/common/visitfile/sign/66c6028b5ff8e7ad0af2e6e7c7a6cc36?showdoc=.jpg)

果然是如我们的预期的百分比进行分割的。那么此时我们希望第一个单元格进行文字省略处理。我们加上代码试试。
```
<style>
    td{
        overflow: hidden; // 按照宽度隐藏
        text-overflow:ellipsis; // 省略
        white-space: nowrap;// 不换行
    }
</style>
```
![](https://www.showdoc.cc/server/api/common/visitfile/sign/d65ee576ef6c42345a5cd735207e8913?showdoc=.jpg)
从效果上面看：文字没有换行有效果，然而宽度被撑开的面目全非，这绝对不是我们想要的结果。

接下来就要介绍我们的一个样式了table-layout
﻿
## table-layout
```
/* Keyword values */
table-layout: auto;
table-layout: fixed;
﻿
/* Global values */
table-layout: inherit;
table-layout: initial;
table-layout: unset;
```
【auto】  
大多数浏览器采用自动表格布局算法对表格布局。表格及单元格的宽度取决于其包含的内容。

【fixed】  
表格和列的宽度通过表格的宽度来设置，某一列的宽度仅由该列首行的单元格决定。在当前列中，该单元格所在行之后的行并不会影响整个列宽。

使用 “fixed” 布局方式时，整个表格可以在其首行被下载后就被解析和渲染。这样对于 “automatic” 自动布局方式来说可以加速渲染，但是其后的单元格内容并不会自适应当前列宽。任何一个包含溢出内容的单元格可以使用 overflow  属性控制是否允许内容溢出。  
﻿
我们把相应的样式添加上再看看效果：  
```
<style>
    table{
        table-layout: fixed;
    }
    td{
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
    }
</style>
```
﻿
![](https://i.loli.net/2019/05/24/5ce7eeebcaf7878137.png)

分析：我们发现左边的单元格和右边的单元格进行对比依然是 40% 60% 的比例宽度，但是它的省略功能也实现了。

[注意]`td`上不添加宽度的话，则平均分配宽度 总宽度/列的数量

## 小结
本文主要讲解了table-layout的两种方式：auto以及fixed
﻿