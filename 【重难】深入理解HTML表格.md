## 前言
在CSS出现之前，table元素常常用来布局。这种做法在HTML4之后不再推荐使用。而现在有些矫枉过正，使用table展示数据都可能会被说不规范。本文将详细介绍HTML表格table

## table 
【默认样式】
```
//IE7-浏览器不支持border-spacing
table{
　　border-collapse: separate;
　　border-spacing: 2px;
　　border: 1px solid gray;
}
```

【属性】
1、border(在html5中，border只能为"1"或" ")(html5已废弃)
```
border="0"//没有边框
border="8"//8像素宽的边框
```

2、cellpadding(px/%)(html5已废弃)
规定单元边界与单元内容之间的间距

3、cellspacing(px/%)(html5已废弃)
规定单元格之间的间距

4、summary(html5已废弃)
表格内容的摘要

5、width(html5已废弃)
表格宽度

```
<table border="2" cellpadding="5" cellspacing="3" summary="测试表格" width="300">
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table> 
```
6、frame(IE7-浏览器不能正常显示)(html5已废弃)
```
void	不显示外侧边框。
above	显示上部的外侧边框。
below	显示下部的外侧边框。
hsides	显示上部和下部的外侧边框。
vsides	显示左边和右边的外侧边框。
lhs	显示左边的外侧边框。
rhs	显示右边的外侧边框。
box	在所有四个边上显示外侧边框。
border	在所有四个边上显示外侧边框。
```

7、rules(IE7-浏览器不能正常显示)(html5已废弃)
```
none	没有线条。
groups	位于行组和列组之间的线条。
rows	位于行之间的线条。
cols	位于列之间的线条。
all	位于行和列之间的线条。
```

【样式】
1、border-spacing 可替代HTML属性cellspaing(IE7-不支持)
[注意]只有当border-collapse值为separate时，该样式才有效
```
border-spacing: x y
//x:水平间距 y:垂直间距。若只有一个值，则水平间距和垂直间距相等。注意，不可为负值。
```

2、empty-cells(IE7-不支持)
```
empty-cells: hide 不在空单元格周围绘制边框和背景，类似于hidden效果
empty-cells: show(默认) 在空单元格周围绘制边框和背景
```

3、CSS实际上有两种截然不同的边框模型。按布局术语来说，如果单元格相互之间是分隔的，是分隔边框模型在起作用；另一种是合并边框模型，单元格边框会相互合并。
```
border-collapse:separate;
```
[注意]在分隔边框模型中，不能为行、行组、列和列组设置边框。
```
border-collapse:collapse;
```
　　在合并边框模型中，表格无法设置内边距padding，且单元格边框之间也没有间距。单元格之间的边框会在单元格间的假想表格线上居中，且表格宽度只包含表格边框的一半

【边框合并的规则】
　　a、某个合并边框的border-style为hidden，它会优先于所有其他合并边框。这个位置上的所有边框都隐藏

　　b、某个合并边框的border-style为none，它的优先级最低

　　c、宽边框优先于窄边框

　　d、若宽度相同，double\solid\dashed\dotted\ridge\outset\groove\inset，优先级逐渐降低

　　e、若样式也相同，cell\row\row group\column\column group\table，优先级逐渐降级

4、table-layout
```
table-layout:auto//自动宽度布局
```
【自动布局的步骤】

　　a、对于一列中的单元格，计算最小和最大单元格宽度

　　b、对于各一列，计算最小和最大列宽

　　c、若单元格跨列，最小列宽之和要等于跨列单元格最小单元格宽度


```
table-layout:fixed//固定宽度布局
```

[注意]对于表单元格的长文本来说，使用word-wrap或word-break来强制换行，使用text-overflow实现文本溢出控制都需要设置`table-layout:fixed`

【固定布局的步骤】

　　a、width属性值不是auto的所有列元素会根据width值设置该列的宽度

　　b、如果一个列的宽度为auto，则根据该单元格设置此列宽度，如果跨多列，则宽度平均分配

　　c、如果列宽度仍为auto，则自动确定其大小，使其宽度尽可能相等

　　[注意]使用固定宽度布局，浏览器可以更快地计算出表格的布局

5、vertical-align
```
vertical-align: top;//顶端对齐
vertical-align: bottom;//底端对齐
vertical-align: middle;//中间对齐
vertical-align: baseline(默认);//基线对齐
```
[注意]`vertical-align:sub\super\text-top\text-bottom`应用到表格单元格时会被忽略

## 行
【`<tr><th><td>`】

```
  <tr>行 table row 
  <th>表头 table head
  <td>表格数据 table data
```
【默认样式】
```
th{
    padding: 1px;
    text-align: center;
    font-weight: bold;
}
td{
    padding: 1px;
}
```

【属性】
1、colspan
　　规定单元格可纵跨的列数

2、rowspan
　　规定单元格可横跨的行数

　　[注意]关于行的表格元素生成矩形框，这些框有内容、内边距和边框，但是没有外边距margin。表头呈现为居中的粗体文本

