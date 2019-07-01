## 前言
cookie是一种早期的客户端存储机制，起初是针对服务器端脚本设计使用的，只适合存储少量文本数据。从最底层来看，作为HTTP协议的一种扩展实现它。cookie数据会自动在Web浏览器和Web服务器之间传输，因此服务端脚本就可以读、写存储在客户端的cookie的值。任何以cookie形式存储的数据，不论服务器端是否需要，每一次HTTP请求都会把这些数据传输到服务器端。

## 概述
“cookie”这个名字没有太多的含义，但是在计算机历史上其实很早就用到它了。“cookie”和“magic cookie”用于代表少量数据，特别是指类似密码这种用于识别身份或者许可访问的保密数据。在javascript中，cookie用于保存状态以及能够为Web浏览器提供一种身份识别机制。但是，javascript中使用cookie不会采用任何加密机制，因此它们是不安全的。但是，通过https来传输cookie数据是安全的，不过这和cookie本身无关，而和https协议相关

HTTP Cookie，通常直接叫做cookie，最初是在客户端用于存储会话信息的。该标准要求服务器对任意HTTP请求发送Set-Cookie HTTP头作为响应的一部分，其中包含会话信息。例如，这种服务器响应的头可能如下

```
HTTP/1.1 200 OK
Content-type: Text/html
Set-Cookie: name=value
Other-header: other-header-value
```

这个HTTP响应设置以name为名称、以value为值的一个cookie，名称和值在传送时都必须是URL编码的。浏览器会存储这样的会话信息，并在这之后，通过为每个请求添加Cookie HTTP头将信息发送回服务器，如下所示：

```
GET /index.html HTTP/1.1
Cookie: name=value
Other-header: other-header-value
```

发送回服务器的额外信息可以用于唯一验证客户来自于发送的哪个请求

## 标识
浏览器默认打开Cookie功能。window.navigator.cookieEnabled属性返回一个布尔值，表示浏览器是否打开Cookie功能

```
window.navigator.cookieEnabled // true
```
可以通过浏览器的一些设置将cookie功能关闭

![](./image/270.png)  

```
window.navigator.cookieEnabled // false
```

此时，客户端本地将不再存储任何cookie


## 限制

cookie在性质上是绑定在特定的域名下的。当设定了一个cookie后，再给创建它的域名发送请求时，都会包含这个cookie。这个限制确保了储存在cookie中的信息只能让批准的接受者访问，而无法被其他域访问

[注意]不同的浏览器存入的cookie位置不一样，不能通用

由于cookie是存在客户端计算机上的，还加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间。每个域的cookie总数是有限的，不过浏览器之间各有不同。如下所示

- IE6-浏览器限制每个域名最多20个cookie
- IE7+浏览器限制每个域名最多50个。IE7最初是支持每个域名最大20个cookie，之后被微软的一个补丁所更新
- Firefox限制每个域最多50个cookie
- Opera限制每个域最多30个cookie
- Safari和Chrome对于每个域的cookie数量限制没有硬性规定

当超过单个域名限制之后还要再设置cookie，浏览器就会清除以前设置的cookie。IE和Opera会删除最近最少使用过的(LRU, LeastRecentlyUsed)cookie，腾出空间给新设置的cookie。Firefox看上去好像是随机决定要清除哪个cookie，所以考虑cookie限制非常重要，以免出现不可预期的后果

浏览器中对于cookie的尺寸也有限制。大多数浏览器都有大约4096B(加减1)的长度限制。为了最佳的浏览器兼容性，最好将整个cookie长度限制在4095B(含4095)以内。尺寸限制影响到一个域下所有的cookie，而并非每个cookie单独限制

如果尝试创建超过最大尺寸限制的cookie，那么该cookie会被悄无声息地丢掉。注意，虽然一个字符串常占用一字节，但是多字节情况则有不同

【同源】  
两个网址只要域名相同和端口相同，就可以共享Cookie。注意，这里不要求协议相同

也就是说，http://example.com设置的Cookie，可以被https://example.com读取


## 组成
cookie由浏览器保存的以下7块信息构成

```
Set-Cookie: name=value[; expires=date][; max-age=secondes][; domain=domain][; path=path][; secure]
```

　　1、名称：唯一确定cookie的名称。cookie名称是不区分大小写的，所以myCookie和MyCookie被认为是同一个cookie。然而，实践中最好将cookie名称看作是区分大小写的，因为某些服务器会这样处理cookie。cookie的名称必须是经过URL编码的

　　2、值：储存在cookie中的字符串值。值必须被URL编码

　　3、域：cookie对于哪个域是有效的。所有向该域发送的请求中都会包含这个cookie信息。这个值可以包含子域(subdomain，如www.wrox.com)，也可以不包含它(如.wrox.com，则对于wrox.com的所有子域都有效)。如果没有明确设定，那么这个域会被认作来自设置cookie的那个域

