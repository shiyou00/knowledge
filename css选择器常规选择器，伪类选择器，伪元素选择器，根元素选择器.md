## 前言
> CSS的一个核心特性是能向文档中的一组元素类型应用某些规则，本文将详细介绍CSS选择器
﻿
## 选择器
﻿
【通配选择器】  
星号*代表通配选择器，可以与任何元素匹配
```
*{color: red;}
```
﻿
【元素选择器】  
文档的元素是最基本的选择器
```
html{color: black;}
p{color: gray;}
h2{color: silver;}  
```
﻿
【类选择器】  
类选择器用于选择一类元素
```
.div{color: red;} // 单个类的选择
.div1.div2{color: red;} // 多个类的选择，既包含div1类也包含div2类的元素将被选择
p.div{color: red;} // p元素且包含div类的元素将被选择
```
﻿
【ID选择器】  
ID选择器用于选择一个元素
```
#test{color: red;}
div#test{color: red;} 结合元素选择ID
```
[注意]在实际中，浏览器并不会检查ID的唯一性，设置多个ID，可以为这些具有相同ID的元素应用相同样式，但在编写DOM脚本时只能识别该id的第一个元素。

【属性选择器】  
属性选择器根据元素的属性及属性值来选择元素(IE6-不支持)
```
// 简单属性选择器
h1[class]{color: red;}
img[alt]{color: red;}
a[href][title]{color: red;}
#div[class]{color: red;}
.box[id]{color: red;}
[class]{color: red;}
﻿
// 具体属性选择器
a[href="http://www.baidu.com"][title="baidu"] {color: red;}
[class="test box"]{color: red;} // class里面的值以及顺序必须完全相同，并且不可多空格或者少空格
[id="tox"]{color: red;}   // ID选择器和指定id属性的属性选择器并不是一回事，主要在于优先级不同
﻿
// 部分属性选择器
[class ~="b"] 选择class属性值在用空格分隔的词列表中包含词语"b"的所有元素，例如：class="ab"不满足[class ~="b"]，而class="a b"或class="b"满足
[class |="b"] 选择class属性值等于b或以b-开头的所有元素，例如：class="ab"或class="ab-"不满足[class |="a"]，而class="a"或class="a-"满足
[class ^="b"] 选择class属性值以"b"开头的所有元素
[class $="b"] 选择class属性值以"b"结尾的所有元素
[class *="b"] 选择class属性值包含"b"的所有元素 
```

【分组选择器】  
将要分组的选择器放在规则左边，并用逗号隔开
```
h1,p{color: red;} // 选择了h1 和 p 元素
```
﻿
【后代选择器】
```
ul li{color: red;} // ul 下面的所有 li元素 将被选择
div p, ul li{color: red;} // div元素 下面的所有p元素将被选择 并且 ul下面的所有 li元素将被选择
```
﻿
子元素选择器(IE6-不支持)
```
ul > li{color: red;} // ul下面的子元素 li将被选择 ， 不选中孙子元素
```
﻿
【兄弟元素选择器】
```
// 相邻兄弟选择器(IE6-不支持)
h1 + p{color: red;}  // 选择紧接在 h1 元素后出现的段落，h1 和 p 元素拥有共同的父元素
[注意]两个元素之间的文本内容不会影响相邻兄弟结合符起作用
```

## 伪类选择器
伪类顺序：link-visited-focus-hover-active

【静态伪类(只应用于超链接)】  
[注意]visited伪类只能设置字体颜色、边框颜色、outline颜色的样式
﻿
```
:link       未访问
:visited    已访问
a:link{color: red;}
a:visited{color: green;}
```
【动态伪类(可应用于任何元素)】
```
:focus     拥有焦点(IE7-不支持)
:hover     鼠标停留(IE6-不支持给<a>以外的其他元素设置伪类)
:active    正被点击(IE7-不支持给<a>以外的其他元素设置伪类)
```

