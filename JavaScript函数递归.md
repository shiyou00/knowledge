## 递归函数
> 递归函数是在一个函数通过名字调用自身的情况下构成的

项目中例如树状结构渲染，对象深复制，等很多方面都会使用到递归函数

```
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * factorial(num-1);
    }
}
```

非严格模式下还可以使用arguments.callee
```
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num-1);
    }
}
```

[注意]arguments.callee === factorial === 函数本身


## 对象深复制(简单版)

```
function deepClone(obj) {
    // 判断是不是对象
  function isObject(o) {
    return (typeof o === 'object' || typeof o === 'function') && o !== null
  }
    // 不是对象的话报错
  if (!isObject(obj)) {
    throw new Error('非对象')
  }
    
  let isArray = Array.isArray(obj)
  let newObj = isArray ? [...obj] : { ...obj } // 是数组的话 用数组的解构来实现对象的复制，对象也同理
// 静态方法 Reflect.ownKeys() 返回一个由目标对象自身的属性键组成的数组
  Reflect.ownKeys(newObj).forEach(key => {
    newObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key] // 这里进行递归实现多层对象的复制
  })

  return newObj
}
```

## 小结
通过本文我们学习了什么是递归，以及递归在项目中的使用，并且有一点一定要注意，递归函数一定要有终止条件不然会导致栈溢出。
