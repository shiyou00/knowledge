## 属性
字符串String类型的每个实例都有一个length属性，表示字符串中的字符个数。由于字符串是不可变的，所以字符串的长度也不可变

字符串的length属性不会在for/in循环中枚举，也不能通过delete操作符删除

**[注意]对于字符串s来说，最后一个字符的索引是`s.length - 1`**

```
var str = "test";
console.log(str.length);//4
str.length = 6;
console.log(str,str.length);//"test",4
```

## 实例方法
字符串String对象有多达20多个实例方法，包括toString()、toLocaleString()、valueOf()从Object对象继承的3种对象通用方法，chartAt()、中括号[]、charCodeAt()和fromCharCode()4种访问字符方法，concat()和加号+这2种字符串拼接方法，slice()、substr()和substring()3种创建子字符串方法，toLowerCase()、toLocaleLowerCase()、toUpperCase()、toLocaleUpperCase()这4种大小写转换方法，indexOf()和lastIndexOf()这2种查找字符串位置的方法，match()、search()、replace()、split()这4种正则匹配方法以及去除首尾空格的trim()方法

## 对象通用方法
String类型是与字符串对应的包装类型，继承了Object对象的通用方法toString()、toLocaleString()、valueOf()这三个方法

【toString()】  
toString()方法返回string的原始字符串值

【toLocaleString()】  
toLocaleString()方法返回string的原始字符串值

【valueOf()】  
valueOf()方法返回string的原始字符串值

```
console.log("test".valueOf());//"test"
console.log("test".toString());//"test"
console.log("test".toLocaleString());//"test"
```

## 访问字符方法
字符串的访问字符方法总共有chartAt()、中括号[]、charCodeAt()和fromCharCode()四种

【chartAt()】  
charAt()方法接收一个基于0的字符位置的参数，返回指定位置的字符。当参数为空或NaN时，默认参数为0；当参数超出范围时，则返回一个空字符串　
```
var str = "hello";
console.log(str.charAt(1));//e
console.log(str.charAt(-1));//''
console.log(str.charAt(10));//''
console.log(str.charAt());//h 
console.log(str.charAt(NaN));//h
```

charAt()方法涉及到Number()函数的隐式类型转换，如果转换为数值，则按照上述规则输出字符串；如果转换为NaN，则输出第0个字符
```
var str = "hello";
console.log(str.charAt(true));//'e'
console.log(str.charAt(false));//'h'
console.log(str.charAt('abc'));//'h'
console.log(str.charAt({}));//'h'
console.log(str.charAt([2]));//'l'
```

【中括号】
ECMAScript5定义了另一个访问字符的方法，使用方括号加数字索引来访问字符串中的特定字符。如果参数超出范围或是NaN时，则输出undefined；没有参数时，会报错；该方法没有Number()转型函数的隐式类型转换，但参数为单数值数组时可转换为数值

[注意]IE7-浏览器不支持

```
var str = "hello";
console.log(str[0]);//h
console.log(str[[1]]);//e
console.log(str[false]);//undefined
console.log(str[-1]);//undefined
console.log(str[NaN]);//undefined
console.log(str[]);//报错
```

【charCodeAt()】  
charCodeAt()方法类似于charAt()方法，接收一个基于0的字符位置的参数，但返回的是指定位置的字符16位Unicode编码。返回值是一个16位的整数，在0-65535之间，即0x0000-0xffff之间

参数为空或NaN时，默认参数为0；当参数超出范围时，则返回NaN

```
var str = "hello";
console.log(str.charCodeAt());//104
console.log(str.charCodeAt(0));//104
console.log(str.charCodeAt(1));//101
console.log(str.charCodeAt(-1));//NaN
console.log(str.charCodeAt(10));//NaN
console.log(str.charCodeAt(NaN));//104
```

