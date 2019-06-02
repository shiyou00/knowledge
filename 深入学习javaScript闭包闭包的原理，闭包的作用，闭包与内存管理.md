## 前言
虽然JavaScript是一门完整的面向对象的编程语言，但这门语言同时也拥有许多函数式语言的特性。

函数式语言的鼻祖是LISP，JavaScript在设计之初参考了LISP两大方言之一的Scheme，引入了Lambda表达式、闭包、高阶函数等特性。使用这些特性，我们经常可以用一些灵活而巧妙的方式来编写JavaScript代码。

## 闭包
> 闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数

一个最常见的闭包案例
```
var closure = function(){
    var count = 0;
    return function(){
        count ++;
    }
}
closure(); // 1
closure(); // 2
closure(); // 3
```

对于JavaScript程序员来说，闭包（closure）是一个难懂又必须征服的概念。闭包的形成与变量的作用域以及变量的生存周期密切相关。

### 变量的作用域
> 变量的作用域，就是指变量的有效范围。我们最常谈到的是在函数中声明的变量作用域。

当在函数中声明一个变量的时候，如果该变量前面没有带上关键字var，这个变量就会成为全局变量，这当然是一种容易造成命名冲突的做法。

另外一种情况是用var关键字在函数中声明变量，这时候的变量即是局部变量，只有在该函数内部才能访问到这个变量，在函数外面是访问不到的。代码如下：
```
var func = function(){
    var a = 1;
    alert ( a );     // 输出: 1
};

func();
alert ( a );     // 输出：Uncaught ReferenceError: a is not defined
```

[重点]在JavaScript中，函数可以用来创造函数作用域。此时的函数像一层半透明的玻璃，在函数里面可以看到外面的变量，而在函数外面则无法看到函数里面的变量。这是因为当在函数中搜索一个变量的时候，如果该函数内并没有声明这个变量，那么此次搜索的过程会随着代码执行环境创建的作用域链往外层逐层搜索，一直搜索到全局对象为止。变量的搜索是从内到外而非从外到内的。

### 变量的生存周期
除了变量的作用域之外，另外一个跟闭包有关的概念是变量的生存周期。

对于全局变量来说，全局变量的生存周期当然是永久的，除非我们主动销毁这个全局变量。

而对于在函数内用var关键字声明的局部变量来说，当退出函数时，这些局部变量即失去了它们的价值，它们都会随着函数调用的结束而被销毁：
```
var func = function(){
    var a = 1;      // 退出函数后局部变量a将被销毁
    alert ( a );
};

func();
```

看闭包的形式
```
var func = function(){
    var a = 1;
    return function(){
        a++;
        alert ( a );
    }
};

var f = func();

f();    // 输出：2
f();    // 输出：3
f();    // 输出：4
f();    // 输出：5
```
> 当退出函数后，局部变量a并没有消失，而是似乎一直在某个地方存活着。这是因为当执行var f = func();时，f返回了一个匿名函数的引用，它可以访问到func()被调用时产生的环境，而局部变量a一直处在这个环境里。既然局部变量所在的环境还能被外界访问，这个局部变量就有了不被销毁的理由。在这里产生了一个闭包结构，局部变量的生命看起来被延续了。

【闭包经典经典案例】
```
function createFunctions(){
    var result = new Array();

    for (var i=0; i < 10; i++){
        result[i] = function(){
            return i;
        };
    }

    return result;
} 
```
这个函数会返回一个函数数组。表面上看，似乎每个函数都应该返自己的索引值，即位置0的函数返回0，位置1的函数返回1，以此类推。但实际上，每个函数都返回10。因为每个函数的作用域链中都保存着createFunctions()函数的活动对象，所以它们引用的都是同一个变量i。当createFunctions()函数返回后，变量i的值是10，此时每个函数都引用着保存变量i的同一个变量对象，所以在每个函数内部i的值都是10。但是，我们可以通过创建另一个匿名函数强制让闭包的行为符合预期，如下所示。

