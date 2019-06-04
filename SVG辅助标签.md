## 超链接
在SVG中，可以使用超链接`<a>`。超链接可以添加到任意的图形上，类比于热区`<area>`

SVG中的超链接有如下3个常用属性
```
xlink:href 指定连接地址
xlink:title 指定连接标题
target 指定打开方式
```
```
<svg version="1.1" width="300" height="70" xmlns="http://www.w3.org/2000/svg">
  <a xlink:href="http://baidu.com">
    <rect x="10" y="10" width="30" height="30" fill="transparent" stroke="black"/>
  </a>
  <a xlink:href="http://cnblogs.com">
    <circle cx="80" cy="30" r="20" fill="transparent" stroke="black"/>
  </a>
</svg>
```

## 光栅图像
SVG有一个image元素，可以利用它嵌入任意光栅（以及矢量）图像。它的规格要求应用至少支持PNG、JPG和SVG格式文件

　　嵌入的图像变成一个普通的SVG元素。这意味着，可以在其内容上用剪切、遮罩、滤镜、旋转等操作
```
<svg version="1.1"
     xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
     width="200" height="200">
  <image x="90" y="-65" width="128" height="146" transform="rotate(45)"
     xlink:href="https://developer.mozilla.org/media/img/mdn-logo.png"/>
</svg>
```
![](./image/162.png)

## 辅助标签
SVG中有四个常用的辅助标签：`<g>、<use>、<symbol>、<defs>`

### `<g>`
　　`<g>`是一个分组(容器)标签，用来组合元素。可以设置一组元素的属性

　　由于`<g>`是一个通用的分组标签，可以包含任何形状，因此其只能设置所有形状都有的属性，包括stroke、fill等。由于cx、cy是圆特有的属性，所以无法在`<g>`上设置

　　下面是一个制作同心圆的小实例
```
<svg version="1.1" height="100" xmlns="http://www.w3.org/2000/svg">
  <circle cx="50" cy="50" r="40" fill="transparent" stroke="black" stroke-width="5"></circle>
  <circle cx="50" cy="50" r="30" fill="transparent" stroke="black" stroke-width="5"></circle>
  <circle cx="50" cy="50" r="20" fill="transparent" stroke="black" stroke-width="5"></circle>
  <circle cx="50" cy="50" r="10" fill="transparent" stroke="black" stroke-width="5"></circle>
</svg>
```

![](./image/163.png)

　　如果要改变所有圆的样式时，就要一个一个进行修改，稍显麻烦。这时，使用分组标签`<g>`就非常合适。如果想通过`<g>`标签来平移所有`<circle>`元素的位置，可以使用transform()来实现

```
<svg version="1.1" xmlns="http://www.w3.org/2000/svg">
  <g transform="translate(60,60)" fill="transparent" stroke="red" stroke-width="5">
    <circle r="40"></circle>
    <circle r="30"></circle>
    <circle r="20"></circle>
    <circle r="10"></circle>    
  </g>
</svg>
```
![](./image/164.png)

### defs
SVG允许定义以后需要重复使用的图形元素。 建议把所有需要再次使用的引用元素定义在defs元素里面。这样做可以增加SVG内容的易读性和可访问性。 在defs元素中定义的图形元素不会直接呈现

```
<svg version="1.1" height="40" xmlns="http://www.w3.org/2000/svg">
	<defs>
	  <linearGradient id="Gradient01">
		<stop offset="20%" stop-color="#39F" />
		<stop offset="90%" stop-color="#F3F" />
	  </linearGradient>
	</defs>
  	<rect x="10" y="10" width="60" height="30" fill="url(#Gradient01)"  />
</svg>
```
![](./image/165.png)

### use 
use元素在SVG文档内取得目标节点，并在别的地方复制它们。它的效果等同于这些节点被深克隆到一个不可见的DOM中，然后将其粘贴到use元素的位置

　　因为克隆的节点是不可见的，所以当使用CSS样式化一个use元素以及它的隐藏的后代元素的时候，隐藏的、克隆的DOM不能保证继承CSS属性，除非明文设置使用CSS继承。

　　出于安全原因，一些浏览器可能在use元素上应用同源策略，还有可能拒绝载入`xlink:href`属性内的跨源URI
  
```
<svg version="1.1" height="70" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <circle  id="Port" r="10"/>
    </defs>
    <text y="15">black</text>
    <use x="50" y="10" xlink:href="#Port" />
    <text y="35">red</text>
    <use x="50" y="30" xlink:href="#Port" fill="red"/>
    <text y="55">blue</text>
    <use x="50" y="50" xlink:href="#Port" fill="blue"/>
</svg>
```
![](./image/166.png)

### symbol 
　　symbol元素用来定义一个图形模板对象，它可以用一个`<use>`元素实例化。symbol元素对图形的作用是在同一文档中多次使用，添加结构和语义，从而提升了可访问性

　　一个symbol元素本身是不呈现的。只有symbol元素的实例（亦即，一个引用了symbol的 `<use>`元素）才能呈现

　　一个`<symbol>`元素可以有preserveAspectRatio和viewBox属性。而`<g>`元素不能拥有这些属性。因此相比于在`<defs>`元素中使用`<g>`的方式来复用图形，使用`<symbol>`元素也许是一个更好的选择
```
<svg version="1.1"  xmlns="http://www.w3.org/2000/svg">
    <symbol id="sym01" viewBox="0 0 150 110">
        <circle cx="50" cy="50" r="40" stroke-width="8" stroke="red" fill="red"/>
        <circle cx="90" cy="60" r="40" stroke-width="8" stroke="green" fill="white"/>
    </symbol>
    <use xlink:href="#sym01"
         x="0" y="0" width="100" height="50"/>
    <use xlink:href="#sym01"
         x="0" y="50" width="75" height="38"/>
    <use xlink:href="#sym01"
         x="0" y="100" width="50" height="25"/>
</svg>
```
![](./image/167.png)
