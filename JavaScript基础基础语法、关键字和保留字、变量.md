## 前言
任何语言的核心都必然会描述这门语言最基本的工作原理。而描述的内容通常都要涉及这门语言的语法、操作符、数据类型、内置功能等用于构建复杂解决方案的基本概念。如前所述，ECMA-262通过叫做ECMAScript的“伪语言”为我们描述了JavaScript的所有这些基本概念。

由于基础概念较多如果放在一篇文章会使得篇幅特别长，所以本文主要讲解：基础语法，关键字和保留字，变量等内容

## 语法
ECMAScript的语法大量借鉴了C及其他类C语言（如Java和Perl）的语法。因此，熟悉这些语言的开发人员在接受ECMAScript更加宽松的语法时，一定会有一种轻松自在的感觉。

【区分大小写】  
要理解的第一个概念就是ECMAScript中的一切（变量、函数名和操作符）都区分大小写。
```
var test = 1;
var Test = 2;
这是完全不同的两个变量
```

【标识符】  
所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。

标识符规则
- 第一个字符必须是一个字母、下划线（_）或一个美元符号（$）；
- 其他字符可以是字母、下划线、美元符号或数字。
- 一般采用驼峰格式，即第一个字母小写，剩下的每个单词的首字母大写

**[注意]标识符中的字母也可以包含扩展的ASCII或Unicode字母字符（如À和Æ），但我们不推荐这样做。**

正确的标识符
```
var _a = 1;
var $a = 1;
var a = 1;
firstSecond
myCar
doSomethingImportant
```

错误的标识符
```
var 1bar = 1;// 报错：Uncaught SyntaxError: Unexpected number
var %11aa = 2; // 报错：Uncaught SyntaxError: Unexpected token %
```

[注意]不能把关键字、保留字、true、false和null用作标识符。

【注释】
```
// 单行注释

/*
 *  这是一个多行
 *  （块级）注释
 */
```

【语句】  
ECMAScript中的语句以一个分号结尾；如果省略分号，则由解析器确定语句的结尾，如下例所示：
```
var sum = a + b                 // 即使没有分号也是有效的语句——不推荐
var diff = a - b;               // 有效的语句——推荐
```
虽然语句结尾的分号不是必需的，但我们建议任何时候都不要省略它。因为加上这个分号可以避免很多错误（例如不完整的输入），开发人员也可以放心地通过删除多余的空格来压缩ECMAScript代码（代码行结尾处没有分号会导致压缩错误）。另外，加上分号也会在某些情况下增进代码的性能，因为这样解析器就不必再花时间推测应该在哪里插入分号了。

可以使用C风格的语法把多条语句组合到一个代码块中，即代码块以左花括号（{）开头，以右花括号（}）结尾：
```
if (test)
    alert(test);       // 有效但容易出错，不要使用

if (test){             // 推荐使用
    alert(test);
}
```
在控制语句中使用代码块可以让编码意图更加清晰，而且也能降低修改代码时出错的几率。

## 关键字和保留字
ECMA-262描述了一组具有特定用途的关键字，这些关键字可用于表示控制语句的开始或结束，或者用于执行特定操作等。按照规则，关键字也是语言保留的，不能用作标识符。以下就是ECMAScript的全部关键字（带*号上标的是第5版新增的关键字）：

```
break         do           instanceof        typeof
case          else         new               var
catch         finally      return            void
continue      for          switch            while
debugger*     function     this              with
default       if           throw
delete        in           try
```

ECMA-262还描述了另外一组不能用作标识符的保留字。尽管保留字在这门语言中还没有任何特定的用途，但它们有可能在将来被用作关键字。以下是ECMA-262第3版定义的全部保留字：
```
abstract      enum            int            short
boolean       export          interface      static
byte          extends         long           super
char          final           native         synchronized
class         float           package        throws
const         goto            private        transient
debugger      implements      protected      volatile
double        import          public
```

