## 什么是iframe
> HTML内联框架元素 `<iframe>` 表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。
﻿
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="300"
    height="200"
    src="https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&layer=mapnik">
</iframe>
```
﻿
【src】
嵌套页面的URL地址。使用遵守同源策略的  'about:blank' 来嵌套空白页。
﻿
## 同源策略
> 同源政策由 Netscape 公司引入浏览器。目前，所有浏览器都实行这个政策。
﻿
```
协议相同: http https ftp 指的是协议
域名相同: www.baidu.com www.taobao.com 指的是域名
端口相同: 默认是80端口 8800 8899这个指的是端口
https://www.baidu.com:80（80是默认的可以省略）
```
举例来说，http://www.example.com/dir/page.html这个网址，协议是http://，域名是www.example.com，端口是80（默认端口可以省略）。它的同源情况如下。
```
http://www.example.com/dir2/other.html：同源
http://example.com/dir/other.html：不同源（域名不同）
http://v2.www.example.com/dir/other.html：不同源（域名不同）
http://www.example.com:81/dir/other.html：不同源（端口不同）
```
﻿
## 跨文档消息传递
跨文档消息传送(cross-document messaging)，有时候简称为 XDM，指的是在来自不同域的页面间 传递消息。例如，www.wrox.com 域中的页面与位于一个内嵌框架中的 p2p.wrox.com 域中的页面通信。 在 XDM 机制出现之前，要稳妥地实现这种通信需要花很多工夫。XDM 把这种机制规范化，让我们能 既稳妥又简单地实现跨文档通信。
﻿
XDM 的核心是 postMessage()方法。在 HTML5 规范中，除了 XDM 部分之外的其他部分也会提 到这个方法名，但都是为了同一个目的:向另一个地方传递数据。对于 XDM 而言，“另一个地方”指的 是包含在当前页面中的`<iframe>`元素，或者由当前页面弹出的窗口。
﻿
## postMessage
> postMessage()方法接收两个参数:一条消息和一个表示消息接收方来自哪个域的字符串。第二 个参数对保障安全通信非常重要，可以防止浏览器把消息发送到不安全的地方
```
var iframeWindow = document.getElementById("myframe").contentWindow; 
iframeWindow.postMessage("A secret", "http://www.wrox.com");// 第一个参数如果需要传入对象请JSON.stringify()
```
最后一行代码尝试向内嵌框架中发送一条消息，并指定框架中的文档必须来源于"http:// www.wrox.com"域。如果来源匹配，消息会传递到内嵌框架中;否则，postMessage()什么也不做。
﻿
如果传给 postMessage()的第二个参 数是"*"，则表示可以把消息发送给来自任何域的文档，但我们不推荐这样做。
﻿
接收到 XDM 消息时，会触发 window 对象的 message 事件。这个事件是以异步形式触发的，因此 从发送消息到接收消息(触发接收窗口的 message 事件)可能要经过一段时间的延迟。触发 message 事件后，传递给 onmessage 处理程序的事件对象包含以下三方面的重要信息
﻿
- data:作为 postMessage()第一个参数传入的字符串数据。
- origin:发送消息的文档所在的域，例如"http://www.wrox.com"。
- source:发送消息的文档的 window 对象的代理。这个代理对象主要用于在发送上一条消息的 窗口中调用 postMessage()方法。如果发送消息的窗口来自同一个域，那这个对象就是 window。
﻿
```
window.onmessage = function(event){
    //确保发送消息的域是已知的域
    if (event.origin == "http://www.wrox.com"){
    //处理接收到的数据 
    processMessage(event.data);
    //可选:向来源窗口发送回执
    event.source.postMessage("Received!", "http://p2p.wrox.com"); }
};
```
[注意]event.source 大多数情况下只是 window 对象的代理，并非实际的 window 对 象。换句话说，不能通过这个代理对象访问 window 对象的其他任何信息。记住，只通过这个代理调用 postMessage()就好，这个方法永远存在，永远可以调用。
﻿
## 兼容
支持 XDM 的浏览器有 IE8+、Firefox 3.5+、Safari 4+、Opera、Chrome、iOS 版 Safari 及 Android 版 WebKit。XDM 已经作为一个规范独立出来，现在它的名字叫 Web Messaging，官方页面是 http://dev.w3.org/html5/postmsg/。
﻿
## 小结
通过本文我学习了什么是iframe，以及iframe如何和父窗口进行数据通信