同样地，charCodeAt()方法涉及到Number()函数的隐式类型转换，如果转换为数值，则按照上述规则输出相应值；如果转换为NaN，则输出第0个字符的字符编码
```
var str = "hello";
console.log(str.charCodeAt(true));//101
console.log(str.charCodeAt(false));//104
console.log(str.charCodeAt('abc'));//104
console.log(str.charCodeAt({}));//104
console.log(str.charCodeAt([2]));//l08
```

【fromCharCode()】  
String构造函数本身有一个静态方法:fromCharCode()。这个方法的任务是接收一个或多个字符编码，然后把它们转换成一个字符串。从本质上看，这个方法与实例方法charCodeAt()执行的是相反的操作。若参数为空或NaN时，则返回空字符串；若参数超出0-65535的范围，则输出字符不可控

```
console.log(String.fromCharCode(104,101,108,108,111));//'hello'
console.log(String.fromCharCode());//''
console.log(String.fromCharCode(NaN));//''
console.log(String.fromCharCode(-1));
console.log(String.fromCharCode(65560));
```

如果一个字符占用四字节，则需要拆成两个字符表示
```
console.log(String.fromCharCode(0xD842, 0xDFB7)); // "𠮷"
```

## 字符串拼接
关于字符串拼接共有concat()和加号+两种方法

【concat()】  
concat()方法用于将一个或多个字符串拼接起来，返回拼接得到的新字符串，而原字符串不发生改变。若参数(第一个参数除外)不是字符串，则通过String()方法隐式转换为字符串，再进行字符串拼接

```
var stringValue = 'hello ';
var result = stringValue.concat('world','!');
console.log(result);//'hello world!'
console.log(stringValue);//'hello'
```

[注意]第一个参数只能是字符串，如果是其他类型(数组除外)则报错
```
(1).concat('2');//报错
(true).concat('false');//报错
({}).concat('abc');//报错
```

[注意]由于数组也存在concat()方法，参数会按照首先出现的参数是数组还是字符串来决定如何转换
```
'1,2,3,'.concat([4,5]);//'1,2,3,4,5'
[1,2,3].concat(',4,5');//[1, 2, 3, ",4,5"]
```

【加号运算符(+)】  
虽然concat()是专门用来拼接字符串的方法，但实践中使用更多的还是加号运算符(+)。使用加号运算符在许多时候都比concat()简单方便

```
var stringValue = 'hello ';
console.log(stringValue.concat('world','!'));//'hello world!'
console.log(stringValue + 'world' + '!');//'hello world!' 
```

[注意]当操作数其中一个是字符串，或者对象转换为字符串时，才进行字符串拼接
```
1 + 2;//3
'1' + 2;//'12'
var o = {valueOf:function(){return '1';}};
o + 2;//'12'
var o = {valueOf:function(){return 1;}};
o + 2;//3
```

## 创建子字符串
创建子字符串共有slice()、substr()和substring()三种方法

【slice()】  
slice(start,end)方法需要两个参数start和end，返回这个字符串中从start位置的字符到(但不包含)end位置的字符的一个子字符串；如果end为undefined或不存在，则返回从start位置到字符串结尾的所有字符

如果start是负数，则start = max(length + start,0)

如果end是负数，则end = max(length + end,0)

start和end无法交换位置

```
var stringValue = 'hello world';
console.log(stringValue.slice());//'hello world'
console.log(stringValue.slice(0, 2));//'he'
console.log(stringValue.slice(0, -2));//'hello wor'
console.log(stringValue.slice(2));//'llo world'
console.log(stringValue.slice(-2));//'ld'
console.log(stringValue.slice(2,undefined));//'llo world'
console.log(stringValue.slice(2,-5));//'llo '
console.log(stringValue.slice(2,-20));//''
console.log(stringValue.slice(20));//''
console.log(stringValue.slice(-2,2));//''
console.log(stringValue.slice(-2,-20));//''            
console.log(stringValue.slice(-2,20));//'ld'
console.log(stringValue.slice(-20,2));//'he'
console.log(stringValue.slice(-20,-2));//'hello wor'
```

