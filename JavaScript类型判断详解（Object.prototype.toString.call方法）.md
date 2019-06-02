## 前言
在编写一些类库中，我们经常需要判断一些未知的用户的输入和配置，故而需要进行一系列的类型判断。故而总结下JS是如何进行类型判断的

## typeof
> typeof操作符返回一个字符串，表示未经计算的操作数的类型；该运算符数据类型（返回字符串，对应列表如图）

```
typeof undefined = undefined
typeof Null = object
typeof Boolean = boolean
typeof Number = number
typeof String = string
typeof Symbol = symbol
typeof 函数 = function
typeof 其它对象均返回object
```
至此我们可以确定：Null，数组之类的对象是没有办法通过typeof来确定的。

## instanceof
> instanceof运算符用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置

```
var str = "This is a simple string"; 
var num = 1111;
var boolean = true;
var und = undefined;
var nl = null;
var sb = Symbol('1111');
var obj = {}; // 非原始类型数据字面量定义

console.log(str instanceof String);         // false
console.log(num instanceof Number);         // false
console.log(boolean instanceof Boolean);    // false
console.log(nl instanceof Object);          // false
console.log(sb instanceof Symbol);          // false
console.log(obj instanceof Object);         // true

var strN = new String("This is a simple string");
var numN = new Number(1111);
var booleanN = new Boolean(true);
var objN = new Object();

console.log(strN instanceof String);            // true
console.log(numN instanceof Number);            // true
console.log(booleanN instanceof Boolean);       // true
console.log(objN instanceof Object);            // true
```
字面量产出的原始数据类型无法使用instanceof判断

上面两种方法都有着各自的缺陷，那么我们应该如何确切有效的进行类型判断呢？

下面介绍一个非常可靠的方法：

## Object.propotype.toString
> 返回一个表示该对象的字符串

```
Object.prototype.toString.call('string');       //"[object String]"
Object.prototype.toString.call(1111);           //"[object Number]"
Object.prototype.toString.call(true);           //"[object Boolean]"
Object.prototype.toString.call(null);           //"[object Null]"
Object.prototype.toString.call(undefined);      //"[object Undefined]"
Object.prototype.toString.call(Symbol('111'));  //"[object Symbol]"
Object.prototype.toString.call({});             //"[object Object]"
```

## 参考文献
- [MDN toString](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
