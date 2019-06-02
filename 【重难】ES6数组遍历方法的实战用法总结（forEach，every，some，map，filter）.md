## 前言
ES6原生语法中提供了非常多好用的数组'遍历'方法给我们，让我们可以实现更多更强大的功能，下面让我们通过这篇文章好好学习下，该如何使用它们

代码线上测试地址：[babel test](https://www.babeljs.cn/repl/)

<h2 id=1>forEach</h2>
对数组的每个元素执行一次提供的函数。跳过空位元素

> 没有办法中止或者跳出 forEach() 循环，除了抛出一个异常。如果你需要这样，使用 forEach() 方法是错误的。

语法解析
```
arr.forEach((currentValue,index,array)=>{});
// currentValue 数组中正在处理的当前元素
// index 当前索引值
// array 正在处理的数组
// 返回值是undefined
```

例子则是非常简单的应用了
```
[1,2,3].forEach((currentValue,index,array)=>{
	console.log(currentValue,index,array);
});
```

<h2 id=2>every</h2>
回调函数中，所有的都返回真，则返回真，有一个返回假，则返回假。  
简而言之：“一假则假”  

语法解析
```
arr.every((currentValue,index,array)=>{
    // currentValue = 当前执行元素
    // index = 当前索引值
    // array = 执行的数组
})
返回值是true或者false
```

可别小看这个方法，我自己平时工作当中两个方面经常使用到
1. 全选中使用
2. 多个关系的搜索中使用

先看一个简单的例子
```
[12, 5, 8, 130, 44].every((item)=>{
    return item >= 10
})
上面的意思：当数组中所有的元素的值都大于10的时候则返回true，否则返回false
```

全选伪代码示例
```
let allChecked = false;
const arr = [
    {
        id:"a",
        name:"a",
        checked:false
    },
    {
        id:"b",
        name:"b",
        checked:false
    }
]
allChecked = arr.every((item)=>{
    return item.checked === true
})
// 实现起来就是这么简单，当所有的都选中了，allChecked 全选的变量就赋值true
// 这如果硬是用es5的语法去实现的话，还是比较麻烦的
```

<h2 id=3>some</h2>
回调函数中有一个返回真，则返回真  
简言之：“一真则真”  

语法解析
```
arr.some((currentValue,index,array)=>{
    // currentValue = 当前执行元素
    // index = 当前索引值
    // array = 执行的数组
})
返回值是true或者false
```

代码展示
```
let bok = [2, 5, 8, 1, 4].some((item)=>{
    return item>5
})
// bok = true
// 只要有一个数组大于5 则整体返回true
```

<h2 id=4>map</h2>
> 创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

语法
```
arr.map((currentValue,index,array)=>{
    // currentValue = 当前执行元素
    // index = 当前索引值
    // array = 执行的数组
})

// 返回一个新的数组
```

实例
```
let arr = [1,2,3,4];
const map = arr.map(x=>x*2);
//返回每一个处理过后的新数组 [2,4,6,8]
```

<h2 id=5>filter</h2>
> 返回一个新数组，其结果是改数组中的每个元素符合条件的结果

语法
```
arr.filter((currentValue,index,array)=>{
    // currentValue = 当前执行元素
    // index = 当前索引值
    // array = 执行的数组
})

// 返回一个新的数组
```

顾名思义这个应该肯定是各类查询，筛选上面

实例
```
let arr = [{name:"abc"},{name:"bcd"},{name:"afc"}];

arr.filter((item)=>{
    return item.name.includes('b');
});
// 筛选出名字字段中带有b的项
```

<h2 id=6>reduce && reduceRight</h2>
> reduce()方法在数组的每个成员上执行一个reducer函数(您提供的)，生成一个输出值。

> reduceRight是从右到左的相加(其它的同reduce是一样的，所以这里只讲reduce)

语法
```
无参数
arr.reduce((accumulator, currentValue,currentIndex,array)=>{
    // accumulator第一项的值或者上一次叠加的结果值
    // currentValue 当前项
    // currentIndex 当前项索引
    // array 数组本身
});
有参数
arr.reduce((accumulator, currentValue,currentIndex,array)=>{},参数);
参数 = accumulator 第一次运行时的初始值
```

实例1：计算数据总和
```
const arr = [1,2,3];
const num = arr.reduce((acc,cur,index)=>{
    return acc + cur
});
// num = 6

const num1 = arr.reduce((acc,cur,index)=>{
    return acc+cur
},10)
// num = 16
```

实例2：计算一个字符串中字母出现的次数
```
const str = 'aaabbcccdd';
str.split('').reduce((acc,cur)=>{
    acc[cur] ? acc[cur]++ : acc[cur] = 1
},{});
解析：初始化的值是一个空对象
运行的时候，判断对象里面是不是有当前的字母，
如果没有的话则添加到对象中，并赋值为1
如果已经存在的话在++，这样就计算出一个字符串中字母出现的次数
同样可以利用这点进行数组去重

const arr = ['a','a','b','c'];

const obj = arr.reduce((acc,cur)=>{
    return acc[cur] ? acc[cur]++ : acc[cur] = 1
},{})

最后通过obj.keys() 的方法获取到的数组就是去重之后的。
```

<h2 id=7>indexOf</h2>
> indexOf()方法返回在数组中可以找到给定元素的第一个索引，如果不存在，则返回-1。

```
var beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];

console.log(beasts.indexOf('bison'));
// expected output: 1

// start from index 2
console.log(beasts.indexOf('bison', 2));
// expected output: 4

console.log(beasts.indexOf('giraffe'));
// expected output: -1
```

<h2 id=8>lastIndexOf</h2>
> lastIndexOf()方法返回给定元素在数组中找到的最后一个索引，如果该元素不存在，则返回-1。数组从fromIndex开始向后搜索。

```
var animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

console.log(animals.lastIndexOf('Dodo'));
// expected output: 3

console.log(animals.lastIndexOf('Tiger'));
// expected output: 1
```

## 小结
本篇文章主要讲述了ES6中数组新增的一些方法，以及如何使用。其实这些方法的实战场景还是非常多的，需要在实战中才能有更加深刻的体会