slice()方法涉及到Number()转型函数的隐式类型转换，当start被转换为NaN时，相当于start = 0；当end被转换为NaN时(end为undefined除外)，则输出空字符串
```
var stringValue = 'hello world';
console.log(stringValue.slice(NaN));//'hello world'
console.log(stringValue.slice(0,NaN));//''
console.log(stringValue.slice(true,[3]));//'el'
console.log(stringValue.slice(null,undefined));//'hello world'
console.log(stringValue.slice({}));//'hello world'
console.log(stringValue.slice('2',[5]));//'llo'
```

【substring()】  
substring(start,end)方法需要两个参数start和end，返回这个字符串中从start位置的字符到(但不包含)end位置的字符的一个子字符串；如果end为undefined或不存在，则返回从start位置到字符串结尾的所有字符

如果任一参数是NaN或负数，则被0取代

如果任一参数大于字符串长度，则被字符串长度取代

如果start 大于 end，则交换它们的值

```
var stringValue = 'hello world';
console.log(stringValue.substring());//'hello world'
console.log(stringValue.substring(2));//'llo world'
console.log(stringValue.substring(2,undefined));//'llo world'
console.log(stringValue.substring(20));//''
console.log(stringValue.substring(-2,2));//'he'
console.log(stringValue.substring(NaN,2));//'he'
console.log(stringValue.substring(-2,20));//'hello world'
console.log(stringValue.substring(3,2));//'l'
console.log(stringValue.substring(3,NaN));//'hel'
console.log(stringValue.substring(-20,2));//'he'
console.log(stringValue.substring(-20,-2));//'' 
```

同样地，substring()方法也涉及到Number()转型函数的隐式类型转换
```
var stringValue = 'hello world';
console.log(stringValue.substring(true,[3]));//'el'
console.log(stringValue.substring(null,undefined));//'hello world'
console.log(stringValue.substring({}));//'hello world'
console.log(stringValue.substring('2',[5]));//'llo'
```

【substr()】  
substr(start,end)方法需要两个参数start和end，end代表返回的子字符串的字符个数；该方法返回这个字符串中从start位置的字符开始的end个字符的一个子字符串；如果end为undefined或不存在，则返回从start位置到字符串结尾的所有字符

如果start是负数，则start = max(length + start,0)

如果start是NaN，则相当于start = 0

如果end是负数或NaN，则end = 0，因此会返回空字符串

start和end无法交换位置

[注意]该方法不是ECMAScript标准，已经被弃用

[注意]IE8-浏览器在处理向substr()传递负值的情况时存在问题，它会返回原始的字符串

## 大小写转换
ECMAScript中涉及字符串大小写转换的方法有4个：toLowerCase()、toLocaleLowerCase()、toUpperCase()、toLocaleUpperCase()

toLowerCase()和toUpperCase()是两个经典的方法，借鉴自java.lang.String中的同名方法。而toLocaleLowerCase()和toLocaleUpperCase()方法则是针对特定地区的实现，对有些地区来说，针对地区的方法与其通用方法得到的结果相同，但少数语言(如土耳其语)会为Unicode大小写转换应用特殊的规则，这时候就必须使用针对地区的方法来保证实现正确的转换

【toUpperCase()】  
toUpperCase()方法将字符串转换成大写

【toLowerCase()】  
toLowerCase()方法将字符串转换成小写

【toLocaleUpperCase()】  
toLocaleUpperCase()方法将字符串转换成大写(针对地区)

【toLocaleLowerCase()】  
toLocaleLowerCase()方法将字符串转换成小写(针对地区)

[注意]在不知道自己的代码将在哪个语言环境中运行的情况下，使用针对地区的方法更稳妥
```
var string = 'Hello World';
console.log(string.toLowerCase());//hello world
console.log(string.toLocaleLowerCase());//hello world
console.log(string.toUpperCase());//HELLO WORLD
console.log(string.toLocaleUpperCase());//HELLO WORLD
```

