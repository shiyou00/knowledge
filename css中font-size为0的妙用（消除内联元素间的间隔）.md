## 前言
```
<div>
    <input type="text">
    <input type="button" value="提交">
</div>
```
![](./image/41.png)

看不片我们不难发现为什么会多出一个间隙出来呢。我们该如何消除呢？

## inline && inline-block元素间隙
> 元素间留白间距出现的原因就是标签段之间的空格

因此去除的方法之一就是把标签的间隙去除掉
```
<div>
    <input type="text"><input type="button" value="提交">
</div>
```
![](./image/42.png)

果然就没有间隙了，但是这样代码的可读性太差了。  
解决方法还有一些，例如：margin负值 , letter-spacing, word-spacing,浮动等但是最佳的解决方案还是font-size:0

## font-size = 0
```
<div style="font-size: 0;">
    <input type="text">
    <input type="button" value="提交">
</div>
```

![](./image/43.png)

## 小结
之所以会想着把这个问题记录下来也是，当碰到一些精细的设计图时，距离的调整很重要。
