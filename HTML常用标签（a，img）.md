## a元素
> `<a>`元素 (或HTML锚元素, Anchor Element)通常用来表示一个锚点/链接。但严格来说，`<a>`元素不是一个链接，而是超文本锚点，可以链接到一个新文件、用id属性指向任何元素。如果没有`<a>`元素没有href属性的话，可以作为原本链接位置的占位符，常用于home链接
﻿
[注意]任何文档流内容都可以被嵌套，只要不是交互内容类别(如按钮、链接等)
﻿
【href】
href属性表示地址，共包括以下3种：
```
链接地址:<a href="http://www.baidu.com">百度</a>
下载地址:<a href="test.zip">下载测试</a>
锚点:
href:#id名 
<a href="#test">目录</a>
<div id="test" style="height: 200px;width: 200px; border: 1px solid black;margin-bottom: 300px;">内容</div>
﻿
href:页面地址#id名
<a href="http://baike.baidu.com/view/2202.htm#2">足球比赛规则</a>
﻿
手机号码：
在移动端，使用<a href="tel:15012345678>15012345678</a>可以唤出手机拨号盘
```
href属性一定不要留空，若暂时不需要写地址，则写#或javascript:;。若href留空，会刷新页面
﻿
【href与src的区别】
href(hypertext reference)指超文本引用，表示当前页面引用了别处的内容
﻿
src(source)表示来源地址，表示把别处的内容引入到当前页面，相当于资源占位
﻿
所以`<img>`、`<script>`、`<iframe>`等应该使用src，而`<a>`和`<map>`应该使用href
﻿
﻿
【target】
target属性表示链接打开方式
1、_self    当前窗口（默认）
2、_blank    新窗口
3、_parent 父框架集
4、_top 整个窗口
5、_framename 指定框架
﻿
﻿
【download】
download属性用来设置下载文件的名称(firefox/chrome/opera支持)
﻿
```
<a href="test.zip" download="gogo">test</a>
```
﻿
【注意事项】
1、`<a>`标签的文本颜色只能自身进行设置，从父级继承不到
﻿
2、`<a>`标签的下划线颜色跟随文本颜色进行变化
﻿
3、`<a>`标签不可嵌套`<a>`标签
﻿
--------------------------------------------------------------------
﻿
## img
> `<img>`表示image图像，从技术上讲，`<img>`标签并不会在网页中插入图像，而是从网页上链接图像。`<img>` 标签创建的是被引用图像的占位空间。
﻿
【必须属性】
1、src:地址
2、alt:图像替代文本，供探索引擎抓取使用
﻿
【可选属性】
1、height:图像高度
2、width:图像宽度
3、ismap:为图像定义为服务器端图像映射
4、longdesc:与alt属性类似，提供多于1024字符的长文本描述
5、usemap:为图像定义客户端图像映射 usemap = "#\<map>元素的name或id属性"
6、srcset:指定图片的地址和对应的图片质量。属性格式：图片地址 宽度描述w 多个资源之间用逗号分隔。对于srcset里面出现了一个w单位，可以理解成图片质量。如果可视区域小于这个质量的值，就可以使用，当然，浏览器会自动选择一个最小的可用图片。但是，会发现随着浏览器窗口宽度变大，图片也在不断变大
﻿
[注意]浏览器会自动匹配最佳显示的图片，如果大图既然缓存了就用大图了，再缩小也不会变成小图了
﻿
```
<img src="small.jpg" srcset="small.jpg 300w,middle.gif 500w,big.gif 800w">
```
﻿
7、sizes:用来设置图片的尺寸零界点，主要跟响应式布局打交道。属性格式：媒体查询 宽度描述(支持px)，多条规则用逗号分隔
[注意]如果加上sizes属性，会发现，随着浏览器宽度变大，图片一直保持其初始尺寸。所以，应该sizes和srcset两个属性配合使用
```
<img src="small.jpg" srcset="small.jpg 300w,middle.gif 500w,big.gif 800w" sizes="(max-width:300px) 300px, (max-width:500px) 500px,800px">
```