这4种方法均不支持String()隐式类型转换，只支持字符串类型
```
(true).toLowerCase();//报错
(2).toLocaleLowerCase();//报错
({}).toUpperCase();//报错
([]).toLocaleUpperCase();//报错
```

[注意]大小写转换方法可以连续使用
```
var string = 'Hello World';
console.log((string.toUpperCase()).toLowerCase());//hello world
```

将带有分割符的字符串转换为驼峰的形式
```
var txt = "border-top-left";
var arr = txt.split('-');
for(var i = 1; i < arr.length; i++){
    arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
}
var result = arr.join('');
console.log(result);//'borderTopLeft"
```

## 查找子串位置
有两个从字符串中查找子字符串位置的方法：indexOf()和lastIndexOf()。查找子串位置的方法同访问字符方法charAt()和中括号[]方法有相反的地方，一个通过字符串查找位置，一个则是通过位置查找字符

【indexOf()】  
indexOf(searchString,start)方法接收searchString和start两个参数，返回searchString首次出现的位置，如果没有找到则返回-1

该方法会隐式调用String()转型函数，将searchString非字符串值转换为字符串；隐式调用Number()转型函数，将start非数字值(undefined除外)转换为数值

searchString表示要搜索的子字符串；start表示该搜索的开始位置，若忽略该参数或该参数为undefined、NaN或负数时，start = 0

```
var string = 'hello world world';
console.log(string.indexOf('ld'));//9
console.log(string.indexOf('ld',undefined));//9
console.log(string.indexOf('ld',NaN));//9
console.log(string.indexOf('ld',-1));//9
console.log(string.indexOf('ld',10));//15
console.log(string.indexOf('ld',[10]));//15
console.log(string.indexOf('true',[10]));//-1
console.log(string.indexOf(false,[10]));//-1
```

【lastIndexOf()】  
与indexOf()不同，lastIndexOf()从右向左查找

lastIndexOf(searchString,start)方法接收searchString和start两个参数，返回searchString第一次出现的位置，如果没有找到则返回-1

同样地，该方法会隐式调用String()转型函数，将searchString非字符串值转换为字符串；隐式调用Number()转型函数，将start非数字值(undefined除外)转换为数值

searchString表示要搜索的子字符串；start表示该搜索的开始位置，若忽略该参数或该参数为undefined、NaN时，start = length - 1；若start为负数，start = 0

```
var string = 'hello world world';
console.log(string.lastIndexOf('ld'));//15
console.log(string.lastIndexOf('ld',undefined));//15
console.log(string.lastIndexOf('ld',NaN));//15
console.log(string.lastIndexOf('ld',-1));//-1
console.log(string.lastIndexOf('h',-1));//0
console.log(string.lastIndexOf('w',undefined));//12

console.log(string.lastIndexOf('ld',10));//9
console.log(string.lastIndexOf('ld',[10]));//9
console.log(string.lastIndexOf('true',[10]));//-1
console.log(string.lastIndexOf(false,[10]));//-1
```

可以通过循环调用indexOf()或lastIndexOf()来找到所有匹配的子字符串
```
function allIndexOf(str,value){
    var result = [];
    var pos = str.indexOf(value);
    while(pos > -1){
        result.push(pos);
        pos = str.indexOf(value,pos+value.length);
    }
    return result;
}
console.log(allIndexOf('helllhelllhelll','ll'));//[2,7,12]
```

lastIndexOf()方法常用于获取URL地址中的扩展名
```
var url = "http://cnblogs.com/xiaohuochai.txt";
function getFileFormat(url){
    var pos = url.lastIndexOf('.');
    return url.slice(pos+1);
}
console.log(getFileFormat(url));//'txt'
```

## 正则匹配方法
javascript中的一些正则操作如查找和测试等可以通过RegExp的方法实现，而切分和替换等另一些操作可以通过String类的方法实现

String类共有match()、search()、replace()、split()这4种正则匹配方法 

