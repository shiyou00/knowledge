## HTML 条件注释(hack常用)
> IE条件注释是微软从IE5开始就提供的一种非标准逻辑语句，作用是可以灵活的为不同IE版本浏览器导入不同html元素。很显然这种方法的最大好处就在于属于微软官方给出的兼容解决办法而且还能通过W3C的效验

【识别IE】
```
<!--[if IE]>
<div class="box" id="box">只在IE中会显示</div>
<![endif]-->
```

【识别IE具体版本】
```
6    [if IE 6]
7    [if IE 7]
8    [if IE 8]
9    [if IE 9]
```
```
<!--[if IE 7]>
<div class="box" id="box"></div> // 只在IE7中显示
<![endif]-->
```

【IE范围】
```
gt          大于(greater than)
gte     　大于等于(greater than or equal)
lt           小于(less than)
lte         小于等于(less than or equal)
```
```
<!--[if lte IE 7]>
<div class="box" id="box"></div> // 小于等于IE7识别
<![endif]-->
```

识别非IE
> 实际上识别的是IE10+浏览器和其他非IE浏览器
```
<!--[if !IE]>
<div class="box" id="box"></div>
<![endif]-->
```

**实际应用中我们常常用来判断IE版本来引入相应的JS进行hack，或者通过不同IE版本引入不同的jquery版本**