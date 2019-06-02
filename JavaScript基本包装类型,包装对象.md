## 前言
javascript对象是一种复合值，它是属性或已命名值的集合。通过'.'符号来引用属性值。当属性值是一个函数时，称其为方法。通过o.m()来调用对象o中的方法。我们发现，字符串也同样具有属性和方法

```
var s = 'hello world';
console.log(s.length);//11
```
字符串既然不是对象，为什么它会有属性呢？这就引出了今天介绍的内容——包装对象

## 定义
在javascript中，“一切皆对象”，就连三种原始类型的值(数值、字符串、布尔值)，在一定条件下，也会自动转为对象，也就是原始类型的“包装对象”

包装对象是特殊的引用类型。每当读取字符串、数字或布尔值的属性或方法时，创建的临时对象称做包装对象

为了便于引用字符串的属性和方法，javascript将字符串值通过调用new String()的方式转换成对象，这个对象继承了字符串的属性和方法，并被用来处理属性和方法的引用。一旦属性或方法引用结束，这个新创建的对象就会销毁。数字或布尔值也类似

实际的过程可以理解为下面：
```
var s1 = 'some text';
var s2 = s1.substring(2);

//上述过程看起来发生了三个步骤
var s1 = new String('some text'); //(1)创建String类型的一个实例  
var s2 = s1.substring(2); //(2)在实例上调用指定的方法
s1 = null; //(3)销毁这个实例
```

## 生存期
引用类型和基本包装类型的主要区别是对象的生存期。使用new操作符创建的引用类型的实例，在执行流离开当前作用域之前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁。这意味着不能在运行时为基本类型值添加属性和方法

```
var s1 = 'some text';
s1.color = 'red';
alert(s1.color);//undefined
```

```
var s2 = new Boolean('some text');
s2.color = 'red';
alert(s2.color);//'red'
```

## 显式创建
可以通过new操作符显式创建包装对象，但应该在绝对必要的情况下再这样做。因为这种做法，很容易让人分不清是在处理基本类型还是引用类型的值

有两种方式显式创建包装类型的方式:
```
// Object方式
var s = new Object('abc');
var b = new Object(true);
var n = new Object(123);

// 构造函数方式
var s = new String('abc');
var b = new Boolean(true);
var n = new Number(123);
```

## 转型函数
直接调用转型函数与使用new调用基本包装类型的构造函数是不一样的，使用转型函数返回的是基本类型值

```
var s = 'abc'; // typeof s === string
var s1 = String(s); // typeof s1 === string
var s2 = new String(s); // typeof s2 === object
var s3 = new Object(s); // typeof s3 === object
```

## 比较运算
【1】等于运算符'=='将原始值和其包装对象视为相等，因为其中一个操作数是对象，需要调用对象的valueOf()方法，Number对象、Boolean对象和String对象的valueOf()方法都返回其对应的原始值

```
var s1 = new String('abc');
var s2 = 'abc';
var n1 = new Number(123);
var n2 = 123;
var b1 = new Boolean(true);
var b2 = true;
console.log(s1 == s2, n1 == n2, b1 == b2);//true true true
```

【2】全等运算符'==='将原始值和其包装对象视为不相等。因为全等运算符要比较类型和值，原始值和其包装对象的类型不同
```
var s1 = new String('abc');
var s2 = 'abc';
var n1 = new Number(123);
var n2 = 123;
var b1 = new Boolean(true);
var b2 = true;
console.log(s1 === s2, n1 === n2, b1 === b2);//false false false
```

## 小结
通过本文，我们学习了什么是包装类型，包装类型就是原始类型的对象表示，这样使得原始类型也可以有自己的属性和方法了。

包装对象和原始值的区别基本上就是“基本类型”和“引用类型”的区别了。譬如：能否定义属性和方法，typeof 值不一样。

使用String()等转型函数和使用new String() 是不一样的。前者返回基本类型，后者返回对象类型。