【match()】  
match()方法只接受一个为正则或字符串的参数，并以数组的形式返回匹配的内容。这个方法类似于正则表达式RegExp的exec()方法，只是调换了RegExp和String对象的位置

若匹配失败，则match()方法返回null
```
'x'.match(/y/);//null
```

【1】若不设置全局标志，match()方法和exec()方法结果相同
```
var string = 'cat,bat,sat,fat';
var pattern = /.at/;
var matches = string.match(pattern);
console.log(matches,matches.index,matches.input);//['cat'] 0 'cat,bat,sat,fat' 

var string = 'cat,bat,sat,fat';
var pattern = /.at/;
var exec = pattern.exec(string);
console.log(exec,exec.index,exec.input);//['cat'] 0 'cat,bat,sat,fat' 
```

【2】设置全局标志后，exec()方法依然返回单次的匹配结果，而match()方法会返回一个字符串数组，其中包括各次成功匹配的文本，但没有index和input属性

```
var string = 'cat,bat,sat,fat';
var pattern = /.at/g;
var matches = string.match(pattern);
console.log(matches,matches.index,matches.input);//["cat", "bat", "sat", "fat"] undefined undefined 

var string = 'cat,bat,sat,fat';
var pattern = /.at/g;
var exec = pattern.exec(string);
console.log(exec,exec.index,exec.input);//['cat'] 0 'cat,bat,sat,fat' 
var exec = pattern.exec(string);
console.log(exec,exec.index,exec.input);//['bat'] 4 'cat,bat,sat,fat' 
```

【3】match()方法作为字符串String的方法，接受参数为字符串，结果与不设置全局标志的正则表达式为参数相同，只返回第一个匹配项，且具有index和input属性
```
var string = 'cat,bat,sat,fat';
var matches = string.match('at');
console.log(matches,matches.index,matches.input);//['at'] 1 'cat,bat,sat,fat'
var matches = string.match('at');
console.log(matches,matches.index,matches.input);//['at'] 1 'cat,bat,sat,fat'
```

当不设置全局标志时，match()方法和exec()方法都包含捕获分组的信息；设置全局标志后，match()方法不包含捕获分组的信息
```
var string = 'cat,bat,sat,fat';
var pattern = /(.)at/g;
var matches = string.match(pattern);
console.log(matches);//['cat', 'bat', 'sat', 'fat'] 
var exec = pattern.exec(string);
console.log(exec);//['cat','c'] 

var string = 'cat,bat,sat,fat';
var pattern = /(.)at/;
var matches = string.match(pattern);
console.log(matches);//['cat','c']  
var exec = pattern.exec(string);
console.log(exec);//['cat','c'] 
```

【tips】两种方法找出字符串中所有的数字

【1】用charAt()方法
```
var str1 = 'j1h342jg24g234j 3g24j1';
var array = [];
var temp = '';
for(var i = 0; i < str1.length; i++){
    var value = parseInt(str1.charAt(i));//如果用Number()将无法排除空格
    if(!isNaN(value)){
        temp += str1.charAt(i);
    }else{
        if(temp != ''){
            array.push(temp);
            temp = '';    
        }
    }
}
if(temp != ''){
    array.push(temp);
    temp = '';    
}
console.log(array);//["1", "342", "24", "234", "3", "24", "1"]
```

【2】用match()方法
```
var str1 = 'j1h342jg24g234j 3g24j1';
array = str1.match(/\d+/g);
console.log(array);//["1", "342", "24", "234", "3", "24", "1"]
```

【search()】  
search()方法接受一个正则或字符串的参数，返回匹配的内容在字符串中首次出现的位置，类似于不能设置起始位置的indexOf，找不到返回-1

[注意]search()方法不执行全局匹配，忽略全局标志g，也会忽略RegExp对象的lastIndex属性，总是从字符串的开始位置开始搜索

```
'x'.search(/y/);//-1
```