【目标伪类:target(IE8-不支持)】  
匹配锚点对应的目标元素
```
:target{color: red;}
#test :target{color: red;}//id为test的目标元素
```
﻿
【UI元素伪类(IE8-不支持)】
```
:enabled    可用状态
:disabled    不可用状态
:checked    选中状态
﻿
input:enabled{color: red} // [注意]input和:和enabled之间都不可以有空格
```
﻿
【结构伪类(IE8-不支持)】
```
E:first-child(IE6-不支持) 父元素的第一个子元素,且该子元素是E，与E:nth-child(1)等同
E:last-child(IE6-不支持) 父元素的最后一个子元素，且该子元素是E，与E:nth-last-child(1)等同
:root	选择文档的根元素，即<html>元素
E F:nth-child(n) 选择父元素的第n个子元素，父元素是E，子元素是F
E F:nth-last-child(n) 选择父元素的倒数第n个子元素，父元素是E，子元素是F
E F:nth-of-type(n) 选择父元素的具有指定类型的第n个子元素，父元素是E，子元素是F
E F:nth-last-of-type(n) 选择父元素的具有指定类型的倒数第n个子元素，父元素是E，子元素是F
E:first-of-type	选择父元素中具有指定类型的第1个子元素，与E:nth-of-type(1)相同
E:last-of-type	选择父元素中具有指定类型的最后1个子元素，与E:nth-last-of-type(1)相同
E:only-child	选择父元素中只包含一个子元素，子元素是E
E:only-of-type	选择父元素中只包含一个同类型的子元素，子元素是E
E:empty	选择没有子元素的元素，而且该元素也不包含任何文本节点
```
[注意]n可以是整数(从1开始)，也可以是公式，也可以是关键字(even、odd)
﻿
```
p:first-child    代表的并不是<p>的第一个子元素，而是<p>元素是某元素的第一个子元素
p > i:first-child    匹配所有<p>元素中的第一个<i>元素
p:first-child i 匹配所有作为第一个子元素的<p>元素中的所有<i>元素
```
﻿
【:lang 相当于|=属性选择器(IE7-不支持)】
```
p:lang(en) 匹配语言为"en"的<p>
```
﻿
【伪类的结合】
```
a:visited:hover:first-child{color: black;} // [注意]顺序无关
```
﻿
## 伪元素选择器

IE8-浏览器仅支持伪元素选择器的单冒号表示法

【:first-letter 设置首字母样式】  
所有前导标点符号应与第一个字母一同应用该样式；  
只能与块级元素关联；  
只有当选择器部分和左大括号之间有空格时，IE6-浏览器才支持。因为first-letter中存在连接符的原因

```
p:first-letter {color: red;} 
```

【:first-line 设置首行样式】  
只能与块级元素关联；  
只有当选择器部分和左大括号之间有空格时，IE6-浏览器才支持。因为first-line中存在连接符的原因
```
p:first-line{color: red;} 
```
﻿
【:before 在元素内容前面插入内容(IE7-不支持)】  
默认这个伪元素是行内元素，继承元素可继承的属性；  
所有元素都必须放在出现该伪元素的选择器的最后面。
```
p:before{content:"text"}
```
﻿
【:after 在元素内容后面插入内容(IE7-不支持)】  
默认这个伪元素是行内元素，继承元素可继承的属性
```
p:after{content:"text"}
```
﻿
【::selection 匹配被用户选择的部分】  
目前selection只支持color和background两个属性，且只支持双冒号写法(IE8-浏览器不支持)
﻿
```
::-moz-selection 　　 firefox浏览器需要添加前缀
```
﻿
【根元素选择器】  
根元素选择器:root用来选择HTML元素，但由于其实质是伪类选择器，所以其优先级更高。在HTML上设置的样式，如果在:root上也设置了同样的样式，则会被覆盖
﻿
```
html{font-size:20px;}
:root{font-size:30px;}
```
最终是30px