　　4、路径：对于指定域中的路径，必须是绝对路径（比如/、/books），如果未指定，默认为请求该Cookie的网页路径。例如，可以指定cookie只有从"http://www.wrox.com/books/"中才能访问，那么http://www.wrox.com的页面就不会发送cookie信息，即使请求都是来自同一个域的

　　[注意]这里的匹配不是绝对匹配，而是从根路径开始，只要path属性匹配发送路径的一部分，就可以发送。比如，path属性等于/blog，则发送路径是/blog或者/blogroll，Cookie都会发送。path属性生效的前提是domain属性匹配

　　5、失效时间(expires)：表示cookie何时应该被删除的时间戳(也就是，何时应该停止向服务器发送这个cookie)。默认情况下，浏览器会话结束时即将所有cookie删除；不过也可以自己设置删除时间。这个值是个GMT格式的日期(Wdy,DD-Mon-YYYY HH:MM:SS GMT)，用于指定应该删除cookie的准确时间。因此，cookie可在浏览器关闭后依然保存在用户的机器上。如果设置的失效日期是个以前的时间，则cookie会被立刻删除

```
document.cookie = "a = 2; expires = " + (new Date( +new Date() + 4000*60*60*24 )).toUTCString();
```

　　[注意1]必须使用toUTCString()或者toGMTString()，如果使用toString()会因为时区问题，导致时间设置错误

　　[注意2]浏览器根据本地时间，决定Cookie是否过期，由于本地时间是不精确的，所以没有办法保证Cookie一定会在服务器指定的时间过期

　　6、有效期(max-age)：表示cookie有效期为多久，单位为秒(s)
```
document.cookie = "b = 3; max-age=60";
```

　　7、安全标志：指定后，cookie只有在使用SSL连接的时候才发送到服务器。例如，cookie信息只能发送给"https:www.wrox.com"，而"http:www.wrox.com"的请求则不能发送cookie

　　每一段信息都作为Set-Cookie头的一部分，使用分号加空格分隔每一段，如下所示

```
HTTP/1.1 200 0K
Content-type: text/html
Set-Cookie: name=value; expires=Mon, 22-Jan-17 07:10:24 GMT; domains=.wrox.com
Other-header: other-header-value
```

　　该头信息指定了一个叫做name的cookie，它会在格林威治时间2017年1月22日7:10:24失效，同时对于www.wrox.com和wrox.com的任何子域(如p2p.wrox.com)都有效

secure标志是cookie中唯一一个非名值对儿的部分，直接包含一个secure单词。如下所示
```
HTTP/1.1 200 0K
Content-type: text/html
Set-Cookie: name=value; domain=.wrox.com; path=/; secure
Other-header: other-header-value
```

这里，创建了一个对于所有wrox.com的子域和域名下(由path参数指定的)所有页面都有效的cookie。因为设置了secure标志，这个cookie只能通过SSL连接才能传输

[注意]域、路径、失效时间、有效期和安全标志都是服务器给浏览器的指示，以指定何时应该发送cookie。这些参数并不会作为发送到服务器的cookie信息的一部分，只有名值对儿才会被发送

## 读取
通过document.cookie属性可以获取cookie的值，其返回值是一个字符串，该字符串都是由一系列名值对儿组成，不同名/值对之间通过“分号和空格”分开，其内容包含了所有作用在当前文档的cookie。但是，它并不包含其他设置的cookie属性

```
document.cookie = "name=match; domain=127.0.0.1; path=/test";
console.log(document.cookie);//'age=32; name=match'
```

但是为了更好地査看cookie的值，一般会采用split()方法将cookie值中的名/值对都分离出来

把cookie的值从cookie属性分离出来之后，必须要采用相应的解码方式(取决于之前存储cookie值时采用的编码方式)，把值还原出来。比如，先采用decodeURIComponent()方法把cookie值解码出来，之后再利用JSON.parse()方法转化成json对象

```
function getCookie(key){
    var arr1 = document.cookie.split("; ");
    for(var i = 0; i < arr1.length; i++){
        var arr2 = arr1[i].split("=");
        if(arr2[0] == key){
            return decodeURIComponent(arr2[1]);
        }
    }
}

console.log(getCookie('name'));//'match'
console.log(getCookie('age'));//'32'
```

## 设置
当用于设置值的时候，document.cookie属性可以设置为一个新的cookie字符串。这个cookie字符串会被解释并添加到现有的cookie集合中。设置document.cookie并不会覆盖cookie，除非设置的cookie的名称已经存在。设置cookie的格式如下，和Set-Cookie头中使用的格式一样

