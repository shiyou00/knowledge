## 前言
underscore 源码没有出现 undefined（注意，其实有出现一处，是为 "undefined"，而不是 undefined），而用 void 0 代替之。为什么要这么做？

## 为什么不直接用undefined
> undefined在JavaScript中并不属于保留字/关键字，因此在IE5.5~8中我们可以将其当作变量那样对其赋值（IE9+及其他现代浏览器中赋值给undefined将无效）

```
var undefinedBackup = undefined;
undefined = 1;
// 显示"undefined"
console.log(typeof undefinedBackup);
// 在IE5.5~8中显示"number"，其他浏览器中则显示"undefined"
console.log(typeof undefined);
```
于是采用void方式获取undefined则成了通用准则。

## 一元运算符void的作用
void的行为特点为：

1. 不管void后的运算数是什么，只管返回纯正的undefined；  
2. void会对其后的运算数作取值操作，因此若属性有个getter函数，那么就会调用getter函数（因此会产生副作用）

```
var article = {
    _view: 0,
    get view(){
        console.log(this._view);
    	return this._view++;
    }
};
var test = void article.view; // 显示0
console.log(test); // 显示undefined
console.log(article._view); // 显示1
```

## 还有啥方式可以得到纯正的undefined？

除了通过一元运算符void获取纯正的undefined，其实我们还有如下方法来获取：

1. 未赋值的变量
```
var myUndefined;
console.log(typeof myUndefined); // 显示"undefined"
```

2. 无返回值函数
```
var getUndefined = function(){};
```

3. 未定义的属性

```
var myUndefined1 = {}[''];
var myUndefined2 = [][0];
```
