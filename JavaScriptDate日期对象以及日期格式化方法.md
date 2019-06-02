## 前言
Date对象是javascript语言中内置的数据类型，用于提供日期和时间的操作接口。Date对象是在早期java中的java.util.Date类基础上创建的，为此，Date类型使用自UTC1970年1月1日0点开始经过的毫秒数来保存日期，它可以表示的时间范围是1970年1月1日0点前后的各1亿天。

## 静态方法
总共有3个静态方法：Date.now()、Date.parse()、Date.UTC()

【Date.now()】
> ECMAScript5新增了now()方法，该方法返回当前时间距离1970年1月1日0点UTC的毫秒数。该方法不支持传递参数

```
Date.now() // 1554790104142
+new Date() // 会把时间隐式转换成时间戳 1554790169778
new Date().getTime() // 1554790104942
```

【Date.parse()】
> 该方法用于解析一个日期字符串，参数是一个包含待解析的日期和时间的字符串，返回从1970年1月1日0点到给定日期的毫秒数

该方法会根据日期时间字符串格式规则来解析字符串的格式，除了标准格式外，以下格式也支持。如果字符串无法识别，将返回NaN

1、'月/日/年' 如6/13/2004

2、'月 日,年' 如January 12,2004或Jan 12,2004

3、'星期 月 日 年 时:分:秒 时区' Tue May 25 2004 00:00:00 GMT-0700

```
Date.parse('4/9/2019') // 1554739200000
Date.parse('May 12,2019') // 1557590400000
Date.parse('Tue May 12 2019 00:00:00 GMT-0700') // 1557644400000
Date.parse('T00:00:00') // NaN
Date.parse() // NaN
```

[注意]在ECMAScript5中，如果使用标准的日期时间字符串格式规则的字符串中，数字前有前置0，则会解析为UTC时间，时间没有前置0，则会解析为本地时间。其他情况一般都会解析为本地时间
```
console.log(Date.parse('7/12/2016'));//1468252800000
console.log(Date.parse('2016-7-12'));//1468252800000
console.log(Date.parse('2016-07-12'));//1468281600000
```

【Date.UTC()】
> Date.UTC()同样返回给定日期的毫秒数，但其参数并不是一个字符串，而是分别代表年、月、日、时、分、秒、毫秒的数字参数

```
console.log(Date.UTC(1970));//NaN
console.log(Date.UTC(1970,0));//0
console.log(Date.UTC(1970,0,2));//86400000
console.log(Date.UTC(1970,0,1,1));//3600000
console.log(Date.UTC(1970,0,1,1,59));//714000
console.log(Date.UTC(1970,0,1,1,59,30));//717000
```

## 构造函数
Date()构造函数有多达5种的使用方法

【1】Date()函数可以不带new操作符，像一个函数一样调用。它将忽略所有传入的参数，并返回当前日期和时间的一个字符串表示

[注意]由于Date()函数没有使用操作符，实际上它不能被称为构造函数
```
console.log(Date());//Tue Apr 09 2019 15:57:50 GMT+0800 (中国标准时间)
console.log(Date('2016/1/1'));//Tue Apr 09 2019 15:57:50 GMT+0800 (中国标准时间)
console.log(typeof Date());//'string'
```

【2】Date()函数使用new操作符，且不带参数时，将根据当前时间和日期创建一个Date对象
```
console.log(new Date());//Tue Apr 09 2019 15:59:13 GMT+0800 (中国标准时间)
console.log(new Date);//Tue Apr 09 2019 15:59:13 GMT+0800 (中国标准时间)
console.log(typeof new Date());//'object'
```

【3】Date()函数可接受一个数字参数，该参数表示设定时间与1970年1月1日0点之间的毫秒数
```
console.log(new Date(0));//Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
console.log(new Date(86400000));//Fri Jan 02 1970 08:00:00 GMT+0800 (中国标准时间)
console.log(typeof new Date(0));//object
```

