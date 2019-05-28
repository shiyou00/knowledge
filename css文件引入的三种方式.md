## 前言
1995年，W3C发布了CSS草案
1996年，W3C正式推出CSS1
1998年，推出CSS2
2001年从CSS3开始，CSS这门语言分割成多个独立的模块，每个模块独立分级，且只包含一小部分功能；
2011年开始设计CSS4
本文将主要介绍引入CSS样式的方式，包括外部样式表、内部样式表和行间样式三种方式
﻿
[注意]CSS语法非常简单，但容易忽略的一点是不能省略分号(最后一个样式除外)
﻿
## 外部样式表
```
<link rel="stylesheet" type="text/css" href="sheet1.css" media="all" />
```
﻿
## 内部样式表
```
<style>
body{
    background-color: red;
}    
</style>
```
﻿
[注意]文档中可出现多个style标签，且样式规则与层叠样式规则一致
﻿
【使用@import指令】
@import指令用于指示Web浏览器加载一个外部样式表，并在表现HTML文档时使用其样式。唯一的区别在于命令的具体语法和位置。
@import指令常用于样式表需要使用另一个样式表中的样式的情况。
```
<style>
@import url(sheet2.css);
body{
    background-color: red;
}    
</style>
```
[注意]@import必须出现在style元素中，且要放在其他CSS规则之前，否则将根本不起作用。
﻿
## 行间样式
如果只是想为单个元素指定一些样式，可以使用HTML的style属性来设置一个行间样式。
```
<body style="background-color: red; height: 100px; border: 10px solid black;" style="background-color: red;">
```
[注意]行间样式若存在多个style属性，只能识别第一个
﻿
﻿
[注意]`<style>`标签和`<link>`标签可以写在`<body>`标签里面