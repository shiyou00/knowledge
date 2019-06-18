## 前言
虽然现在才学习Deferred显示的有点太晚了，但是可以让我们了解清楚，一些事物的发展。譬如Deferred 和 Promise 之间的差异。

## 什么是deferred对象
> deferred对象就是jQuery的回调函数解决方案

deferred是如何处理回调的。

【ajax普通写法】

```
　　$.ajax({
　　　　url: "test.html",
　　　　success: function(){
　　　　　　alert("哈哈，成功了！");
　　　　},
　　　　error:function(){
　　　　　　alert("出错啦！");
　　　　}
　　});
```


有了deferred对象后的写法
```
$.ajax("test.html")
　.done(function(){ alert("哈哈，成功了！"); })
　.fail(function(){ alert("出错啦！"); });
```

现在是纯链式调用了。

通过上面我们大概已经对deferred有一个初步了解了。就是让回调的方式变成了现在的链式调用。

## deferred在ajax中使用

【常规使用】
```
$.ajax("test.html")
　.done(function(){ alert("哈哈，成功了！"); })
　.fail(function(){ alert("出错啦！"); });
```

【多个回调函数】
```
$.ajax("test.html")
　.done(function(){ alert("哈哈，成功了！");} )
　.fail(function(){ alert("出错啦！"); } )
　.done(function(){ alert("第二个回调函数！");} );
```

【为多个操作指定回调函数】
> 这段代码的意思是，先执行两个操作$.ajax("test1.html")和$.ajax("test2.html")，如果都成功了，就运行done()指定的回调函数；如果有一个失败或都失败了，就执行fail()指定的回调函数。

```
$.when($.ajax("test1.html"), $.ajax("test2.html"))

　　.done(function(){ alert("哈哈，成功了！"); })

　　.fail(function(){ alert("出错啦！"); });
```

## deferred在普通异步函数中的应用

现在有一个这样的函数
```
　　var wait = function(){
　　　　var tasks = function(){
　　　　　　alert("执行完毕！");
　　　　};
　　　　setTimeout(tasks,5000);
　　};
```

我现在想等wait函数执行完成后，获取回调然后再去执行一些事情，该如何处理。

【老方法】
```
　　var wait = function(callback){
　　　　var tasks = function(){
          callback&&callback();  
　　　　　　 alert("执行完毕！");
　　　　};
　　　　setTimeout(tasks,5000);
　　};
```

函数经过这么一改造，我们只需这样调用：
```
wait(function(){
    console.log('callback');
});
```

这就是我们传统的编写回调的方式。那么现在我们有了deferred该如何做能让我们可以链式调用呢？

【新方法】
```
    var wait = function(){
        var dtd = $.Deferred(); //在函数内部，新建一个Deferred对象
        var tasks = function(){
            console.log("执行完毕！");
            dtd.resolve(); // 改变Deferred对象的执行状态
        };
        setTimeout(tasks,5000);
        return dtd.promise(); // 返回promise对象
    };
    $.when(wait())
        .done(function(){ console.log("哈哈，成功了！"); })
        .fail(function(){ console.log("出错啦！"); });
```

代码解释：

`dtd.resolve()`  
> 这行代码其实跟我们上面的 callback&&callback(); 这句非常相似。dtd.resolve()是告诉dtd对象，我们运行结束了。此时会把结果返回出去给done函数。

`return dtd.promise()` 
> jQuery提供了deferred.promise()方法。它的作用是，在原来的deferred对象上返回另一个deferred对象，后者只开放与改变执行状态无关的方法（比如done()方法和fail()方法），屏蔽与改变执行状态有关的方法（比如resolve()方法和reject()方法），从而使得执行状态不能被改变。

这里讲wait方法改造成dtd对象方法有很多种，这里只讲了一种常用的方式。

## deferred对象的方法
1. `$.Deferred()` 生成一个deferred对象
2. `deferred.done()` 指定操作成功时的回调函数
3. `deferred.fail()` 指定操作失败时的回调函数
4. `deferred.promise()` 没有参数时，返回一个新的deferred对象，该对象的运行状态无法被改变；接受参数时，作用为在参数对象上部署deferred接口。
5. `deferred.resolve()` 手动改变deferred对象的运行状态为"已完成"，从而立即触发done()方法。
6. `deferred.reject()` 这个方法与`deferred.resolve()`正好相反，调用后将deferred对象的运行状态变为"已失败"，从而立即触发`fail()`方法。
7. `$.when()` 为多个操作指定回调函数。
8. `deferred.then()`有时为了省事，可以把done()和fail()合在一起写，这就是then()方法。
```
$.when($.ajax( "/main.php" ))
 .then(successFunc, failureFunc );
```
9. `deferred.always()`这个方法也是用来指定回调函数的，它的作用是，不管调用的是deferred.resolve()还是deferred.reject()，最后总是执行。
```
$.ajax( "test.html" )
 .always( function() { alert("已执行！");} );
```

## 小结
相比通过本文，我们会对现在的ES6 Promise 对象更加了解些了。同时如果以后有机会再次使用jquery库时，我们也可以大胆的使用deferred对象，让代码更加优雅！