第5版把在非严格模式下运行时的保留字缩减为下列这些：
```
class          enum          extends         super
const          export        import
```

在严格模式下，第5版还对以下保留字施加了限制：
```
implements     package       public
interface      private       static
let            protected     yield
```

注意，let和yield是第5版新增的保留字；其他保留字都是第3版定义的。为了最大程度地保证兼容性，建议读者将第3版定义的保留字外加let和yield作为编程时的参考。

除了上面列出的保留字和关键字，ECMA-262第5版对eval和arguments还施加了限制。在严格模式下，这两个名字也不能作为标识符或属性名，否则会抛出错误。

## 变量
ECMAScript的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。

```
var message ;
let message ;
```
这行代码定义了一个名为message的变量，该变量可以用来保存任何值（像这样未经过初始化的变量，会保存一个特殊的值——undefined）

有一点必须注意，即用var操作符定义的变量将成为定义该变量的作用域中的局部变量。
```
function test(){
    var message = "hi"; // 局部变量
}
test();
alert(message); // 错误！
```

不过，可以像下面这样省略var操作符，从而创建一个全局变量：
```
function test(){
    message = "hi"; // 全局变量
}
test();
alert(message); // "hi"
```
**[注意]非常不推荐这么做**

【一条语句定义多个变量】
```
var message = "hi",
    found = false,
    age = 29;
```

在严格模式下，不能定义名为eval或arguments的变量，否则会导致语法错误。

### 声明提升(hoisting)
块级作用域  
　　块级作用域是指花括号内的每一段代码都具有各自的作用域，而javascript没有块级作用域。javascript只有函数作用域：变量在声明它们的函数体以及这个函数体嵌套的任意函数体内都是有定义的

　　这意味着，变量在声明之前甚至已经可用。javascript这个特性被非正式地称为声明提升(hoisting)，javascript函数里声明的所有变量(不涉及赋值)都被提前到函数体的顶部

　　[注意]其实除了变量提升，函数也被提升，到函数部分会有详细介绍
  
```
var scope = 'global';
function f(){
    console.log(scope);//undefined
    var scope = 'local';
    console.log(scope);//'local'
}

//变量声明提升之后，相当于下面代码
var scope = 'global';
function f(){
    var scope;
    console.log(scope);//undefined
    scope = 'local';
    console.log(scope);//'local'
}
```
javascript中没有块级作用域，所以一些程序员特意将变量声明放在函数体顶部，这种源代码非常清晰地反映了真实的变量作用域

### 属性变量
　　当声明一个javascript全局变量时，实际上是定义了全局对象window的一个属性
　　当使用var声明一个变量时，创建的这个变量是不可配置的，也就是说这个变量无法通过delete运算符删除
  
```
var truevar = 1;
console.log(truevar,window.truevar);//1 1
delete truevar;//false
console.log(truevar,window.truevar);//1 1
```

　　如果没有使用严格模式并给一个未声明的变量赋值的话，javascript会自动创建一个全局变量，以这种方式创建的变量是全局对象的正常的可配置属性，并可以删除它们

　　[注意]IE8-浏览器下，如果删除window属性时，不论该属性是如何创建的，都会报错
  
```
window.fakevar1 = 10;
this.fakevar2 = 20;
var fakevar3 = 30;
fakevar4 = 40;

console.log(delete fakevar1);//IE8-浏览器报错，其他浏览器返回true
console.log(delete fakevar2);//IE8-浏览器报错，其他浏览器返回true
console.log(delete fakevar3);//所有浏览器都返回false
console.log(delete fakevar4);//所有浏览器都返回true
```

　　javascript全局变量是全局对象的属性，这是在ECMAScript中强制规定的。局部变量当做跟函数调用相关的某个对象的属性。ECMAScript3称为调用对象(call object)，ECMAScript5称为声明上下文对象(declarative environment record)。javascript允许使用this关键字来引用全局对象，却没有办法可以引用局部变量中存放的对象。这种存放局部变量对象的特有性质，是一种对我们不可见的内部实现
