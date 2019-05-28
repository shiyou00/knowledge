## HTML meta元素
> `<meta>`标签(meta-information)用于提供页面有关的元数据，除了提供文档字符集、使用语言、作者等基本信息外，还涉及对关键词和网页等级的设定。通过设置不同的属性，元数据可以分为以下几种:
﻿
```
<meta charset="utf-8"/> // charset声明声明当前文档所使用的字符编码，但该声明可以被任何一个元素的lang特性的值覆盖。文档的编码一定要与文件本身的编码保持一致，否则会出现乱码，推荐使用UTF-8编码
<meta name="keywords" content="HTML, CSS, XML" /> // 关键词
<meta name="description" content="Free Web tutorials on HTML, CSS, JavaScript" /> // 描述
<meta name="author" content="xxxx"> // 作者
<meta name="copyright" content="版权"> // 版权
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" /> // 视口设置
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" /> // IE浏览器渲染 如果安装了GCF(Google Chrome Frame谷歌内嵌浏览器框架GCF)，则使用GCF来渲染页面，如果没有安装，则使用最高版本的IE内核进行渲染
<meta name="renderer" content="webkit"> // 如果是双核浏览器，则使用webkit内核渲染
<meta http-equiv="refresh" content="5"> // 让网页多少秒刷新
<meta http-equiv="refresh" content="5;url=http://www.baidu.com"> // 跳转到其他网页
<meta http-equiv="Expires" Content="Sat Nov 28 2016 21:19:15 GMT+0800"> // 可以用于设定网页的到期时间，一旦过期则必须到服务器上重新调用。需要注意的是必须使用GMT时间格式
<meta http-equiv="Pragma" Content="No-cach"> // 用于设定禁止浏览器从本地机的缓存中调阅页面内容，用户无法脱机浏览
<meta http-equiv="windows-Target" content="_top"> // 强制页面在当前窗口中以独立页面显示，可以防止自己的网页被别人当作一个frame页调用
```
﻿
【base标签】
```
<base href="http://cnblogs.com" target="_blank"> // 用于指定文档里所有相对URL地址的基础URL，为页面上所有链接规定默认地址和默认打开方式。文档中的基础URL可以使用document.baseURI进行查询
```