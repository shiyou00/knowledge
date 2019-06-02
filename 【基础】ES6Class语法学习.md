## 前言
大多数面向对象的编程语言都支持类和类继承的特性，而JS却不支持这些特性，只能通过其他方法定义并关联多个相似的对象，这种状态一直延续到了ES5。由于类似的库层出不穷，最终还是在ECMAScript 6中引入了类的特性。本文将详细介绍ES6中的类

## 类的定义

【ES5的类】
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

【ES6的类】
```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```
<b class="danger">[注意]this关键字则代表实例对象</b>

<b class="danger">[注意]类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式。</b>

<b class="danger">[注意]类不存在变量提升，这点与ES5完全不同</b>

```
typeof Point  // 'function'
Point === Point.prototype.constructor // true
```
类的数据类型就是函数，类本身就指向构造函数。

## 属性的另一种定义方式
```
class IncreasingCounter {

  _count = 0; // 可以直接这样定义，而不定义在constructor中

  increment() {
    this._count++;
  }
}
```
<b class="pri">这种写法比较简洁，比较推荐的写法</b>

## 是否可枚举
ES5中类的方法可以枚举
```
var Point = function (x, y) {
  // ...
};

Point.prototype.toString = function() {
  // ...
};

Object.keys(Point.prototype)
// ["toString"]
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

ES6中类的方法不可枚举
```
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```


## constructor构造函数
1、constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。  
2、一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。


## 类的调用
通过 new 关键字调用，如果忘记加 new 则会报错
```
const point = new Point();
```

## 类的属性表达式
```
let methodName = 'getArea';

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

## 立即执行类

```
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```

## 静态方法
> 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```
<b class="danger">[注意]如果静态方法包含this关键字，这个this指的是类，而不是实例。</b>

<b class="pri">[注意]静态方法可继承</b>

## 静态属性
> 静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

```
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

## new.target 属性
> new是从构造函数生成实例对象的命令。ES6 为new命令引入了一个new.target属性，该属性一般用在构造函数之中，返回new命令作用于的那个构造函数。如果构造函数不是通过new命令或Reflect.construct()调用的，new.target会返回undefined，因此这个属性可以用来确定构造函数是怎么调用的。

```
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

Class 内部调用new.target，返回当前 Class。
```
class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    this.length = length;
    this.width = width;
  }
}

var obj = new Rectangle(3, 4); // 输出 true
```

【new.target子类继承会返回子类的名称】
> 利用这个特点，可以写出不能独立使用、必须继承后才能使用的类。

```
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

var x = new Shape();  // 报错
var y = new Rectangle(3, 4);  // 正确
```

## extends 继承

```
class Point {
}

class ColorPoint extends Point {
}
```

【父类的静态方法，也会被子类继承】
```
class A {
  static hello() {
    console.log('hello world');
  }
}

class B extends A {
}

B.hello()  // hello world
```

## super 关键字
1、super作为函数调用时，代表父类的构造函数。  
2、super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。  
3、在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例。  
4、super在静态方法之中指向父类，在普通方法之中指向父类的原型对象。  

【1】
```
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
<b class="danger">[注意]子类必须在constructor方法中调用super方法，否则新建实例时会报错。</b>


【2】
```
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```
子类B当中的super.p()，就是将super当作一个对象使用。这时，super在普通方法之中，指向A.prototype，所以super.p()就相当于A.prototype.p()。

【3】
```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

【4】
```
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2
```

## Object.getPrototypeOf()
> Object.getPrototypeOf方法可以用来从子类上获取父类。

```
Object.getPrototypeOf(ColorPoint) === Point // true
```

## 类的 prototype 属性和__proto__属性
大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。

（1）子类的__proto__属性，表示构造函数的继承，总是指向父类。

（2）子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

```
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

【非继承时__proto , prototype】
```
class A {
}

A.__proto__ === Function.prototype // true
A.prototype.__proto__ === Object.prototype // true
```
这种情况下，A作为一个基类（即不存在任何继承），就是一个普通函数，所以直接继承Function.prototype。但是，A调用后返回一个空对象（即Object实例），所以A.prototype.__proto__指向构造函数（Object）的prototype属性。