```
function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){
        result[i] = function(num){
            return function(){
                return num;
            };
        }(i);
    }
    return result;
}
```
[重点]在重写了前面的createFunctions()函数后，每个函数就会返回各自不同的索引值了。在这个版本中，我们没有直接把闭包赋值给数组，而是定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组。这里的匿名函数有一个参数num，也就是最终的函数要返回的值。在调用每个匿名函数时，我们传入了变量i。由于函数参数是按值传递的，所以就会将变量i的当前值复制给参数num。而在这个匿名函数内部，又创建并返回了一个访问num的闭包。这样一来，result数组中的每个函数都有自己num变量的一个副本，因此就可以返回各自不同的数值了。


## 闭包的作用

【封装变量】  
闭包可以帮助把一些不需要暴露在全局的变量封装成“私有变量”。假设有一个计算乘积的简单函数：
```
var mult = function(){
    var a = 1;
    for ( var i = 0, l = arguments.length; i < l; i++ ){
        a = a * arguments[i];
    }
    return a;
};
```

mult函数接受一些number类型的参数，并返回这些参数的乘积。现在我们觉得对于那些相同的参数来说，每次都进行计算是一种浪费，我们可以加入缓存机制来提高这个函数的性能：
```
var cache = {};

var mult = function(){
    var args = Array.prototype.join.call( arguments, ',' ); // 通过call方法借用数组方法
    if ( cache[ args ] ){
        return cache[ args ];
    }

    var a = 1;
    for ( var i = 0, l = arguments.length; i < l; i++ ){
        a = a * arguments[i];
    }

    return cache[ args ] = a;
};

alert ( mult( 1,2,3 ) );     // 输出：6
alert ( mult( 1,2,3 ) );     // 输出：6

```

我们看到cache这个变量仅仅在mult函数中被使用，与其让cache变量跟mult函数一起平行地暴露在全局作用域下，不如把它封闭在mult函数内部，这样可以减少页面中的全局变量，以避免这个变量在其他地方被不小心修改而引发错误。代码如下：
```
var mult = (function(){
    var cache = {};
    return function(){
        var args = Array.prototype.join.call( arguments, ',' );
        if ( args in cache ){
            return cache[ args ];
        }
        var a = 1;
        for ( var i = 0, l = arguments.length; i < l; i++ ){
            a = a * arguments[i];
        }
        return cache[ args ] = a;
    }
})();
```

提炼函数是代码重构中的一种常见技巧。如果在一个大函数中有一些代码块能够独立出来，我们常常把这些代码块封装在独立的小函数里面。独立出来的小函数有助于代码复用，如果这些小函数有一个良好的命名，它们本身也起到了注释的作用。如果这些小函数不需要在程序的其他地方使用，最好是把它们用闭包封闭起来。代码如下：
```
var mult = (function(){
    var cache = {};
    var calculate = function(){   // 封闭calculate函数
        var a = 1;
        for ( var i = 0, l = arguments.length; i < l; i++ ){
            a = a * arguments[i];
        }
        return a;
    };

    return function(){
        var args = Array.prototype.join.call( arguments, ',' );
        if ( args in cache ){
            return cache[ args ];
        }

        return cache[ args ] = calculate.apply( null, arguments );
    }
})();
```

【延续局部变量的寿命】

img对象经常用于进行数据上报，如下所示：
```
var report = function( src ){
    var img = new Image();
    img.src = src;
};

report( 'http://xxx.com/getUserInfo' );
```

但是通过查询后台的记录我们得知，因为一些低版本浏览器的实现存在bug，在这些浏览器下使用report函数进行数据上报会丢失30%左右的数据，也就是说，report函数并不是每一次都成功发起了HTTP请求。丢失数据的原因是img是report函数中的局部变量，当report函数的调用结束后，img局部变量随即被销毁，而此时或许还没来得及发出HTTP请求，所以此次请求就会丢失掉。

现在我们把img变量用闭包封闭起来，便能解决请求丢失的问题：
```
 var report = (function(){
    var imgs = [];
    return function( src ){
        var img = new Image();
        imgs.push( img );
        img.src = src;
    }
})();
```

