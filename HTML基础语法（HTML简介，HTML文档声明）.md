## HTML 概念介绍

【概念】
> (Hyper Text Markup Language)超文本标记语言，是用来描述网页的一种语言
超文本(Hyper Text):不只包括文本，也可以包括图片、链接、音乐、视频等非文本元素
标记语言(Markup Language):标记语言是一套标记标签，HTML使用标记标签来描述网页

【标签】
单标签：`<img src="" alt="" />`
双标签：`<b></b>`

**HTML标签对大小写不敏感，但要全小写**

【属性】
标签的属性
常用属性：
- class 类
- id 元素ID
- style 元素的行内样式
- title 元素的额外信息，可在工具提示中显示

【元素】
HTML元素以开始标签起始，以结束标签终止，元素的内容是开始标签与结束标签之间的内容。

【文档】
HTML文档被称为网页，由嵌套的HTML元素构成

【注释】
注释是在HTML插入的描述性文本，用来解释该代码或提示其他信息。
```
<!-- 注释内容 -->
```
ps: 注释只出现在代码中，不会在页面中显示；且注释不可嵌套

## HTML文档声明
> HTML文档通常以类型声明开始，该声明将帮助浏览器确定其尝试解析和显示的HTML文档类型。

### 特点
> 文档声明必须是HTML文档的第一行、且顶格显示，对大小写不敏感。因为任何放在DOCTYPE前面的东西，比如批注或XML声明，会令IE9或更早期的浏览器触发怪异模式(后面的渲染模式会介绍)
由于文档类型声明不是标签，因此不应具有关闭标签

【HTML5】
```
<!DOCTYPE html>
```

在HTML5之前，文档声明一般有三种类型：严格型strict、过渡型transitional、框架frameset

严格型DTD包含所有HTML元素和属性，但不包含展示性的和弃用的元素(如font)；而过渡型或宽松型(loose)则包含展示性和弃用的元素

【HTML4.01】（1999）
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
"http://www.w3.org/TR/html4/strict.dtd">

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

【XHTML1.0】（2000）
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN"  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

【DTD】
DTD称为文档类型定义，它可以定义合法的XML文档构建模块，它使用一系列合法的元素来定义文档的结构。在HTML中，DTD使用XML定义了HTML标签规范

由于HTML5不基于SGML，所以不需要引用DTD。但是需要doctype来启用标准模式(后面的渲染模式会介绍)。HTML5的语法元素来自SGML、HTML、XHTML1.X，使它成为一种有自己规则的合成语言

【渲染模式】
在很久以前的网络上，页面通常有两种版本：为网景(Netscape)的Navigator准备的版本以及为微软(Microsoft)的Internet Explorer准备的版本。当W3C创立网络标准后，为了不破坏当时既有的网站，浏览器不能直接起用这些标准。因此，浏览器采用了两种模式，用以把能符合新规范的网站和老旧网站区分开。

浏览器排版引擎有三种模式：怪异模式(Quirks mode)、接近标准模式(Almost standards mode)以及标准模式(Standards mode)。在怪异模式下，排版会模拟Navigator4与Internet Explorer 5的非标准行为。为了支持在网络标准被广泛采用前，就已经建好的网站，这么做是必要的。在标准模式下，行为即由HTML与CSS的规范描述的行为。在接近标准模式下，只有少数的怪异行为被实现

对HTML文档来说，浏览器使用文档开头的DOCTYPE来决定用怪异模式处理或标准模式处理。如果文档中没有DOCTYPE将触发文档的怪异模式。怪异模式最明显的影响是会触发怪异盒模型。在CSS中盒模型被分为两种，第一种是W3C的标准模型，第二种是怪异盒模型。不同之处在于怪异盒模型的宽高定义的是可见元素框的尺寸，而不是元素框的内容区尺寸

## 小结

通过本节的内容，我们初步了解了什么是HTML，并且同时了解了HTML的文档头的声明，以及标准模型和怪异模型是通过文档头的定义来触发