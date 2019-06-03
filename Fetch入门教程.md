## 前言
Fetch API 提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局 fetch()方法，该方法提供了一种简单，合乎逻辑的方式来跨网络异步获取资源。本文将详细介绍fetch的相关内容

## 概述
　　跨网络异步获取资源的功能以前是使用 XMLHttpRequest实现的。Fetch提供了一个更好的替代方法，可以很容易地被其他技术使用，例如 Service Workers。Fetch还提供了单个逻辑位置来定义其他HTTP相关概念，例如 CORS和HTTP的扩展

　　fetch 规范与 jQuery.ajax() 主要有两种方式的不同

　　1、当接收到一个代表错误的 HTTP 状态码时，从 fetch()返回的 Promise 不会被标记为 reject， 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ）， 仅当网络故障时或请求被阻止时，才会标记为 reject

　　2、默认情况下, fetch 不会从服务端发送或接收任何 cookies，如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 credentials 选项）

### fetch请求
　　一个基本的 fetch请求设置起来很简单。这里我们通过网络获取一个图像并将其插入到一个 `<img>` 元素中。最简单的用法是只提供一个参数用来指明想fetch到的资源路径，然后返回一个包含响应结果的promise(一个 Response 对象)
```
let myImage = document.querySelector('img');

fetch('flowers.jpg')
.then(function(response) {
    return response.blob();
})
.then(function(myBlob) {
    let objectURL = URL.createObjectURL(myBlob);
    myImage.src = objectURL;
});
```

### 自定义参数
fetch() 接受第二个可选参数，一个可以控制不同配置的 init 对象：
```
var myHeaders = new Headers();

var myInit = { method: 'GET',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' };

fetch('flowers.jpg',myInit)
.then(function(response) {
  return response.blob();
})
.then(function(myBlob) {
  var objectURL = URL.createObjectURL(myBlob);
  myImage.src = objectURL;
});
```

### post
```
// 通过fetch获取百度的错误提示页面
fetch('https://www.baidu.com/search/error.html', {
    method: 'POST',
    body: JSON.stringify({a:1}),
　  headers: { ...new Headers(headers), 'Content-Type': 'application/json' }
  })
  .then((res)=>{
    return res.text()
  })
  .then((res)=>{
    console.log(res)
  })
```

### JSON数据
```
var url = 'https://example.com/profile';
var data = {username: 'example'};

fetch(url, {
  method: 'POST', // or 'PUT'
  body: JSON.stringify(data), // data can be `string` or {object}!
  headers: new Headers({
    'Content-Type': 'application/json'
  })
}).then(res => res.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

### 上传文件
可以通过HTML `<input type="file" />`元素，`FormData()` 和`fetch()`上传文件。

上传单个文件：
```
var formData = new FormData();
var fileField = document.querySelector("input[type='file']");

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
.then(response => response.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

上传多个文件
```
var formData = new FormData();
var photos = document.querySelector("input[type='file'][multiple]");

formData.append('title', 'My Vegas Vacation');
formData.append('photos', photos.files);

fetch('https://example.com/posts', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(response => console.log('Success:', JSON.stringify(response)))
.catch(error => console.error('Error:', error));
```


### 检测成功
　　如果遇到网络故障，fetch() promise 将会 reject，带上一个 TypeError 对象。虽然这个情况经常是遇到了权限问题或类似问题——比如 404 不是一个网络故障。想要精确的判断 fetch() 是否成功，需要包含 promise resolved 的情况，此时再判断 Response.ok 是不是为 true

```
fetch('flowers.jpg').then(function(response) {
  if(response.ok) {
    response.blob().then(function(myBlob) {
      var objectURL = URL.createObjectURL(myBlob);
      myImage.src = objectURL;
    });
  } else {
    console.log('Network response was not ok.');
  }
})
.catch(function(error) {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});
```

###  强制发送cookie

为了让浏览器发送包含凭据的请求（即使是跨域源），要将credentials: 'include'添加到传递给 fetch()方法的init对象。
```
fetch('https://example.com', {
  credentials: 'include'
})
```

如果你只想在请求URL与调用脚本位于同一起源处时发送凭据，请添加credentials: 'same-origin'。
```
fetch('https://example.com', {
  credentials: 'same-origin'  
})
```

要改为确保浏览器不在请求中包含凭据，请使用credentials: 'omit'。
```
fetch('https://example.com', {
  credentials: 'omit'
})
```

### Headers
　　使用 Headers 的接口，可以通过 Headers() 构造函数来创建一个自己的 headers 对象。一个 headers 对象是一个简单的多名值对：
```
var content = "Hello World";
var myHeaders = new Headers();
myHeaders.append("Content-Type", "text/plain");
myHeaders.append("Content-Length", content.length.toString());
myHeaders.append("X-Custom-Header", "ProcessThisImmediately");
```

也可以传一个多维数组或者对象字面量：
```
myHeaders = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
```

它的内容可以被获取：
```
console.log(myHeaders.has("Content-Type")); // true
console.log(myHeaders.has("Set-Cookie")); // false
myHeaders.set("Content-Type", "text/html");
myHeaders.append("X-Custom-Header", "AnotherValue");
 
console.log(myHeaders.get("Content-Length")); // 11
console.log(myHeaders.getAll("X-Custom-Header")); // ["ProcessThisImmediately", "AnotherValue"]
 
myHeaders.delete("X-Custom-Header");
console.log(myHeaders.getAll("X-Custom-Header")); // [ ]
```

### 封装
下面是对fetch的一个简单的封装
```
function _fetch(url, data, method = 'GET',options={}) {
    const body = o2s(data);
    let params = {
        method: method,
    };
    if (method === 'GET') { // 如果是GET请求，拼接url
        url += '?' + body;
    } else {
         params.body=body
    }
    if(options.cookie!=undefined){
        params.credentials='include'
    }
    if(options.headers!=undefined && typeof options.headers=="object"){
        params.headers=new Headers(options.headers);
    }else{
        params.headers=new Headers({
            'Accept': 'application/json',
            'Content-Type': 'application/x-www-form-urlencoded'
        });
    }
    fetch(url, params).then(r => options.dataType=="text"?r.text():r.json()).then(r => r);
}
export function o2s(obj, arr = [], idx = 0) {
    for (let item in obj) {
        arr[idx++] = [item, obj[item]];
    }
    return new URLSearchParams(arr).toString();
}
export function get(url, data,,options={}) {
    return _fetch(url, data, 'GET',options);
}

export function post(url, data,options={}) {
    return _fetch(url, data, 'POST',options);
}
post("/api/test",{title:"标题"},{
    dataType:"json",
    cookie:true,
    headers:{
        'Accept': 'application/json',
        'Content-Type': 'application/x-www-form-urlencoded'
    }
});
```