## 闭包和面向对象设计
过程与数据的结合是形容面向对象中的“对象”时经常使用的表达。

对象以方法的形式包含了过程，而闭包则是在过程中以环境的形式包含了数据。

通常用面向对象思想能实现的功能，用闭包也能实现。反之亦然。

闭包的写法
```
var extent = function(){
    var value = 0;
    return {
        call: function(){
            value++;
            console.log( value );
        }
    }
};

var extent = extent();

extent.call();     // 输出：1
extent.call();     // 输出：2
extent.call();     // 输出：3
```

换成面向对象的写法，代码如下
```
var extent = {
    value: 0,
    call: function(){
        this.value++;
        console.log( this.value );
    }
};

extent.call();     // 输出：1
extent.call();     // 输出：2
extent.call();     // 输出：3
```

## 闭包与this
在闭包中使用this对象也可能会导致一些问题。我们知道，this对象是在运行时基于函数的执行环境绑定的：在全局函数中，this等于window，而当函数被作为某个对象的方法调用时，this等于那个对象。不过，匿名函数的执行环境具有全局性，因此其this对象通常指向window1。但有时候由于编写闭包的方式不同，这一点可能不会那么明显。下面来看一个例子。

[注意]在通过call()或apply()改变函数执行环境的情况下，this就会指向其他对象。

```
var name = "The Window";

var object = {
    name : "My Object",

    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};

alert(object.getNameFunc()());  //"The Window"（在非严格模式下）
```

以上代码先创建了一个全局变量name，又创建了一个包含name属性的对象。这个对象还包含一个方法——getNameFunc()，它返回一个匿名函数，而匿名函数又返回this.name。由于getNameFunc()返回一个函数，因此调用object.getNameFunc()()就会立即调用它返回的函数，结果就是返回一个字符串。然而，这个例子返回的字符串是"The Window"，即全局name变量的值。为什么匿名函数没有取得其包含作用域（或外部作用域）的this对象呢？

因为每个函数在被调用时都会自动取得两个特殊变量：this和arguments。内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量；不过，把外部作用域中的this对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了，如下所示。

```
var name = "The Window";

var object = {
    name : "My Object",

    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
};

alert(object.getNameFunc()());  //"My Object"
```

## 闭包与内存管理
闭包是一个非常强大的特性，但人们对其也有诸多误解。一种耸人听闻的说法是闭包会造成内存泄露，所以要尽量减少闭包的使用。

局部变量本来应该在函数退出的时候被解除引用，但如果局部变量被封闭在闭包形成的环境中，那么这个局部变量就能一直生存下去。从这个意义上看，闭包的确会使一些数据无法被及时销毁。使用闭包的一部分原因是我们选择主动把一些变量封闭在闭包中，因为可能在以后还需要使用这些变量，把这些变量放在闭包中和放在全局作用域，对内存方面的影响是一致的，这里并不能说成是内存泄露。如果在将来需要回收这些变量，我们可以手动把这些变量设为null。

跟闭包和内存泄露有关系的地方是，使用闭包的同时比较容易形成循环引用，如果闭包的作用域链中保存着一些DOM节点，这时候就有可能造成内存泄露。但这本身并非闭包的问题，也并非JavaScript的问题。在IE浏览器中，由于BOM和DOM中的对象是使用C++以COM对象的方式实现的，而COM对象的垃圾收集机制采用的是引用计数策略。在基于引用计数策略的垃圾回收机制中，如果两个对象之间形成了循环引用，那么这两个对象都无法被回收，但循环引用造成的内存泄露在本质上也不是闭包造成的。

同样，如果要解决循环引用带来的内存泄露问题，我们只需要把循环引用中的变量设为null即可。将变量设置为null意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运行时，就会删除这些值并回收它们占用的内存。

## 闭包关联关键词
1、变量的作用域  
2、函数像一层半透明的玻璃  
3、闭包经典案例：for循环中i值的获取  
4、闭包作用：1封装变量 2延续局部变量的寿命(图片ping案例)  
5、闭包与面向对象  
6、闭包与内存管理
