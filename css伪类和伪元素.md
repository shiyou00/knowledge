## 前言
CSS的伪类和伪元素在平时的代码中经常会出现，可是一旦别人问你，什么是伪类，什么是伪元素，可能还是不能完整的表述出来，下面我们来一探究竟。
﻿
## 伪类和伪元素定义
> 伪类用于在页面中的元素处于某个状态时，为其添加指定的样式。
﻿
> 伪元素会创建一个抽象的伪元素，这个元素不是DOM中的真实元素，但是会存在于最终的渲染树中，我们可以为其添加样式。
﻿
[重点]最常规的区分伪类和伪元素的方法是：实现伪类的效果可以通过添加类来实现，但是想要实现伪元素的等价效果只能创建实际的DOM节点。
﻿
[注意]伪类使用单冒号 “:”  ; 伪元素使用双冒号 “::”
﻿
## 伪元素
> 伪元素可以分为排版伪元素、突出显示伪元素、树中伪元素三类。
﻿
### 排版伪元素
﻿
【::first-line】
> 设置元素中第一行文本的样式
﻿
[注意]::first-line伪元素只有应用在块级容器上才有效，且必须出现在相同流中的块级子孙元素中（即没有定位和浮动）。
[注意]只有当选择器部分和左大括号之间有空格时，IE6-浏览器才支持。因为first-line中存在连接符的原因
﻿
```
.firstLine::first-line {
    width: 200px;
    text-transform: uppercase;
    background: #f3f3f3;
 }
<p class="firstLine">hello world hello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello world </p>
```
虽然在DOM中看不到，但实际上，上面的这段HTML代码会通过添加虚拟标签的方式进行修改。
﻿
【::first-letter】
> 指定一个元素第一个字母的样式
﻿
[注意1]所有前导标点符号应与第一个字母一同应用该样式
﻿
[注意2]只能与块级元素关联
﻿
[注意3]只有当选择器部分和左大括号之间有空格时，IE6-浏览器才支持。因为first-letter中存在连接符的原因
﻿
```
<style>
.letter{
    width: 200px;
    border: 1px solid black;
    text-indent: 0.5em;
}
.letter:first-letter{
    font-size: 30px;
    float: left;
} 
</style>
﻿
<div class="letter">测试首字母下层，测试首字母下层测试首字母下层测试首字母下层测试首字母下层测试首字母下层测试首字母下层测试首字母下层测试首字母下层</div>
```
﻿
### 突出显示伪元素
> 突出显示伪元素表示文档中特定状态的部分，通常采用不同的样式展示该状态。如页面内容的选中。突出显示伪元素不需要在元素树中有体现，并且可以任意跨越元素边界而不考虑其嵌套结构。
﻿
【::selection】
> 匹配被用户选择的部分
﻿
[注意1]firefox浏览器需要添加-moz-前缀
[注意2]只支持双冒号写法
[注意3]只支持颜色和背景颜色两个样式
﻿
```
div::selection{color: red;}
```
<style>
p::selection{
    color:#fff;
}
</style>
﻿
﻿
### 树中伪元素
﻿
【::before & ::after】
> ::before是在源元素的实际内容前添加伪元素。::after是在源元素的实际内容后添加伪元素。当::before/::after伪元素的content属性不为'none'时，这两类伪元素就会生成一个元素，作为源元素的子元素，可以和DOM树中的元素一样定义样式。
﻿
[兼容]IE7-浏览器不支持
[注意]默认这个伪元素是行内元素，且继承元素可继承的属性；IE7-浏览器中必须声明!DOCTYPE，否则不起作用
﻿
```
// content = 字符串
.box:after{content:"后缀"}
﻿
// content = url
div:before{
    content: url("arrow.gif");
}
﻿
// content = attr
<div data-before="把我现在上去"></div>
div:before{
    content: attr(data-before);
}
```
[特] content的内容可以直接使用 url() 引入图片
﻿
【::marker】
> ::markder可以用于定义列表项标记的样式。
﻿
```
<style>
    .item::marker{
        color:green;
    }
</style>
﻿
<ul>
 <li class="item">Item 1</li>
 <li class="item">Item 2</li>
 <li class="item">Item 3</li>
 </ul>
```
该伪元素暂时只有safari支持，尝试的话请使用safari。可以用于该伪元素的属性也有限
﻿
【::placeholder】
> 表示输入框内占位提示文字。可以定义其样式。
﻿
```
::placeholder {
 color: blue;
}
```
﻿
## 伪元素速查表
```
    /* Typographic Pseudo-elements */
    ::first-line            /* 选取文字块首行字符 */
    ::first-letter          /* 选取文字块首行首个字符 */
﻿
    /* Highlight Pseudo-elements */
    ::selection             /* 选取文档中高亮(反白)的部分*/
    ::inactive-selection    /* 选取非活动状态时文档中高亮(反白)的部分*/
    ::spelling-error        /* 选取被 UA 标记为拼写错误的文本 */
    ::grammar-error         /* 选取被 UA 标记为语法错误的文本 */
﻿
    /* Tree-Abiding Pseudo-elements */
    ::before                /* 在选中元素中创建一个前置的子节点 */
    ::after                 /* 在选中元素中创建一个后置的子节点 */
    ::marker                /* 选取列表自动生成的项目标记符号 */
    ::placeholder           /* 选取字段的占位符文本(提示信息) */
    
    /* WebVTT Format */
    ::cue                   /* 匹配所选元素中 WebVTT 提示 */
﻿
    /* Fullscreen API */
    ::backdrop              /* 匹配全屏模式下的背景 */
```
﻿
## 伪类
> 伪类经常与伪元素混淆，伪元素的效果类似于通过添加一个实际的元素才能达到，而伪类的效果类似于通过添加一个实际的类来达到。实际上css3为了区分两者，已经明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示。
﻿
### 锚点
关于锚点`<a>`，有常见的5个伪类，分别是:link,:hover,:active,:focus,:visited
```
a:link{background-color:pink;}/*品红，未访问*/
a:hover{background-color:lightblue;}/*浅蓝，鼠标悬停*/
a:active{background-color:lightgreen;}/*浅绿，正被点击*/
a:focus{background-color:lightgrey;}/*浅灰，拥有焦点*/
a:visited{color:orange;}/*字体颜色为橙色，已被访问*/
/*[注意]visited伪类只能设置字体颜色、边框颜色、outline颜色的样式*/
```
﻿
### 伪类顺序
对于伪类顺序，有一个口诀是love-hate，代表着伪类的顺序是link、visited、focus、hover、active。但是否伪类的顺序只能如此呢？为什么是这个顺序呢？
﻿
CSS层叠中有一条法则十分重要，就是后面覆盖前面，所以伪类的顺序是需要精心考虑的。
﻿
【1】link和visited必须在最前面，且没有先后顺序，否则link或visited的效果将覆盖 hover active focus
﻿
[注意]link和visited称为静态伪类，只能应用于超链接
﻿
【2】hover、active、focus这三个伪类必须是focus、hover、active的顺序，因为在focus状态下，也需要触发hover和active，而要触发active一定要先触发hover，所以active要放在hover后面
﻿
[注意]hover、active、focus称为动态伪类，可应用于任何元素，但IE7-浏览器不支持:focus，:hover和:active在IE6-浏览器下只支持给\<a>设置
﻿
所以最终的顺序只有两种:link、visited、focus、hover、active或visited、link、focus、hover、active
﻿
```
a:link{background-color:pink;}/*品红，未访问*/
a:visited{color:orange;}/*字体颜色为橙色，已被访问*/
a:focus{background-color:lightgrey;}/*浅灰，拥有焦点*/
a:hover{background-color:lightblue;}/*浅蓝，鼠标悬停*/
a:active{background-color:lightgreen;}/*浅绿，正被点击*/
```
﻿
### UI元素伪类
> UI元素伪类包括:enabled、:disabled、:checked三个，主要针对于HTML中的form元素，IE8-浏览器不支持
﻿
```
:enabled    可用状态
:disabled   不可用状态
:checked    选中状态
﻿
input:enabled{
    border: 1px solid black;
    background-color: transparent;
}
input:disabled{
    border: none;
    background-color: gray;
}
input:checked{
    outline: 2px solid lightblue;
}
﻿
<button onclick = "btn.disabled = false;">按钮可用</button>
<button onclick = "btn.disabled = true;">按钮不可用</button>
<input type="button" id="btn" value="按钮">
<label>Male<input type="radio" name="sex" /></label>
<label>Female<input type="radio" name="sex"  /></label>
```
﻿
### 结构伪类
> 结构伪类可分为以下3种情况，IE8-浏览器不支持
﻿
【1】:nth-child(n)、:nth-last-child(n)、first-child、last-child、:only-child
```
E F:nth-child(n)           选择父元素的第n个子元素
E F:nth-last-child(n)      选择父元素的倒数第n个子元素
E F:first-child            父元素的第一个子元素，与E F:nth-child(1)等同
E F:last-child             父元素的最后一个子元素，与E F:nth-last-child(1)等同
E F:only-child             选择父元素中只包含一个子元素
﻿
p:first-child    　　 代表的并不是<p>的第一个子元素，而是<p>元素是某元素的第一个子元素
p > i:first-child    匹配所有<p>元素中的第一个<i>元素
p:first-child i 　　  匹配所有作为第一个子元素的<p>元素中的所有<i>元素
```
[注]n可以是整数(从1开始)，也可以是公式，也可以是关键字(even、odd)
﻿
【2】:nth-of-type(n)、:nth-last-of-type(n)、:first-of-type、:last-of-type、:only-of-type
```
E F:nth-of-type(n)          选择父元素的具有指定类型的第n个子元素
E F:nth-last-of-type(n)     选择父元素的具有指定类型的倒数第n个子元素
E F:first-of-type           选择父元素中具有指定类型的第1个子元素，与E F:nth-of-type(1)相同
E F:last-of-type         　  选择父元素中具有指定类型的最后1个子元素，与E F:nth-last-of-type(1)相同
E F:only-of-type        　　 选择父元素中只包含一个同类型的子元素
﻿
.box div:nth-of-type(even){color: red;} 
.box p:nth-last-of-type(3){color: green;}
.box div:first-of-type{color: blue;}
.box p:last-of-type{color: yellow;}
.box div:only-of-type{color: pink;}
```
﻿
【3】:root、:not、:empty、:target
```
:root        　选择文档的根元素
:not         　选择除某个元素之外的所有元素
:empty         选择没有子元素的元素，而且该元素也不包含任何文本节点
:target     　 匹配锚点对应的目标元素
﻿
[注意]:not选择器常用于导航之间的竖线处理，如li:not(:last-of-type)
﻿
:root{color:red;}
div:not{background-color: lightgrey;}
p:empty{height:30px;width:30px;background:pink;}
:target{color:blue;} // 当点击页面上面的锚点时，锚点到的目标元素会采用这个样式
```
﻿
【其它】
1、:lang()  匹配某个语言，IE7-浏览器不支持
```
p:lang(en) 匹配语言为"en"的<p>
```
﻿
2、不仅可以使用单一伪类，也可以伪类结合使用
[注意]顺序无关
```
div:hover:first-child{background-color: lightgreen;}
div:last-of-type:active{background-color: lightblue;}  
```
﻿
﻿
## 伪类速查表
```
    /* Logical Combinations */
    :matches() /*:any()*/   /* 匹配 集合内指定 的元素 */
    :not()                  /* 排除 满足指定关系 的元素 */
    :has()                  /* 匹配 满足指定关系 的元素*/
﻿
﻿
    /* Linguistic Pseudo-classes */
    :dir()                  /* 匹配 设置dir(文字书写方向)属性 的元素 */
    :lang()                 /* 匹配 设置lang(定义元素语言)属性 的元素 */
﻿
﻿
    /* Location Pseudo-classes */
    :any-link               /* 匹配 任意有链接锚点 的元素*/
    :link                   /* 匹配 未处于访问记录中 的链接 */
    :visited                /* 匹配 处于访问记录中 的链接 */
    :target                 /* 匹配 URL指向的锚点 的元素 */
    :scope                  /* 匹配 设置scoped属性的style标签 的作用域 */
﻿
﻿
    /* User Action Pseudo-classes */
    :hover                  /* 匹配 处于鼠标悬停状态 的元素 */
    :active                 /* 匹配 处于激活状态 的元素 */
    :focus                  /* 匹配 处于聚焦状态 的元素 */
    :focus-ring             /* 匹配 处于聚焦状态元素 的UA样式(聚焦轮廓) */
    :focus-within           /* 匹配 子节点处于聚焦状态 的元素 */
    :drop                   /* 匹配 处于拖拽状态 的元素 */
    :drop()                 /* 匹配 处于指定拖拽状态 的元素 */
﻿
﻿
    /* Time-dimensional Pseudo-classes */
    :current                /* 匹配 处于当前状态 的定义了timeline属性的元素 */
    :past                   /* 匹配 处于过去状态 的定义了timeline属性的元素 */
    :future                 /* 匹配 处于将来状态 的定义了timeline属性的元素 */
﻿
﻿
    /* Resource State Pseudos */
    :playing                /* 匹配 处于播放状态 的元素 */
    :paused                 /* 匹配 处于暂停状态 的元素 */
﻿
﻿
    /* The Input Pseudo-classes */
    :enabled                /* 匹配 可以编辑 的元素 */
    :disabled               /* 匹配 禁止编辑 的元素 */
    :read-only              /* 匹配 内容只读 的元素 */
    :read-write             /* 匹配 内容可编辑 的元素 */
    :placeholder-shown      /* 匹配 显示字段占位符文本 的元素 */
    :default                /* 匹配 页面载入默认选中 的元素 */
﻿
    :checked                /* 匹配 选中状态 的元素 */
    :indeterminate          /* 匹配 模糊状态 的元素 */
﻿
    :valid                  /* 匹配 输入内容通过类型验证 的元素 */
    :invalid                /* 匹配 输入内容无法通过类型验证 的元素 */
    :in-range               /* 匹配 输入数值符合范围 的元素 */
    :out-of-range           /* 匹配 输入数值溢出范围 的元素 */
    :required               /* 匹配 设置必填属性 的元素 */
    :optional               /* 匹配 可选字段 的元素 */
    :user-invalid           /* 匹配 用户输入内容未通过验证 的元素 */
﻿
    /* Tree-Structural pseudo-classes */
    :root                   /* 匹配 文档树 的根元素*/
    :empty                  /* 匹配 无子节点 的元素 */
    :blank                  /* 匹配 仅包含空格或者换行符 的元素 */
﻿
    :nth-child(n)           /* 匹配 符合元素集合中指定位置 的元素 */
    :nth-last-child(n)      /* 反序匹配 符合元素集合内指定位置 的元素 */
    :first-child            /* 匹配 符合元素集合内首个 的元素 */
    :last-child             /* 匹配 符合元素集合内末尾 的元素 */
    :only-child             /* 匹配 无兄弟节点 的元素 */
﻿
    :nth-of-type(n)         /* 匹配 符合元素集合中同类型指定位置 的元素 */
    :nth-last-of-type(n)    /* 反序匹配 符合元素集合中同类型指定位置 的元素 */
    :first-of-type          /* 匹配 每个在元素集合中初次出现 的元素 */
    :last-of-type           /* 匹配 每个在元素集合中末次出现 的元素 */
    :only-of-type           /* 匹配 无同类兄弟节点 的元素*/
﻿
﻿
    /* Fullscreen API */
    :fullscreen             /* 匹配 全屏显示模式中 的元素 */
﻿
﻿
    /* Page Selectors */
    :first                  /* 打印文档时首页的样式 */
    :left                   /* 打印文档时左侧的样式 */
    :right                  /* 打印文档时右侧的样式 */
```