【4】Date()函数可接受一个字符串参数，参数形式类似于Date.parse()方法。但parse()方法返回的是一个数字，而Date()函数返回的是一个对象 
```
console.log(new Date('6/13/2004'));//Sun Jun 13 2004 00:00:00 GMT+0800 (中国标准时间)
console.log(Date.parse('6/13/2004'));//1087056000000
console.log(typeof new Date(6/13/2004));//object
console.log(typeof Date.parse(6/13/2004));//number
```

**关于标准的日期时间字符串中前置0的处理，也类似于Date.parse()方法，若有前置0，则相当于UTC时间，若没有，则相当于本地时间。其余情况一般都为本地时间**
```
console.log(new Date('7/12/2016'));//Tue Jul 12 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(new Date('2016-7-12'));//Tue Jul 12 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(new Date('2016-07-12'));//Tue Jul 12 2016 08:00:00 GMT+0800 (中国标准时间)
```

【5】Date()函数可接受参数形式类似于Date.UTC()方法的参数，但Date.UTC()方法返回是一个毫秒数，且是UTC时间，而Date()函数返回是一个对象，且是本地时间
```
console.log(new Date(2016,7,12));//Fri Aug 12 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(+new Date(2016,7,12));//1470931200000
console.log(typeof new Date(2016,7,12));//'object'
console.log(Date.UTC(2016,7,12));//1470960000000
console.log(typeof Date.UTC(2016,7,12));//'number'
```

[注意]使用参数类似于Date.parse()函数的方法时，如果日期对象超出范围，浏览器会自动将日期计算成范围内的值；使用参数类似于Date.UTC()函数的方法时，如果日期对象超出范围，浏览器会提示Invalid Date
```
console.log(new Date(2016,7,32));//Thu Sep 01 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(new Date(2016,8,1));//Thu Sep 01 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(new Date('2016-8-32'));//Invalid Date
console.log(new Date('2016-9-1'));//Thu Sep 01 2016 00:00:00 GMT+0800 (中国标准时间)
```

## 实例方法
Date对象没有可以直接读写的属性，所有对日期和时间的访问都需要通过方法。Date对象的大多数方法分为两种形式：一种是使用本地时间，一种是使用UTC时间，这些方法在下面一起列出。例如，get[UTC]Day()同时代表getDay()和getUTCDay()

Date对象一共有46个实例方法，可以分为以下3类：to类、get类、set类

【to类】
> to类方法从Date对象返回一个字符串，表示指定的时间

toString() 返回本地时区的日期字符串

toUTCString() 返回UTC时间的日期字符串

toISOString() 返回Date对象的标准的日期时间字符串格式的字符串

toDateString() 返回Date对象的日期部分的字符串

toTimeString() 返回Date对象的时间部分的字符串

toJSON() 返回一个符合JSON格式的日期字符串，与toISOString方法的返回结果完全相同

toLocaleString() toString()方法的本地化转换

toLocaleTimeString() toTimeString()方法的本地化转换

toLocaleDateString() toDateString()方法的本地化转换

```
console.log(new Date('2016-7-12').toString());//Tue Jul 12 2016 00:00:00 GMT+0800 (中国标准时间)
console.log(new Date('2016-7-12').toUTCString());//Mon, 11 Jul 2016 16:00:00 GMT
console.log(new Date('2016-7-12').toISOString());//2016-07-11T16:00:00.000Z
console.log(new Date('2016-7-12').toDateString());//Tue Jul 12 2016
console.log(new Date('2016-7-12').toTimeString());//00:00:00 GMT+0800 (中国标准时间)
console.log(new Date('2016-7-12').toJSON());//2016-07-11T16:00:00.000Z

console.log(new Date('2016-7-12').toLocaleString());//2016/7/12 上午12:00:00
console.log(new Date('2016-7-12').toLocaleDateString());//2016/7/12
console.log(new Date('2016-7-12').toLocaleTimeString());//上午12:00:00
```

