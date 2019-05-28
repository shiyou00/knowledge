## HTML 属性
### 属性
【class】
规定元素的一个或多个类
注意: 类不能以数字开头
```
class = "classA classB" // 多个类的写法
```
﻿
【id】
规定元素的唯一标识
注意：若浏览器中出现多个id名的情况，CSS样式对所以该id名的元素都生效，但javascript脚本仅对第一个出现该id名的元素生效
﻿
﻿
【dir】
文字的方向
值：ltr/rtl/auto
﻿
【lang】
HTML的lang属性可用于标记网页或部分网页的语言。也就是说lang这个属性不仅仅可以用在html标签上
```
en:英文
zh:中文
zh-CN：简体中文
﻿
<html lang="en">
﻿
<p>You'd say that in Chinese as <span lang="zh">文献情报中心</span>.</p>
```
﻿
有什么作用？
简单来说，可能对于程序来说没有太大的作用，但是它可以告诉浏览器，搜索引擎，一些处理Html的程序对页面语言内容来做一些对应的处理或者事情。
比如可以
根据根据lang属性来设定不同语言的css样式，或者字体
﻿
- 告诉搜索引擎做精确的识别
- 让语法检查程序做语言识别
- 帮助翻译工具做识别
- 帮助网页阅读程序做识别
等...
﻿
【style】
设置行间样式
```
<div style="background:red;"></div>
```
﻿
【tabindex】
规定元素的tab键次序（1开始）
```
<div>
    <a tabindex="3">cc</a>
    <a tabindex="2">bb</a>
    <a tabindex="1">aa</a>
</div>
```
﻿
【title】
规定关于元素的额外信息，鼠标移到元素上时显示一段提示文本
```
<a href="#" title="我是一个超链接">百度</a>
```
![](https://img2018.cnblogs.com/blog/958602/201902/958602-20190215103901040-1586589569.png)
﻿
﻿
### HTML5新增的属性
﻿
【contenteditable】
作用：指定是否可以在浏览器里编辑内容
值：true/false
注意：设置document.designMode ='on'时，页面的任意位置都可以编辑；使用contenteditable ='true'则只对具体元素和其包含的元素起作用
移动端：移动端ios5以及android3之后才支持该属性
﻿
【data-\*】
作用：用于存储页面或应用程序的私有定制数据
注意：属性名不应包含任何大写字母，且在前缀"data-"之后必须有至少一个字符；属性值可以是任意字符串
使用：可以在所有浏览器中使用getAttribute方法来获取data-*属性的值，也可以使用javascript中dataset属性访问data-*属性的值，不过IE10-浏览器不支持dataset
﻿
【draggable】IE8- 不支持
作用：用户是否可以拖动元素
值：true/false/auto
注意：链接和图像默认是可拖动的
﻿
【hidden】(IE7-不支持)
作用：显示或隐藏该元素(与display:none的作用一样)
值：hidden="" || hidden= "hidden"
﻿
【spellcheck】(IE9-不支持)
作用：规定是否对元素进行拼写和语法检查,对拼写错误的单词会在其下方出现红线
范围：可编辑区域（表单或contenteditable元素）
值：true/false
注意：移动端支持不好