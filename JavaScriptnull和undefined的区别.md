## 前言
1995年javascript诞生时，最初像Java一样，只设置了null作为表示”无”的值。根据C语言的传统，null被设计成可以自动转为0

但是，javascript的设计者Brendan Eich，觉得这样做还不够，有两个原因。首先，null像在Java里一样，被当成一个对象。但是，javascript的值分成原始类型和对象类型两大类，Brendan Eich觉得表示”无”的值最好不是对象。其次，javascript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误

因此，Brendan Eich又设计了一个undefined。他是这样区分的：null是一个表示”无”的对象，转为数值时为0；undefined是一个表示”无”的原始值，转为数值时为NaN

但是，目前null和undefined基本是同义的，都是原始类型，且只有一些细微的差别

## Undefined
Undefined类型只有一个值，即特殊的undefined。在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined，例如：
```
var msg;
msg === undefined // true

等同于：
var msg = undefined; // 使用undefined值显式初始化了变量message。但我们没有必要这么做，因为未经初始化的值默认就会取得undefined值。
```

【undefined 出现场景】
```
1、已声明未赋值的变量
var i ;// undefined

2、获取对象不存在的属性
var o = {};
o.i // undefined

3、无返回值的函数的执行结果
function f(){}
f() // undefined

4、函数的参数没有传入
function f(x){return x;}
f() // undefined

5、void(expression)
void(0) // undefined
```

【类型转换】
```
Boolean(undefined):　 false
Number(undefined):　  NaN
String(undefined):　　'undefined'  
```

【类型鉴别】  
 鉴别undefined类型，使用typeof运算符即可
```
typeof undefined ; // undefined
```

[注意]由于undefined并不是一个关键字，其在IE8-浏览器中会被重写，在高版本函数作用域中也会被重写；所以可以用void 0 来替换undefined
```
var undefined = 10;
console.log(undefined);//IE8-浏览器下为10，高版本浏览器下为undefined

function t(){
    var undefined = 10;
    console.log(undefined);
}
console.log(t());//所有浏览器下都是10
```

## Null
Null类型是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示一个空对象指针，而这也正是使用typeof操作符检测null值时会返回"object"的原因，如下面的例子所示：
```
var car = null;
alert(typeof car);     // "object"
// 不同的对象在底层都表示为二进制，在javascript中二进制前三位都为0会被判断为object类型，null的二进制表示是全0，所以执行typeof时返回'object'
```

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值。这样一来，只要直接检查null值就可以知道相应的变量是否已经保存了一个对象的引用，如下面的例子所示：

```
if (car != null){
    // 对car对象执行某些操作
}
```

实际上，undefined值是派生自null值的，因此ECMA-262规定对它们的相等性测试要返回true：
```
null == undefined // true
```

【类型转换】
```
Boolean(null): 　　false
Number(null):　　  0
String(null): 　　 'null'
```

【类型鉴别】  
鉴别null类型，使用typeof运算符不可行，因为该运算符会返回'object'，null被认为是空对象指针  
判断一个值是否为null类型的最佳方法是直接和null进行恒等比较

```
console.log(typeof null);//'object'
console.log(null === null);//true
console.log(undefined === null);//false
console.log('null' === null);//false
```

## undefined 和 null 的区别
最后总结下二者的区别：

1、null表示空对象指针而undefined表示未经初始化的变量  
2、数字类型转换时 Number(undefined) 是 NaN；而 Number(null) 是 0  
3、做类型判断的时候 undefined 使用typeof即可；而 null则不能使用typeof 而需要和自身进行比较 null === null
