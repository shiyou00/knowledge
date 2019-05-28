## HTML整体结构解释
﻿
```html
<!DOCTYPE html> // 文件应以“<!DOCTYPE ......>”首行顶格开始，推荐使用“<!DOCTYPE html>”
<html>
<head>
    // 必须声明文档的编码charset，且与文件本身编码保持一致，指定字符编码的 meta 必须是 head 的第一个直接子元素。推荐使用UTF-8编码
    <meta charset="utf-8"/> 
    // 页面title是不可缺少的一项，title 必须作为 head 的直接子元素，并紧随 charset 声明之后
    <title>Document</title> 
﻿
    <meta name="keywords" content=""/>
    <meta name="description" content=""/>
    // 设置视口
    <meta name="viewport" content="width=device-width"/>
    // 引入 CSS 时必须指明 rel="stylesheet"
    <link rel="stylesheet" href="5/style.css"/> 
    // link标签必须在head标签中引入
    <link rel="shortcut icon" href="ico.ico"/> 
</head>
<body>
﻿
<script src="//code.jquery.com/jquery-migrate-1.2.1.min.js"></script>   // 1. js引入要放在body的最下方(防止页面阻塞) 2. 移动环境或只针对现代浏览器设计的 Web 应用，如果引用外部资源的 URL 协议部分与页面相同，建议省略协议前缀。这是因为使用 protocol-relative URL 引入 CSS，在 IE7/8 下，会发两次请求。是否使用 protocol-relative URL 应充分考虑页面针对的环境
</body>
</html>
```
﻿
## HTML元素
﻿
### 块级元素
﻿
【h1, h2, h3, h4, h5, h6】
含义：标题元素分为六个等级的标签 
注意：h1 在一个HTML中最好只出现一次(seo方面)
﻿
【p】
含义：段落元素
﻿
【div】
含义：块级空元素
﻿
【hr】
含义：分割元素
﻿
【pre】
含义：预定义格式文本
在该元素中的文本通常按照原文件中的编排，以等宽字体的形式展现出来，文本中的空白符(比如空格和换行符)都会显示出来，通常表示已排版的内容，如代码块和字符画等
﻿
【blockquote】
含义：HTML块级引用元素
代表其中的文字是引用内容。通常在渲染时，这部分的内容会有一定的缩进。若引文来源于网络，则可以将原内容的出处URL地址设置到cite特性上，若要以文本的形式告知读者引文的出处时，可以通过`<cite>`元素
﻿
【address】
含义：联系信息
﻿
骨架类：html body
表单类：form fieldset output legend optgroup option
列表类：ul ol li dl dd dt
HTML5新增结构标签：article aside header footer nav section
HTML5新增多媒体：figure figcaption
HTML5新增功能型：summary details
﻿
﻿
### 内联元素
﻿
通用容器：span
强调重要：em strong
文字间隔：i(斜体) b(粗体)
不精确文字：s(在HTML5中重新定义为有错的、过时的、不被建议使用的文本，常用于表示价格变动等)
高亮显示：mark
次要评论：small
术语处理：dfn(定义术语) abbr(缩写词)
引用：cite(表示作品标题的引用，可以是书影音画等)  q(表示短引用，常用于引用别人说的话，用引号可以表达等价语义)
换行：br wbr(指定单词可以换行的位置)
上下标：sup sub
文本删改：ins(文档中插入的内容) del(文档中删除的内容)
特定时间：time(表示日期)
注音标识：ruby(ruby标签定义注音标识，多用于CJK文字，CJK是指中日韩统一表意文字(Chinese、Japanese、Korean)) rt(标记文字) tp(标记括号)
```html
复制代码
<ruby>
    汉
    <rp>(</rp>
    <rt>hàn</rt>
    <rp>)</rp>
    语
    <rp>(</rp>
    <rt>yǔ</rt>
    <rp>)</rp>    
</ruby>
```
文字方向：bdi(忽略周围文字方向的文字) bdo(覆盖两种方向的设置，允许显式设置方向，并覆盖所有其他当前方向)
```html
<p>When rendered by a browser, <bdo dir="rtl">these words</bdo> will appear as 'sdroweseht'</p>
```
代码：code(计算机代码) kbd(键盘码) samp(计算机例子代码) tt(打字机代码) var(变量)
﻿
### 结构元素
section：表示文档中的一个区域(或节)，是区块级通用元素
article：表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构。
aside：表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响。
nav：HTML导航栏(`<nav>`)描绘一个含有多个超链接的区域，这个区域包含转到其他页面，或者页面内部其他部分的链接列表
header：表示页面头部或区块头部
footer：表示最近一个章节内容或者根节点(sectioning root)元素的页脚
main(IE不支持)：main元素(`<main>`)呈现了文档`<body>`或应用的主体部分。
﻿
﻿
### 交互元素(浏览器的支持度太低)
details ： 主要用于描述文档或文档某个部分的细节
dialog ：用来定义对话框或窗口，且该对话框位于窗口的水平居中位置
﻿
--------------------------------------
﻿
## HTML 元素嵌套规则
1、块元素可以包含内联元素或某些块元素，但内联元素却不能包含块元素，它只能包含其它的内联元素
例：
```html
<div><h1></h1><p></p></div> —— 对
<a href=”#”><span></span></a> —— 对
<span><div></div></span> —— 错
```
﻿
2、块级元素不能放在`<p>`里面包括p本身
```html
<p><ol><li></li></ol></p> —— 错
<p><div></div></p> —— 错
```
﻿
3、有几个特殊的块级元素只能包含内联元素，不能再包含块级元素，这几个特殊的标签是
```html
h1、h2、h3、h4、h5、h6、p、dt
```
﻿
4、a 标签可以嵌套块级元素