*【get类】  
Date对象提供了一系列get类方法，用来获取实例对象某个方面的值

getTime()  
返回距离1970年1月1日0点的毫秒数，同valueOf()

在ECMAScript5之前，可以使用getTime()方法实现Date.now()
```
Date.now = function(){
    return (new Date()).getTime()
}
```

getTimezoneOffset()  
返回当前时间与UTC的时区差异，以分钟表示(8*60=480分钟)，返回结果考虑到了夏令时因素

```
new Date('2016-7-12').valueOf() //1468252800000
new Date('2016-7-12').getTime() //1468252800000
new Date('2016-7-12').getTimezoneOffset() // -480
```

get[UTC]FullYear() 返回年份(4位数)

get[UTC]Month() 返回月份(0-11)

get[UTC]Date() 返回第几天(1-31)

get[UTC]Day() 返回星期几(0-6)

get[UTC]Hours() 返回小时值(0-23)

get[UTC]Minutes() 返回分钟值(0-59)

get[UTC]Seconds() 返回秒值(0-59)

get[UTC]Milliseconds() 返回毫秒值(0-999)

[注意]通过标准日期时间格式字符串，且有前置0的形式的参数设置，设置的是UTC时间

```
const d = new Date('2019-04-12 12:30:30');

d.getFullYear() // 2019
d.getUTCFullYear() // 2019
d.getMonth() // 3
d.getUTCMonth() // 3
d.getDate() // 12
d.getUTCDate() //12
d.getDay() //5
d.getUTCDay() //5
d.getHours() //12
d.getUTCHours() //4
d.getMinutes() //30
d.getSeconds() // 30
d.getMilliseconds() // 0
```

*【set类】  
Date对象提供了一系列set类方法，用来设置实例对象的各个方面

set方法基本与get方法相对应，set方法传入类似于Date.UTC()的参数，返回调整后的日期的内部毫秒数

**[注意]星期只能获取，不能设置**

```
const d = new Date('2019-04-12 12:30:30');
1、先设置一个时间
d.setFullYear('2018') // 1523507430000
// 再去获取就改变了
d.getFullYear() // 2018
```

setTime() 使用毫秒的格式，设置一个Date对象的值

set[UTC]FullYear() 设置年份(4位数)，以及可选的月份值和日期值

set[UTC]Month() 设置月份(0-11)，以及可选的日期值

set[UTC]Date() 设置第几天(1-31)

set[UTC]Hours() 设置小时值(0-23)，以及可选的分钟值、秒值及毫秒值

set[UTC]Minutes() 设置分钟值(0-59)，以及可选的秒值及毫秒值

set[UTC]Seconds() 设置秒值(0-59)，以及可选的毫秒值

set[UTC]Milliseconds() 设置毫秒值(0-999)


## 日期格式化方案

```
Date.prototype.format = function(fmt) {
    var o = {
        "M+" : this.getMonth()+1,                 //月份
        "d+" : this.getDate(),                    //日
        "h+" : this.getHours(),                   //小时
        "m+" : this.getMinutes(),                 //分
        "s+" : this.getSeconds(),                 //秒
        "q+" : Math.floor((this.getMonth()+3)/3), //季度
        "S"  : this.getMilliseconds()             //毫秒
    };
    // 正则匹配y
    if(/(y+)/.test(fmt)) {
        // RegExp.$1 = yyyy
        fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));
    }
    // 分别取用正则去匹配和替换月日时分秒
    for(var k in o) {
        if(new RegExp("("+ k +")").test(fmt)){
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
        }
    }
    return fmt;
};

// 使用
const time = new Date().format("yyyy-MM-dd hh:mm:ss");
```

[注意]这是一个简单版本的时间格式化工具，有很多情况并未完全考虑到。
