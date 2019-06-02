## 字体系列
【1】5种通用字体系列：拥有相似外观的字体系列
serif字体:字体成比例，且有上下短线(衬线字体)，包括Times\Georgia\New century Schoolbook

sans-serif字体:字体成比例，且没有上下短线(无衬线字体)，包括Helvetica\Geneva\Verdana\Arial\Univers

Monospace字体:字体不成比例，等宽字体，包括Courier\Courier New\Andale Mono

Cursive字体:手写体，包括Zapf Chancery\Author\Comic Sans

Fantasy字体:无法归类的字体，包括Western\Woodblock\Klingon

【2】特定字体系列：具体的字体系列
```
font-family:"宋体";
font-family:"arial";
```

【3】默认字体系列  
chrome/opera:"宋体"
firefox:"微软雅黑"
safari/IE:Times,"宋体"

```
font-family:字体系列1,字体系列2 ……
//【注意】若浏览器识别第一个字体，则以第一个字体显示；如果不识别，则尝试下一个。    
font-family: arial，“宋体”,“微软雅黑”;
//【注意】若写英文字体，一定要把英文字体写在前面，英文字体会影响到英文、数字和标点符号。
font-family: Times, 'New Century Schoolbook','New York', serif;
//【注意】若字体名中有一个或多个空格，要添加引号
```

【4】中文字体

对于中文字体来说，常见的是宋体和微软雅黑。宋体是衬线字体，而微软雅黑是无衬线字体。衬线字体常用于排版印刷，而无衬线字体则常用于网页中

一般地，一行中有30-40个文字时，行高为1.5时，有较好的阅读体验。对于标题来说， 更好的样式是取消其加粗设置，并改变其颜色，增加页面的层次感

## 字体加粗

【1】常用值
```
font-weight: normal(正常，默认)
font-weight: bold(加粗)
```
【2】所有值  
normal(正常)/bold(粗体)/bolder(更粗)/lighter(更细)  
100/200/300/400/500/600/700/800/900 (100为最细，900为最粗)

## 字体大小

【1】绝对字体大小：xx-small/x-small/small/medium/large/x-large/xx-large

【2】相对字体大小：smaller/larger

【3】em/%：1em = 100%

【4】默认字体大小：chrome/firefox/opera/IE/safari:16px

【5】最小字体大小：chrome:12px    opera:9px    safari/IE/firefox:无

## font-size
> font-size字体大小设置的是字体中字符em框的高度，实际的字符字形通常比字符em框要矮，与字体类型有关

值: xx-small | x-small | small | medium | large | x-large | xx-large | smaller | larger | `<length>` | `<percentage>` | inherit

初始值: medium
应用于: 所有元素
继承性: 有
百分数: 相对于父元素的字体大小font-size

效果展示：
<iframe style="width: 100%; height: 140px;" src="https://shiyou00.github.io/lion/dist/html/css-font/font.html?case=f1" frameborder="0"></iframe>

## 字体风格
```
font-style: normal(默认)
font-style: italic(斜体)
font-style: oblique(倾斜)
```

## 字体
```
font: [[<font-style> || <font-variant> || <font-weight>]? <font-size>[/<line-height>?<font-family>]
```
[注意]对于font-size，百分数相对于父元素来计算；对于line-height，百分数相对于元素的font-size来计算

## 关键字
CSS标准定义了6个系统字体关键字：
```
caption: 由标题控件使用的字体样式，如按钮和下拉控件

icon: 系统图标所用的字体样式，如文件夹和文件图标

menu: 下拉菜单和菜单列表中文本使用的字体样式

message-box: 对话框中文本使用的字体样式

small-caption: 由标题小控件的标签使用的字体样式

status-bar: 窗口状态条中文本使用的字体样式
```

## font-face
```
@font-face {
    font-family: 自定义名称;
    src: url(../font/test.eot);
    src: url(../font/test.eot?#iefix) format("embedded-opentype"),
         url(../font/test.woff) format("woff"), 
         url(../font/test.ttf) format("truetype"),
         url(../font/test.svg#jq) format("svg");
}
EOT:IE专用
WOFF:标准
TTF:最常见(safari/android/ios)
SVG:图形格式(IE和firefox不支持)
```

两种调用字体的方法
【1】html(&#x + 小图标对应的unicode编码)
```
div{
    font-family: 自定义名称;
    -webkit-font-smoothing:antialiased;//字体抗锯齿、光滑度属性
    -mox-osx-font-smoothing: grayscale;//字体抗锯齿、光滑度属性
}
<div>&#xf048</div>
```

【2】css(`\` + 小图标对应的unicode编码)(不兼容IE7-浏览器)
```
div{
    font-family: 自定义名称;
    -webkit-font-smoothing:antialiased;//字体抗锯齿、光滑度属性
    -mox-osx-font-smoothing: grayscale;//字体抗锯齿、光滑度属性
}
div:before{
    content: "\f048";
}
<div></div>
```

具体实例：有一篇专门的文章讲解iconfont网站字体使用的过程，[点击查看：iconfont实战](CSSiconfont阿里巴巴矢量图库在开发中实战使用.md)
