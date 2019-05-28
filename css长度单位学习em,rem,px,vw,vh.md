## 绝对长度单位
> 绝对长度单位代表一个物理测量
﻿
【像素px(pixels)】
> 像素，为影像显示的基本单位，译自英文“pixel”，pix是英语单词picture的常用简写，加上英语单词“元素”element，就得到pixel，故“像素”表示“画像元素”之意，有时亦被称为pel（picture element）。每个这样的消息元素不是一个点或者一个方块，而是一个抽象的取样。仔细处理的话，一幅影像中的像素可以在任何尺度上看起来都不像分离的点或者方块；但是在很多情况下，它们采用点或者方块显示。每个像素可有各自的颜色值，可采三原色显示，因而又分成红、绿、蓝三种子像素（RGB色域），或者青、品红、黄和黑（CMYK色域，印刷行业以及打印机中常见）。照片是一个个取样点的集合，在影像没有经过不正确的/有损的压缩或相机镜头合适的前提下，单位面积内的像素越多代表分辨率越高，所显示的影像就会接近于真实物体。
﻿
在web上，像素px是典型的度量单位，很多其他长度单位直接映射成像素。最终，他们被按照像素处理
﻿
【英寸in(inches)】
1in = 2.54cm = 96px
﻿
【厘米cm(centimeters)】
1cm = 10mm = 96px/2.54 = 37.8px
﻿
【毫米mm(millimeters)】
1mm = 0.1cm = 3.78px
﻿
【1/4毫米q(quarter-millimeters)】
1q = 1/4mm = 0.945px
﻿
【点pt(points)】
点（英语：point，pt），也音译磅因、磅，是印刷所使用的长度单位，用于表示字型的大小，也用于余白（字距、行距）等其他版面构成要素的长度。
1pt = 1/72in = =0.0139in = 1/72*2.54cm = 1/72*96px = 1.33px
﻿
【派卡pc(picas)】
派卡（英语：pica）是印刷行业使用的长度单位。
1pc = 12pt = 1/6in = 1/6*96px = 16px
﻿
## 字体相关相对长度单位
> em、ex、ch、rem是字体相关的相对长度单位
﻿
【em】[重点]
em表示元素的font-size属性的计算值，如果用于font-size属性本身，相对于父元素的font-size；若用于其他属性，相对于本身元素的font-size
﻿
```
<style>
.box{font-size: 20px;}
.in{
    /* 相对于父元素，所以2*20px=40px */
    font-size: 2em;
    /* 相对于本身元素，所以5*40px=200px */
    height: 5em;
    /* 10*40px=400px */
    width: 10em;
    background-color: lightblue;
}
</style>
```
﻿
【rem】
rem是相对于根元素html的font-size属性的计算值
兼容性: IE8-不支持
```
<style>
/* 浏览器默认字体大小为16px，则2*16=32px，所以根元素字体大小为32px */
html{font-size: 2rem;}
/* 2*32=64px */
.box{font-size: 2rem;}
.in{
    /* 1*32=32px */
    font-size: 1rem;
    /* 1*32=32px */
    border-left: 1rem solid black;
    /* 4*32=128px */
    height: 4rem;
    /* 6*32=192px */
    width: 6rem;
    background-color: lightblue;
}
</style>
```
默认地，浏览器的字体大小font-size是16px，也就是1rem=16px。而如果将HTML的font-size设置为100px，方便后续计算，不设置为10px是因为chrome下最小字体大小为12px
﻿
【ex】
ex是指所用字体中小写x的高度。但不同字体x的高度可能不同。实际上，很多浏览器取em值一半作为ex值
[注意]ex在实际中常用于微调
﻿
【ch】
ch与ex类似，被定义为数字0的宽度。当无法确定数字0宽度时，取em值的一半作为ch值
﻿
兼容性: IE8-不支持
﻿
[注意]ch在实际中主要用于盲文排版
﻿
## 视口相关相对长度单位
关于视口相关的单位有vh、vw、vmin、vmax4个单位
兼容性:IE8-不支持，IOS7.1-不支持，android4.3-不支持(对于vmax，所有IE浏览器都不支持)
[注意]黑莓错误的将其相对于视觉视口来计算；而safari奇怪地相对于html元素来计算，如果html中增加了内容，这两个单位也会发生变化
﻿
【vw】【vh】
布局视口宽度的 1/100
布局视口高度的 1/100
```
<style>
body{margin: 0;}
.vhbox{
    /* 实现与屏幕等高的效果 */
    height: 100vh;
    background-color: lightblue;
}    
</style>
```
﻿
【vmin】
布局视口高度和宽度之间的最小值的 1/100
```
/*类似于contain效果*/
.box{
    height: 100vmin;
    width: 100vmin;
}
```
![vmin](https://images2015.cnblogs.com/blog/740839/201605/740839-20160512165524749-1201120087.png)
﻿
【vmax】
布局视口高度和宽度之间的最大值的 1/100
```
/*类似于cover效果*/
.box{
    height: 100vmax;
    width: 100vmax;
}    
```
![vmax](https://images2015.cnblogs.com/blog/740839/201605/740839-20160512165540359-1863293064.png)