## 目前实现上传的方式

### 浏览器小于等于IE9(低版本浏览器)使用下面的方式实现的
1. flash实现（主流插件的方式，本文不涉及）
2. form + iframe（项目中很少用到，本文不涉及）
> form表单提交的方式是所有浏览器都支持的，借助iframe是为了实现不刷新界面上传

### 主流浏览器 + IE10+ 则是通过以下方式实现的上传
FormData + XHR2 + FileReader + canvas

## FormData介绍
> FormData接口提供了一种轻松构造一组表示表单字段及其值的键/值对的方法，然后可以使用XMLHttpRequest.send()方法轻松地发送这些值。如果将编码类型设置为“multipart/form-data”，则使用与表单相同的格式。

常用方法：
```
FormData.append(); // 添加键值对
FormData.delete(); // 删除键值对
FormData.entries(); // 返回允许遍历此对象中包含的所有键/值对的迭代器
```
...具体还有很多方法可以参考网站：[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)


## XMLHttpRequest简介
> 使用XMLHttpRequest (XHR)对象可以与服务器交互。您可以从URL获取数据，而无需让整个的页面刷新。这使得Web页面可以只更新页面的局部，而不影响用户的操作。XMLHttpRequest在 Ajax 编程中被大量使用。

常用属性和方法
```
const xhr = new XMLHttpRequest()
// 常用属性
xhr.onreadystatechange // 当readyState属性发生变化时调用的函数
xhr. readyState // 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
0: 请求未初始化
1: 服务器连接已建立
2: 请求已接收
3: 请求处理中
4: 请求已完成，且响应已就绪

xhr.responseText // 包含对请求的响应，如果请求未成功或尚未发送，则返回null
xhr.timeout // 表示该请求的最大请求时间（毫秒），超过该时间请求会自动结束。
xhr.upload  // 表示上传过程。

// 常用方法
xhr.abort() // 中止请求
xhr.open() // 初始化一个请求。该方法只能JavaScript代码中使用
xhr.setRequestHeader() // 设置HTTP请求头的值
xhr.send() // 发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回
```
更多关于XMLHttpRequest的信息点击：[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)


## FileReader
> 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

> 其中File对象可以是来自用户在一个`<input>`元素上选择文件后返回的FileList对象,也可以来自拖放操作生成的 DataTransfer对象,还可以是来自在一个HTMLCanvasElement上执行mozGetAsFile()方法后返回结果。

属性：
```
FileReader.error // 表示在读取文件时发生的错误
FileReader.readyState // 表示FileReader状态的数字。0:还没有加载任何数据; 1:数据正在被加载；2:已完成全部的读取请求
FileReader.result // 文件的内容。该属性仅在读取操作完成后才有效，数据的格式取决于使用哪个方法来启动读取操作。
```

事件处理
```
FileReader.onabort= ()=>{} // 该事件在读取操作被中断时触发
FileReader.onerror = ()=>{} // 该事件在读取操作发生错误时触发。
FileReader.onload = ()=>{} // 该事件在读取操作完成时触发。
FileReader.onloadstart = ()=>{} // 该事件在读取操作开始时触发
FileReader.onloadend = ()=>{} // 该事件在读取操作结束时（要么成功，要么失败）触发
FileReader.onprogress = ()=>{} // 该事件在读取Blob时触发
```

方法
```
FileReader.abort() // 中止读取操作。在返回时，readyState属性为DONE
FileReader.readAsArrayBuffer() // 开始读取指定的 Blob中的内容, 一旦完成, result 属性中保存的将是被读取文件的 ArrayBuffer 数据对象
FileReader.readAsDataURL() // 开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容
FileReader.readAsText() // 开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个字符串以表示所读取的文件内容。
```

---------------------------------------------

<h2 id=1>form表单上传</h2>

```
<form id="uploadForm" method="POST" action="upload" enctype="multipart/form-data">
      <input type="file" id="myFile" name="file" />
      <input type="submit" value="提交" />
</form>
```
所有浏览器都支持的上传方式，且submit提交后页面会刷新。  
action: 提交地址  
enctype的常见类型(告诉服务器我们发送过去的数据是用哪种格式进行编码的)  
- application/x-www-form-urlencoded (默认数据编码方式)
- multipart/form-data(复杂，但它允许在数据中包含整个文件，所以常用于文件上传)
- text/plain(一般用于debug)

---------------------------------------------

<h2 id=5>FormData + XHR2 + FileReader + canvas</h2>

【实现步骤】
1. 监听一个input(type=‘file’)的onchange事件，这样获取到文件file；
2. 将file转成dataUrl;
3. 然后根据dataUrl利用canvas绘制图片压缩，然后再转成新的dataUrl；
4. 再把dataUrl转成Blob；
5. 把Blob append进FormData中；
6. xhr实现上传。

HTML代码
```
<input type="file" name="file" accept=“image/*” onchange='handleInputChange(event)'>
```

1、监听input的change事件
```
function handleInputChange (event) {
    const file = event.target.files[0];  // 获取当前选中的文件
    const imgMasSize = 1024 * 1024 * 10; // 限制大小10MB
   
    // 检查文件类型
    if(['jpeg', 'png', 'gif', 'jpg'].indexOf(file.type.split("/")[1]) < 0){
        // 不支持该文件类型
    }

    // 文件大小限制
    if(file.size > imgMasSize ) {
        // 文件大小自定义限制
    }

    // 图片压缩处理函数
    transformFileToDataUrl(file);
}
```