```
name=value;expires=expiration_time;path=domain_path; domain=domain_name;secure 
```

这些参数中，只有cookie的名字和值是必需的。这段代码创建了一个叫name的cookie，值为Nicholas。当客户端每次向服务器端发送请求的时候，都会发送这个cookie; 当浏览器关闭的时候，它就会被删除
```
document.cookie = "name=Nicholas";
```

以简单的名/值对形式存储的cookie数据有效期只在当前Web浏览器的会话内，一旦用户关闭浏览器，cookie数据就丢失了。如果想要延长cookie的有效期，就需要设置max-age属性来指定cookie的有效期(单位是秒)。按照如下的字符串形式设置cookie属性即可：
```
name=value;max-age=seconds
```

由于cookie的名/值中的值是不允许包含分号、逗号和空白符，因此，在存储前一般可以采用encodeURIComponent()对值进行编码。相应的，读取cookie值的时候需要采用decodeURIComponent()函数解码

要给被创建的cookie指定额外的信息，只要将参数追加到该字符串，和Set-Cookie头中的格式一样，如下所示
```
document.cookie = encodeURIComponent("name")+"="+encodeURIComponent("Nicholas") + ";domain=.wrox.com;path=/";
```

下面的函数用来设置一个cookie的值，同时提供一个可选的max-age属性
```
function setCookie(key,value,d){
    if(d === undefined){
        document.cookie = encodeURIComponent(key) + "=" + encodeURIComponent(value);
    }else{
        document.cookie = encodeURIComponent(key) + "=" + encodeURIComponent(value) + ";max-age=" + (d*60*60*24);        
    }
}
```

【改变】  
要改变cookie的值，需要使用相同的名字、路径和域，但是新的值重新设置cookie的值。同样地，设置新max-age属性就可以改变原来的cookie的有效期


【删除】  
要删除一个cookie，需要使用相同的名字、路径和域，然后指定一个任意(非空)的值，并且将max-age属性指定为0，再次设置cookie

## 读写差异
document.cookie属性一次可以读出全部Cookie，但是只能写入一个Cookie，与服务器与浏览器之间的Cookie通信格式有关。浏览器向服务器发送Cookie的时候，是一行将所有Cookie全部发送

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: cookie_name1=cookie_value1; cookie_name2=cookie_value2
Accept: */*
```

上面的头信息中，Cookie字段是浏览器向服务器发送的Cookie

服务器告诉浏览器需要储存Cookie的时候，则是分行指定

```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: cookie_name1=cookie_value1
Set-Cookie: cookie_name2=cookie_value2; expires=Sun, 16 Jul 3567 06:23:41 GMT
```
上面的头信息中，Set-Cookie字段是服务器写入浏览器的Cookie，一行一个

## 子cookie
为了绕开浏览器的单域名下的cookie数限制，一些开发人员使用了一种称为子cookie(subcookie)的概念。子cookie是存放在单个cookie中的更小段的数据。也就是使用cookie值来存储多个名称值对儿。子cookie最常见的格式如下所示

```
name=name1=value1&name2=value2&name3=value3&name4=value4&name5=value5
```
子cookie一般也以査询字符串的格式进行格式化。然后这些值可以使用单个cookie进行存储和访问，而非对每个名称——值对儿使用不同的cookie存储。最后网站或者Web应用程序可以无需达到单域名cookie上限也可以存储更加结构化的数据

为了更好地操作子cookie，必须建立一系列新方法。子cookie的解析和序列化会因子cookie的期望用途而略有不同并更加复杂些


## HTTP专有
设置Cookie的时候，如果服务器加上了HttpOnly属性，则这个Cookie无法被javascript读取(即document.cookie不会返回这个Cookie的值)，只能从服务器端读取。进行AJAX操作时，XMLHttpRequest对象也无法包括这个Cookie。这主要是为了防止XSS攻击盗取Cookie

[注意]cookie依然保存在客户端中，只是无法被document.cookie读取

## 小结
1. cookie 会自动发送给服务器
2. 服务端可以为客户端设置cookie，同时通过设置HttpOnly属性，放置客户端读取
3. 浏览器通过window.navigator.cookieEnabled查询cookie是否启用
4. cookie在有些浏览器上是有数量限制的(Safari和Chrome对于每个域的cookie数量限制没有硬性规定)，通过“子cookie”可以绕开该限制
5. 共享cookie，要满足域名相同和端口相同(协议不要求)
6. cookie包含 name、value、expires、max-age、domain、path、secure 7个设置项
7. document.cookie属性可以读取和设置cookie的值
8. 想要删除一个cookie，只需将其过期时间设置为小于等于当前时间即可
9. 设置cookie时，未设置max-age以及expires时，则是一个会话的时间


