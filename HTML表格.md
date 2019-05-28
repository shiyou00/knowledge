## 标准表格
```
<table border="1">
    <caption>Monthly savings</caption>
    <tr>
        <th>Header 1</th>
        <th colspan="2">Header 2</th>
    </tr>
    <tr>
        <td rowspan="2">row 1, cell 1</td>
        <td>row 1, cell 2</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```
![](https://img2018.cnblogs.com/blog/958602/201903/958602-20190310210611594-2137791589.png)
﻿
## 标签解释
```
<table>	定义表格
<th>	定义表格的表头
<tr>	定义表格的行
<td>	定义表格单元
<caption>	定义表格标题
<colgroup>	定义表格列的组
<col>	定义用于表格列的属性
<thead>	定义表格的页眉
<tbody>	定义表格的主体
<tfoot>	定义表格的页脚
```
﻿
## 属性解释
rowspan = "n" 纵向合并单元格
colspan = "n" 横向合并单元格