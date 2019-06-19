## 前言
之前已经详细的介绍过了`State`对象了，但是其中有params对象配置颇为复杂，本文将详细介绍params的配置

## 一个普通state
```
var mystate = {
  template: '<div ui-view/>',
  controller: function() {}
  url: '/mystate?start',
  params: {
    start: {
      type: 'date',
      value: new Date(),
      squash: true,
    }
  }
}
```
我们可以看到，不仅在url上面要配置好参数,而且还必须要在params对象中对声明好的参数做进一步的配置。


## params 对象的属性值

【type】  
> 参数的类型

常用的类型值有：
```
1. string    // 字符型
2. int       // 数值型
3. bool      // 布尔型
4. date      // new Date(2000, 0, 1)
5. json      // json格式
6. any       // 任意类型
```

【value】
> 设置参数的默认值

【squash】
> 为true当参数值等于默认值时会省略掉该参数

```
  params: {
    myparam: 'defaultParamValue'
    squash: true
  }
```


【dynamic】
> dynamic = true 时，对参数值的更改不会触发state的entered，exited钩子。resolves不会被重新获取，视图也不会被重新加载

默认值：false
```
  params: {
    myparam: 'defaultParamValue'
    squash: true,
    dynamic: false
  }
```

【raw】
> 当raw为true时禁用参数值的url编码

默认值：false
```
  params: {
    taskId: {
      dynamic: false,
      raw: true,
      value: "",
      squash: true
    }
  }
```

## 小结
以上就是params是的一些常用配置。
