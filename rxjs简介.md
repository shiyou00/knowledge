## RxJS 基本介紹
RxJS 是一个库，它通过使用 observable 序列来编写异步和基于事件的程序。它提供了一个核心类型 Observable，附属类型 (Observer、 Schedulers、 Subjects) 和受 [Array#extras] 启发的操作符 (map、filter、reduce、every, 等等)，这些数组操作符可以把异步事件作为集合来处理。

> 可以把 RxJS 当做是用来处理事件的 Lodash 。

ReactiveX 结合了 观察者模式、迭代器模式 和 使用集合的函数式编程，以满足以一种理想方式来管理事件序列所需要的一切。

在 RxJS 中用来解决异步事件管理的的基本概念是：
- Observable (可观察对象)
- Observer (观察者)
- Subscription (订阅)
- Operators (操作符)
- Subject (主体)
- Schedulers (调度器)

下面分别来概述下核心概念，让大家对rxjs的主体部分有个了解

## Observable
它是rxjs最核心的概念，理解了它基本上就理解了60%的rxjs了。

标签1：`可观察对象`
标签2：`惰性运算`
> 当我们创建一个Observable可观察对象，只有当我们对它进行订阅了之后，它才会执行；不订阅不执行！

【创建第一个Observable】  
```
const observable = Observable.create(function (observer) {
  observer.next(1);
});
```

【订阅刚刚创建的可观察对象】  
```
const subscriber = observable.subscribe({
  next: x => console.log('值：' + x),
  error: err => console.error('报错：' + err),
  complete: () => console.log('结束')
});
```
只有订阅过后它才会去执行。

这时我们可以看下控制台：
![](./image/10.png)
输出了上面`observer.next(1)`推送过来的1;

`observable生产者` 把消息推送给 `subscriber消费者`

上面创建的是一个同步执行的`Observable`，接下来我们来创建一个异步的`Observable`
```
asyncObservable() {
    const observable = Observable.create(function (observer) {
      setTimeout(() => {
        observer.next(1); // 这里就好比我们跟后台请求的数据，在angular中一般存放在service中
      }, 3000 );
    });
    console.log('before subscribe');
    // 这里就好比我们在组件中订阅了service的请求数据，当异步数据请求到了就会返回给组件
    observable.subscribe({
      next: x => console.log('值：' + x),
      error: err => console.error('报错：' + err),
      complete: () => console.log('结束')
    });
    console.log('after subscribe');
}
```
输出循序：  
1. before subscribe
2. after subscribe
3. 值：1

我们通过这种方式就已经替代了基础版本的 callback模式回调处理请求来的数据。

上面我们对可观察对象分别做了几个动作：创建、订阅、执行

既然有创建就一定有清除，不然就要内存溢出了

【清除一个可观察对象】
```
  clearObservable() {
    const observable = Observable.create(function (observer) {
      setTimeout(() => {
        observer.next(1);
      }, 3000 );
    });
    const subscriber = observable.subscribe({
      next: x => console.log('值：' + x),
      error: err => console.error('报错：' + err),
      complete: () => console.log('结束')
    });

    subscriber.unsubscribe(); // 这句就是清除一个可观察对象
  }
```

到这里为止，已经基本上对 `Observable` 这个可观察对象已经有一个了解了。  


## Observer
它的中文名叫`观察者`，观察者是由 `Observable` 发送的值的消费者。
  
上面已经提到了 `Observable`是生产者；`subscriber`是消费者。

`Observer` 对 `Observable` next(1) 出来的值进行了消费。

通俗来讲就是它了：
```
const observer = {
                   next: x => console.log('值：' + x),
                   error: err => console.error('报错：' + err),
                   complete: () => console.log('结束')
                 }
const subscriber = observable.subscribe(observer);
```

这样看代码是不是清晰了很多：   
observer = '值的消费者' = '对象且包含一组不类型的消费者'   
所以说对象里面的 `next: ()=>{}` , `error: ()=>{}` , `complete: ()=>{}` 这三个函数就是真正意义上的数据消费者了，它们3个就组成了数据消费对象`observer`

所以`Observer`的标签就是：
1. 观察者
2. 值的消费者


### Observer特性
> 底下有3个数据消费者，当我派出哪个函数，才会消费相应的数据，不派出则不消费