```
var string = 'cat,bat,sat,fat';
var pattern = /.at/;
var pos = string.search(pattern);
console.log(pos);//0

var string = 'cat,bat,sat,fat';
var pattern = /.at/g;
var pos = string.search(pattern);
console.log(pos);//0

var string = 'cat,bat,sat,fat';
var pattern = 'at';
var pos = string.search(pattern);
console.log(pos);//1
```

【replace()】  
replace()方法用于替换一个或多个子字符串。它接收两个参数：第一个是正则表达式或字符串，表示待查找的内容；第二个是字符串或函数，表示替换内容。返回替换后的字符串

【1】字符串替换，只能替换第一个子字符串
```
var string = 'cat,bat,sat,fat';
var result = string.replace('at','ond');
console.log(result);//'cond,bat,sat,fat'
```

【2】不设置全局标志g，也只能替换第一个子字符串
```
var string = 'cat,bat,sat,fat';
var result = string.replace(/at/,'ond');
console.log(result);//'cond,bat,sat,fat'
```

【3】设置全局标志g，替换所有匹配的子字符串
```
var string = 'cat,bat,sat,fat';
var result = string.replace(/at/g,'ond');
console.log(result);//'cond,bond,sond,fond'
```

与match()和seartch()方法相比，replace()方法更为强大，它可以在第二个参数中通过短属性名来使用某些正则表达式的静态属性
```
短属性名         说明
$&              最近一次的匹配项
$`              匹配项之前的文本
$'              匹配项之后的文本
$1,$2...        表示第N个匹配捕获组

