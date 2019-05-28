## 前言
> 表单是网页与用户的交互工具，由一个`<form>`元素作为容器构成，封装其他任何数量的表单控件，还有其他任何`<body>`元素里可用的标签
﻿
表单能够包含`<input>、<menus>、<textarea>、<fieldset>、<legend>、<label>`等表单控件元素
﻿
[注意]表单里嵌套表单是不允许的
﻿
## form元素
> form元素有accept-charset、action、autocomplete、enctype、method、name、novalidate、target共8个属性，其中action和name属性为必需项
﻿
```
<form method="get" action="form.php" name="test"></form>    
<script>
    var oForm = document.forms.test;
    console.log(oForm.method);//get
</script>
```
﻿
【字符集】
accept-charset 属性是一个空格分隔的字符集列表，规定了服务器处理表单数据所接受的字符集。accept-charset 属性允许指定一系列字符集，服务器必须支持这些字符集，从而得以正确解释表单中的数据。该属性的值是用引号包含字符集名称列表。
﻿
【提交地址】
action属性规定提交表单时，向何处发送表单数据；如果忽略这个属性，表单会重定向到表单所在的URL。这个值可以被 `<button>`或者 `<input>` 元素中的 formaction属性重载(覆盖)
﻿
【数据编码】
enctype 属性规定在发送到服务器之前应该如何对表单数据进行编码。大多数情况下该属性不需要设置。这个值可以被 `<button>` 或者 `<input>` 元素中的 formenctype属性重载(覆盖)。当 method属性值为 post时， enctype是提交form给服务器的内容的 MIME 类型 。可能的取值有:
- application/x-www-form-urlencoded 　　在发送前编码所有字符（默认）
- multipart/form-data 　　　　　　　　   不对字符编码。在使用包含文件上传控件的表单时，必须使用该值
- text/plain 　　　　　　　　　　　　      空格转换为 "+" 加号，但不对特殊字符编码
﻿
【数据发送】
表单可以用两种方式(method)发送数据：GET和POST，默认为GET方法。这个值可以被 `<button>` 或者 `<input>` 元素中的 formmethod属性重载(覆盖)
﻿
【POST方法】
　　如果采用POST方法，浏览器将会按照下面两步来发送数据。首先，浏览器将与action属性中指定的表单处理服务器建立联系，一旦建立连接之后，浏览器就会按分段传输的方法将数据发送给服务器
　　在服务器端，一旦POST样式的应用程序开始执行时，就应该从一个标志位置读取参数，而一旦读到参数，在应用程序能够使用这些表单值以前，必须对这些参数进行解码。用户特定的服务器会明确指定应用程序应该如何接受这些参数
﻿
【GET方法】
如果采用GET方法，浏览器会与表单处理服务器建立连接，然后直接在一个传输步骤中发送所有的表单数据：浏览器会将数据直接附在表单的action URL之后。这两者之间用问号进行分隔。
﻿
【自动完成】
autocomplete是HTML5新增的一个属性，规定表单是否应该启用自动完成功能。当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出在字段中填写的选项
﻿
[注意]IE浏览器不支持该属性，只有元素拥有name属性，该属性才有效
﻿
```
<form autocomplete="on | off"> //该属性默认为on，当设置为off时，规定禁用自动完成功能
```
﻿
## input元素的常用type类型
﻿
传统输入控件：
button：定义可点击按钮
checkbox：复选框
file：文件上传
hidden：隐藏的输入字段（常常用于附带的一些内容传递给后台，但是又不想让用户看见的）
image：定义图像形式的提交按钮
password：密码输入
radio：单选按钮
reset：重置按钮
submit：表单提交
text：单行的文字输入
﻿
```
<form action="/" method="GET" name="testForm">
    <input type="text" value="输入文本内容">
    <br>
    <input type="button" value="button">
    <br>
    <input type="checkbox"> checkbox
    <br>
    <input type="file" value="文件上传">
    <br>
    <input type="hidden" value="上传隐藏字段">
    <br>
    <input type="image" value="图形按钮" src="输入图片地址">
    <br>
    <input type="password" value="密码">
    <br>
    <input type="radio"> 单选
    <br>
    <input type="reset" value="重置按钮">
    <br>
    <input type="submit" value="提交表单">
</form>
```
![](https://img2018.cnblogs.com/blog/958602/201903/958602-20190311143212038-1714709688.png)
﻿
﻿
H5新增控件：
- color 　　　　　　    定义调色板
- tel	　　　　  　　	 定义包含电话号码的输入域
- email	　　　  　　定义包含email地址的输入域
- url	　　　　  　　	 定义包含URL地址的输入域 
- search	　　　　　  定义搜索域
- number	　　	　　	 定义包含数值的输入域
- range	　　   　　  定义包含一定范围内数字值的输入域
- date	　　　　　  定义选取日、月、年的输入域 
- month	　　 	　　 	 定义选取月、年的输入域
- week	　　　　　  定义选取周、年的输入域
- time	　　    　　 定义选取月、年的输入域
- datetime	　　　        定义选取时间、日 月、年的输入域(UTC时间)
- datatime-local	　      定义选取时间、日 月、年的输入域(本地时间)
﻿
H5新增的控件在使用前可以去can i ues中查看下兼容问题
﻿
﻿
## input元素的常用属性
﻿
传统属性：accept、alt、checked、disabled、maxlength、name、readonly、size、src、type、value
﻿
H5新增属性：autocomplete、autofocus、form、formaction、formenctype、formmethod、formnovalidate、formtarget、height、list、max、min、multiple、novalidate、pattern、placeholder、required、step、width
﻿
【name】
name属性用于规定input元素的名称，用于对提交到服务器后的表单数据进行标识，或者在客户端通过javascript引用表单数据
﻿
[注意]只有设置了name属性的表单元素才能在提交表单时传递它们的值
﻿
【type】
type属性用来规定input元素的类型
﻿
【accept】
accept属性用来规定能够通过文件上传进行提交的文件类型。理论上可以用来限制上传文件类型，然而它只是建设性的，并很可能被忽略，它接受逗号分隔的MIME类型
﻿
[注意]该属性只能与type="file"配合使用
﻿
```
<input type="file" accept="image/gif,image/jpeg,image/jpg">
```
﻿
【alt】
alt属性为图像输入规定替代文本，功能类似于image元素的alt属性，为用户由于某些原因无法查看图像时提供备选信息
﻿
[注意]alt属性只能与type="image"的input元素配合使用
﻿
【checked】
checked属性规定在页面加载时应该被预先选定的input元素，也可以在页面加载后，通过javascript进行设置
﻿
[注意]checked属性只能与type="radio"或type="checkbox"的input元素配合使用
﻿
【disabled】
disabled属性规定应该禁用input元素。被禁用的字段是不能修改的，也不可以使用tab按键切换到该字段，但可以选中或拷贝其文本
﻿
[注意1]disabled属性无法与type="hidden"的input元素一起使用
[注意2]对于IE7-浏览器必须设置为disabled="disabled"，而不可以直接设置disabled，否则使用javascript控制时将失效
﻿
【readonly】
readonly属性规定输入字段为只读。只读字段是不能修改的，但用户仍然可以使用tab按键切换到该字段，还可以选中或拷贝其文本
readonly属性可与type="text"或"password"的input元素配合使用
﻿
[注意]IE7-浏览器不支持使用javascript控制readonly属性
﻿
【maxlength】
maxlength属性规定输入字段的最大长度，以字符个数计
﻿
[注意]该属性只能与type="text"或type="password"的input元素配合使用
﻿
```
<input maxlength="6">
<input type="password" maxlength="6">
```
﻿
﻿
【size】
size属性对于type="text"或"password"的input元素是可见的字符数；而对于其他类型，是以像素为单位的输入字段宽度
﻿
[注意]由于size属性是一个可视化的设计属性，推荐使用CSS来代替它
﻿
```
<input size="1">
<input type="password" size="2">
```
﻿
﻿
【src】
src属性作为提交按钮显示的图像的URL
﻿
[注意]src属性只能且必须与type="image"的input元素配合使用
﻿
```
<form action="#">
    <input name="test">
    <input type="image" src="http://sandbox.runjs.cn/uploads/rs/26/ddzmgynp/submit.jpg" width="99" height="99" alt="测试图片">
</form>
```
﻿
【value】
value属性为input元素设定值。对于不同的输入类型，value属性的用法也不同：
﻿
type="button"、"reset"、"submit"用于定义按钮上的显示的文本
﻿
type="text"、"password"、"hidden"用于定义输入字段的初始值
﻿
type="checkbox"、"radio"、"image"用于定义与输入相关联的值
﻿
[注意1]type="checkbox"或"radio"必须设置value属性
﻿
[注意2]value属性无法与type="file"的input元素一起使用
﻿
---------------------------------------------------------
﻿
### H5新增属性
﻿
【autocomplete】
autocomplete属性可以在个别元素或整个表单上开启或关闭浏览器的自动完成功能。当用户在字段开始键入时，浏览器基于之前键入过的值，显示出在字段中填写的选项
﻿
autocomplete属性适用form元素以及以下类型的input元素：text、search、url、telephone、email、password、date pickers、range、color
﻿
[注意]IE浏览器不支持该属性，只有元素拥有name属性，该属性才有效
﻿
【autofocus】
autofocus属性规定在页面加载时，域自动地获得焦点
﻿
autofous属性适用于button、input、keygen、select和textarea元素
﻿
```
<input name="test1">
<input name="test2" autofocus>
```
﻿
﻿
【novalidate】
novalidate属性规定在提交表单时不验证form或input域
﻿
novalidate属性适用于form元素以及以下类型的input元素：text、search、url、telephone、email、password、date pickers、range、color
﻿
[注意]IE9-浏览器不支持
﻿
【height】【width】
用于规定image类型的input标签的图像高度和宽度(不常用)
﻿
【list】
大多数输入类型包含一个属性list，它和一个新元素datalist结合使用，这个元素定义当在表单控件输入数据时可用的一个选项列表。datalist元素自身不会在页面显示，而是为其他元素的list属性提供数据
﻿
list属性适用于form元素以及以下类型的input元素：text、search、url、telephone、email、password、date pickers、range、color
﻿
[注意]IE9-浏览器及safari浏览器不支持
﻿
```
<form action="#">
    <input list="list" name="input">
    <datalist id="list">
        <option value="1">
        <option value="2">
        <option value="3">
    </datalist>
</form>
```
﻿
【min】【max】
规定输入域所允许的最小值，最大值
﻿
【step】
step属性为输入域规定合法的数字间隔
﻿
min、max、step属性适用于以下类型的input元素:date pickers、number、range
﻿
```
<input type="number" min="0" max="10" step="0.5" value="6" />
<input type="range" min="0" max="10" step="0.5" value="6" />
```
﻿
【multiple】
multiple属性规定按住ctrl按键，输入字段可以选择多个值
﻿
该属性适用于type="email"和"file"的input元素
﻿
[注意]该属性IE9-浏览器不支持
﻿
```
<button id="btn1">打开文件多选</button>
<button id="btn2">关闭文件多选</button>
<br><br>
<input id="test" type="file" multiple>
<script>
btn1.onclick = function(){
    test.setAttribute('multiple','');
};
btn2.onclick = function(){
    test.removeAttribute('multiple');
};
</script>
```
﻿
【pattern】
pattern属性规定用于验证input域的模式。模型pattern是正则表达式
﻿
pattern属性适用于以下类型的input元素：text、search、url、tel、email、password
﻿
[注意]IE9-浏览器及safari浏览器不支持
﻿
```
<form action="#">
    <input pattern="\d{3}">
    <input type="submit">
</form>
```
﻿
【placeholder】
placeholder属性提供占位符文字，描述输入域所期待的值。占位符会在输入域为空时显示出现，在输入域获得焦点时消失
﻿
placeholder属性适用于以下类型的input元素:text、search、url、tel、email、password
﻿
[注意]IE9-浏览器不支持
﻿
```
<form action="#">
    <input type="tel" placeholder="请输入数字" pattern="\d{11}">    
    <input type="submit">
</form>
```
﻿
要修改placeholder的颜色需要使用::placeholder
```
::placeholder{color:green;}
```
﻿
【required】
required属性规定必须在提交之前填写输入域(不能为空)
﻿
required属性适用于以下类型的input元素：text、search、url、telephone、email、password、date pickers、number、checkbox、radio、file
﻿
[注意]IE9-浏览器及safari浏览器不支持
﻿
```
<form action="#">
    <input required>    
    <input type="submit">
</form>
```
﻿
【form】
form属性规定输入域所属的一个或多个表单，form属性必须和所属表单的id
﻿
form属性适用于所有input标签的类型，若需要引用一个以上的表单时，用空格分隔
﻿
[注意]IE浏览器不支持该属性，只有元素拥有name属性，该属性才有效
﻿
```
<form id="form" action="#">
    <input type="submit">
</form>
<input name="test" form="form">
```
﻿
### 表单重写属性
> 表单重写属性允许重写form元素的某些属性设定。其中，formnovalidate适用于button或input元素，而其他属性适用于submit或reset的button或input元素
﻿
【formaction】
重写表单的action属性
﻿
```
<form action="#" >
First name: <input type="text" name="fname" /><br />
Last name: <input type="text" name="lname" /><br />
<input type="submit" value="提交" /><br />
<input type="submit" formaction="#" value="以管理员身份提交" />
</form>
```
﻿
【formenctype】
重写表单的enctype属性
﻿
```
<form action="#" method="post">
  First name: <input type="text" name="fname" /><br />
  <input type="submit" value="提交" />
  <input type="submit" formenctype="multipart/form-data" value="以multipart/form-data编码提交" />
</form>
```
﻿
【formmethod】
重写表单的method属性
```
<form action="#" method="get">
  First name: <input type="text" name="fname" /><br />
  Last name: <input type="text" name="lname" /><br />
<input type="submit" value="提交" />
<input type="submit" formmethod="post" formaction="#" value="使用POST提交" />
</form>
```
﻿
【formnovalidate】
重写表单的novalidate属性
﻿
```
<form action="#" method="get">
E-mail: <input type="email" name="userid" /><br />
<input type="submit" value="提交" /><br />
<input type="submit" formnovalidate="formnovalidate" value="进行没有验证的提交" />
</form>
```
﻿
﻿
【formtarget】
重写表单的target属性
﻿
```
<form action="#">
  First name: <input type="text" name="fname" /><br />
  Last name: <input type="text" name="lname" /><br />
<input type="submit" value="提交" />
<input type="submit" formtarget="_blank" value="提交到新窗口/选项卡" />
</form> 
```
﻿
## 小结
通过本文我们学习了form元素及其常用属性，input常用的type类型和相关属性。