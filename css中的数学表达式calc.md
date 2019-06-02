## 前言
> 数学表达式calc()是CSS中的函数，主要用于数学运算。使用calc()为页面元素布局提供了便利和新的思路。

## 概念
> 数学表达式calc()是calculate计算的缩写，它允许使用+、-、*、/这四种运算符，可以混合使用%、px、em、rem等单位进行计算

[兼容性] IE8-、safari5.1-、ios5.1-、android4.3-不支持，android4.4-4.4.4只支持加法和减法。IE9不支持用于backround-position  

[注意]+和-运算符两边一定要有空白符(只有留了空白符后css才会判断出是进行运算)

```
<style>
.test1{
    border: calc( 1px + 1px ) solid black;
    /* calc里面的运算遵循*、/优先于+、-的顺序 */
    width: calc(100%/3 - 2*1em - 2*1px);
    background-color: pink;
    font-style: toggle(italic, normal); 
}
.test2{
    /* 由于运算符+的左右两侧没有空白符，所以失效 */
    border: calc(1px+1px) solid black;
    /* 对于，不能小于0的属性值，当运算结果小于0时，按0处理 */
    width: calc(10px - 20px);
    padding-left: 10px;
    background-color: lightblue;
}
</style>
```

```
<div class="test1">测试文字一</div>    
<div class="test2">测试文字二</div>
```
<style>
.test1{
    border: calc( 1px + 1px ) solid black;
    /* calc里面的运算遵循*、/优先于+、-的顺序 */
    width: calc(100%/3 - 2*1em - 2*1px);
    background-color: pink;
    font-style: toggle(italic, normal); 
}
.test2{
    /* 由于运算符+的左右两侧没有空白符，所以失效 */
    border: calc(1px+1px) solid black;
    /* 对于，不能小于0的属性值，当运算结果小于0时，按0处理 */
    width: calc(10px - 20px);
    padding-left: 10px;
    background-color: lightblue;
}
#vh_box{width:100%; height: 100%;}
#vh_t0{width:100%;height:200px; background:blue; }    
#vh_t1{width:100%; height:calc( 100vh - 200px ); background:red;}
</style>

<div class="test1">测试文字一</div>    
<div class="test2">测试文字二</div>

[重点] 数学表达式calc()常用于布局中的不同单位的数字运算
```
<style>
#vh_box{width:100%; height: 100%;}
.vh_t0{width:100%;height:200px; background:blue; }    
.right{width:100%; height:calc( 100vh - 200px ); background:red;} // 浏览器会计算出 100vh 减去 200px 的值。这样对于不同单位的运算是非常便利的。
</style>
<div id="vh_box">
    <div id="vh_t0"></div>
    <div id=""vh_t1></div>    
</div>
```

<div id="vh_box">
    <div id="vh_t0"></div>
    <div id="vh_t1"></div>    
</div>