var string = 'cat-bat-sat-fat';
console.log(string.replace(/(.)(at)/g,'$&'));//'cat-bat-sat-fat'
console.log(string.replace(/(.)(at)/g,'$`'));//'-cat--cat-bat--cat-bat-sat-'
console.log(string.replace(/(.)(at)/g,"$'"));//'-bat-sat-fat--sat-fat--fat-'
console.log(string.replace(/(.)(at)/g,'$1'));//'c-b-s-f'
console.log(string.replace(/(.)(at)/g,'$2'));//'at-at-at-at'
```

```
var string = '2016-06-24';
console.log(string.replace(/(\d{4})-(\d{2})-(\d{2})/g,'$2/$3/$1'));//'06/24/2016'
```

replace()方法的第二个参数可以是函数，这样文本的处理更加灵活

如果在只有一个匹配项的情况下，该方法会向这个函数传递3个参数：模式的匹配项、模式匹配项在字符串中的位置、原始字符串

```
var string = 'cat,bat,sat,fat';
var index = 0;
var matchArray = [];
var posArray = [];
var text = '';
var result = string.replace(/at/g,function(match,pos,originalText){
    matchArray.push(match);
    posArray.push(pos);
    text = originalText;
    index++;
    if(index % 2){
        return 'wow';
    }else{
        return '0';
    }
});
console.log(matchArray);//["at", "at", "at", "at"]
console.log(posArray);//[1, 5, 9, 13]
console.log(text);//'cat,bat,sat,fat'
console.log(result);//'cwow,b0,swow,f0'
```

如果正则表达式定义多个捕获组，则该方法传递给函数的参数依次是模式的匹配项、第一个捕获组的匹配项、第二个捕获组的匹配项……第N个捕获组的匹配项，但最后两个参数仍然分别是模式的匹配项在字符串中的位置和原始字符串，这个函数返回一个字符串

```
var string = 'cat,bat,sat,fat';
var index = 0;
var matchArray = [];
var m1Array = [];
var posArray = [];
var text = '';
var result = string.replace(/(.)at/g,function(match,m1,pos,originalText){
    matchArray.push(match);
    m1Array.push(m1);
    posArray.push(pos);
    text = originalText;
    return m1 + 'ta';
});
console.log(matchArray);//["cat", "bat", "sat", "fat"]
console.log(m1Array);//['c','b','s','f']
console.log(posArray);//[1, 5, 9, 13]
console.log(text);//'cat,bat,sat,fat'
console.log(result);//'cta,bta,sta,fta'
```

首字母大写
```
var text = 'one two three';
var result = text.replace(/\b(\w+)\b/g,function(match,m1,pos,originalText){
    return m1.charAt(0).toUpperCase()+m1.substring(1); 
})
console.log(result);
```

HTML标签转义
```
function htmlEscape(text){
    return text.replace(/[<>"&]/g,function(match,pos,originalText){
        switch(match){
            case '<':
            return '<';
            case '>':
            return '&';
            case '&':
            return '&amp;';
            case '\"':
            return '"';
        }
    });
}
console.log(htmlEscape('<p class=\"greeting\">Hello world!</p>'));
//<p class=" greeting"&Hello world!</p&
console.log(htmlEscape('<p class="greeting">Hello world!</p>'));
//同上
```

日期格式化
```
var array = ['2015.7.28','2015-7-28','2015/7/28','2015.7-28','2015-7.28','2015/7---28'];
function formatDate(date){
    return date.replace(/(\d+)\D+(\d+)\D+(\d+)/,'$1年$2月$3日')
}
var result = [];
for(var i = 0 ; i < array.length; i++){
    result.push(formatDate(array[i]));
}
console.log(result);//["2015年7月28日", "2015年7月28日", "2015年7月28日", "2015年7月28日", "2015年7月28日", "2015年7月28日"]    
```

找出重复项最多的字符和个数
```
var str = 'aaaaabbbbbdddddaaaaaaaffffffffffffffffffgggggcccccce';
var pattern = /(\w)\1+/g;
var maxLength = 0;
var maxValue = '';
var result = str.replace(pattern,function(match,match1,pos,originalText){
    if(match.length > maxLength){
        maxLength = match.length;
        maxValue = match1;
    }
})
console.log(maxLength,maxValue);//18 "f"
```

【split()】  
split()方法基于指定的分隔符将一个字符串分割成多个字符串，并将结果放在一个数组中，分隔符可以是字符串，也可以是一个RegExp

该方法可以接受第二个参数(可选)用于指定数组的大小，如果第二个参数为0-array.length范围内的值时按照指定参数输出，其他情况将所有结果都输出

若指定分隔符没有出现在字符串中，则以数组的形式返回原字符串的值

[注意]参数中的正则表达式是否使用全局标志g对结果没有影响

```
var colorText = 'red,blue,green,yellow';
console.log(colorText.split(''));//["r", "e", "d", ",", "b", "l", "u", "e", ",", "g", "r", "e", "e", "n", ",", "y", "e", "l", "l", "o", "w"]
console.log(colorText.split(','));//["red", "blue", "green", "yellow"]
console.log(colorText.split(',',2));//["red", "blue"]
console.log(colorText.split(',',6));//["red", "blue", "green", "yellow"]
console.log(colorText.split('-'));//["red,blue,green,yellow"]
console.log(colorText.split(/\,/));//["red", "blue", "green", "yellow"]
console.log(colorText.split(/e/));//["r", "d,blu", ",gr", "", "n,y", "llow"]
console.log(colorText.split(/[^\,]+/));//将除去逗号以外的字符串变为分隔符["", ",", ",", ",", ""],IE8-会识别为[",",",",","]
```

## 去除首尾空格
【trim()】  
ECMAScript5为所有字符串定义了trim()方法。这个方法会创建一个字符串的副本，删除前置及后缀的所有空白字符，然后返回结果

由于trim()方法返回的是字符串的副本，所以原始字符串中的前置及后缀空格会保持不变

```
var string = '    hello world   ';
console.log(string.trim());//'hello world'
console.log(string);//'    hello world   '
```

空白字符不仅仅包括空格，还包括制表符(\t)、换行符(\n)和回车符(\r)
```
var string = '    hello world   ';
console.log(string.trimRight());//'    hello world';
```

用正则表达式模拟trim()
```
function fnTrim(str){
    return str.replace(/^\s+|\s+$/,'')
}  
console.log(fnTrim('      hello world   '));//'hello world'
```