2、将file转成dataUrl
```
function transformFileToDataUrl (file) {
    const imgCompassMaxSize = 200 * 1024; // 超过 200k 就压缩

    // 存储文件相关信息
    imgFile.type = file.type || 'image/jpeg';
    imgFile.size = file.size;
    imgFile.name = file.name;
    imgFile.lastModifiedDate = file.lastModifiedDate;

    // 封装好的函数
    const reader = new FileReader();

    // file转dataUrl是个异步函数,onload表示读取完成了
    reader.onload = function(e) {
        const result = e.target.result;

        if(result.length < imgCompassMaxSize) {
            compress(result, processData, false );    // 图片不压缩
        } else {
            compress(result, processData);            // 图片压缩
        }
    };

    reader.readAsDataURL(file);
}
```

3、canvas进行压缩的处理
```
function compress (dataURL, callback, shouldCompress = true) {
    const img = new window.Image(); // new 一个图片对象

    img.src = dataURL;  // 通过fileReader读取到的base64数据

    img.onload = function () {
        // 1、创建canvas上下文
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        // 获取图片宽高赋值给canvas绘图
        canvas.width = img.width;
        canvas.height = img.height;
        // 绘制出一张canvas图片
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

        let compressedDataUrl;

        if(shouldCompress){
            compressedDataUrl = canvas.toDataURL(imgFile.type, 0.2); // 后面的系数是绘图输出图片质量
        } else {
            compressedDataUrl = canvas.toDataURL(imgFile.type, 1); // 不改变原图质量
        }

        document.getElementById('preview').appendChild(img);
        callback(compressedDataUrl); // 最后图片压缩好后，去进行base64 -> blob的转换(传递到后台)
    }
}
```

4、把Blob append进FormData中；
```
function processData (dataURL) {

    const binaryString = window.atob(dataURL.split(',')[1]); // window.atob对用base-64编码过的字符串进行解码 
    const arrayBuffer = new ArrayBuffer(binaryString.length); // ArrayBuffer 对象用来表示通用的、固定长度的原始二进制数据缓冲区。ArrayBuffer 不能直接操作，而是要通过类型数组对象或 DataView 对象来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。
    const intArray = new Uint8Array(arrayBuffer); // Uint8Array类型化数组表示一个由8位无符号整数组成的数组。内容初始化为0。一旦建立，您可以使用对象的方法或使用标准数组索引语法(即使用括号符号)引用数组中的元素。

    for (let i = 0, j = binaryString.length; i < j; i++) {
        intArray[i] = binaryString.charCodeAt(i);
    }

    const data = [intArray];

    let blob;

    // 通过兼容性判断，最后转换为二进制数据
    try {
        blob = new Blob(data, { type: imgFile.type });
    } catch (error) {
        window.BlobBuilder = window.BlobBuilder ||
            window.WebKitBlobBuilder ||
            window.MozBlobBuilder ||
            window.MSBlobBuilder;
        if (error.name === 'TypeError' && window.BlobBuilder){
            const builder = new BlobBuilder();
            builder.append(arrayBuffer);
            blob = builder.getBlob(imgFile.type);
        } else {
            throw new Error('版本过低，不支持上传图片');
        }
    }

    // blob 转 file
    const fileOfBlob = new File([blob], imgFile.name); // File 对象是特殊类型的 Blob，且可以用在任意的 Blob 类型的 context 中。
    const formData = new FormData(); // 把要传输的数据添加到FormData()对象中

    // type
    formData.append('type', imgFile.type);
    // size
    formData.append('size', fileOfBlob.size);
    // name
    formData.append('name', imgFile.name);
    // lastModifiedDate
    formData.append('lastModifiedDate', imgFile.lastModifiedDate);
    // append 文件
    formData.append('file', fileOfBlob);

    uploadImg(formData); // 调用xhr发送数据到后台
}
```

5、xhr实现上传
```
function uploadImg (formData) {
    const xhr = new XMLHttpRequest();

    // 进度监听
    xhr.upload.addEventListener('progress', (e)=>{
        console.log(e, e.loaded , e.total); // 可以利用这两个对象算出目前的传输比例
    }, false);

    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
            const result = JSON.parse(xhr.responseText);
            if (xhr.status === 200) {
                // 上传成功
                console.log(result);
            } else {
                // 上传失败
            }
        }
    };
    xhr.open('POST', '/upload' , true); // 中间"/upload"为后台上传地址(如果需要兼容性强可以使用限制的ajax库)
    xhr.send(formData); // 发送到后台
}
```
代码github访问地址：[上传实例代码](https://github.com/shiyou00/uploadFiles/tree/master/upload)

## 小结
通过本节内容，我们应该彻底的理解了前端上传是如何实现的，给出的代码实例虽然简单，但是已经是非常核心，我们可以通过这个版本去实现一个非常复杂的需求，譬如多图片上传，那么也就是遍历的调用transformFileToDataUrl这个方法去实现。在例如添加上拖拽文件到指定区域去上传，那么我们只需要了解下drag对象就可以很轻松的实现了。