```
<table width="100%" border="1">
  <tr>
    <th>Month</th>
    <th>Savings</th>
    <th>Savings for holiday!</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
    <td rowspan="2">$50</td>
  </tr>
  <tr>
    <td>February</td>
    <td>$80</td>
  </tr>
</table>
```
![](https://i.loli.net/2019/05/24/5ce7ee28e077f14290.png)

## 列 
【`<col><colgroup>`】
`<col>` -> column 列
　　为表格中一个或多个列定义属性值

`<colgroup>` -> column group 列组
　　对表格中的列进行组合，以便对其进行格式化

【属性】
　　span
　　规定col元素应该横跨的列数


【样式】

　　1、`visibility:collapse`
　　该列或列组的所有单元格不显示(设置为其他值则无效)

　　2、border
　　只有当`border-collapse:collapse`时，才能设置border

　　3、background
　　只有当单元格及其行有透明背景时，列或列组的背景才可见

　　4、width
　　定义列或列组的最小宽度
  
```
<table border="1" style="border-collapse: collapse">
  <colgroup span="2" style="width:100px; background-color: red"></colgroup>
  <col style="background-color: green; width:200px; border: 3px solid blue;" >
  <tr>
    <td>数字</td>
    <td>中文</td>
    <td>英文</td>
  </tr>
  <tr>
    <td>1</td>
    <td>一</td>
    <td>a</td>
  </tr>
  <tr>
    <td>2</td>
    <td>二</td>
    <td>b</td>
  </tr>
</table>
```
![](https://i.loli.net/2019/05/24/5ce7ede9ceb5964603.png)

## 其他表格元素
【`<thead><tbody><tfoot>`】

```
<thead>表格页眉
<tbody>表格主体
<tfoot>表格页脚
```
　　[注意]它们的出现次序是：thead、tfoot、tbody，这样浏览器就可以在收到所有数据前呈现页脚 

【`<caption>`表格标题】  

【默认样式】
```
caption{
    text-align: center;
}
```

【样式】
```
caption-side: top(默认)
caption-side: bottom
```
[注意]`<caption>`标签必须紧随`<table>`标签之后，且只能对每个表格定义一个标题

```
<table border="1" >
　 <caption style="caption-side:bottom">北京天气</caption>
  <thead>
    <tr>
      <th>地区</th>
      <th>天气</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>北京</td>
      <td>都雾霾</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>城八区</td>
      <td>雾霾</td>
    </tr>
    <tr>
      <td>郊区</td>
      <td>雾霾</td>
    </tr>
  </tbody>
</table>
```
![](https://i.loli.net/2019/05/24/5ce7eda2d82b765024.png)

## display 
```
table{display: table;}
thead{display: table-header-group;}
tbody{display: table-row-group;}
tfoot{display: table-footer-group;}
tr{display: table-row;}
td,th{display: table-cell;}
col{display: table-column;}
colgroup{display: table-column-group;}
caption{display: table-caption;}
```

[注意]IE7-浏览器不支持为HTML元素设置与表格有关的display值


匿名表格对象
　　CSS定义了一种机制，将遗漏的组件作为匿名对象插入。详细插入规则如下：

　　1、如果table-cell元素的父元素不是table-row元素，则插入匿名table-row对象

　　2、如果table-row元素的父元素不是table、inline-table或table-row-group元素，则插入匿名table元素

　　3、如果table-column元素父元素不是table、inline-table或table-row-group元素，则插入匿名table元素

　　4、如果table-row-group、table-header-group、table-footer-group、table-column-group或table-caption的父元素不是table元素，则插入匿名table元素

　　5、如果table元素或inline-table元素的子元素不是table-row-group、table-header-group、table-footer-group、table-column-group或table-caption，则插入匿名table-row元素

　　6、如果table-row-group、table-header-group、table-footer-group元素的子元素不是table-row元素，则插入匿名table-row元素

　　7、如果table-row元素的子元素不是table-cell元素，则插入匿名tabel-cell元素
  
插入一段不合符规范的代码
```
<table>
    <td>122</td>
    <td>233</td>
</table>
```

看下浏览器的HTML效果：
![](https://i.loli.net/2019/05/24/5ce7ed6e94ed456818.png)

浏览器自动补上了tr

## 表格层
 　　CSS定义了6个不同的层，对应表各个方面的样式都在其各自的层上绘制。默认地，所有元素背景都是透明的，如果单元格、行、列等没有自己的背景，则table元素的背景将透明这些内部元素可见。

![](https://i.loli.net/2019/05/24/5ce7ed4de7a6a14743.png)

```
<table border="4" cellpadding="2" cellspacing="5" id="table" style="background-color: green;">
    <colgroup>
        <col id="col1" style="background-color: red;">
    </colgroup>
    <tbody>
        <tr id="row1" style="background-color: blue;">
            <td id="cell1" style="background-color: yellow;">row 1, cell 1</td>
            <td>row 1, cell 2</td>
        </tr>
        <tr>
            <td>row 2, cell 1</td>
            <td>row 2, cell 2</td>
        </tr>
    </tbody>
</table>
```

![](https://i.loli.net/2019/05/24/5ce7ed27e4c2b20616.png)

## 边距设置
【`<table>`】
　　若处于分隔边框模型，margin和padding都可设置
　　若处于合并边框模型，只可设置margin

【`<thead><tbody><tfoot><tr><col><colgroup>`】
　　margin和padding都不可设置

【`<td><th>`】
　　不可设置margin，但可以设置padding

【`<caption>`】
　　margin和padding都可设置
