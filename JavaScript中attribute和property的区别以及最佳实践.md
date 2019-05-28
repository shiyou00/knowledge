## 前言
使用JavaScript操作DOM元素时往往涉及到两个概念：attribute 和 property。 document.getElementById('test').getAttribute('id') 、 $('#test').attr('id') 、 document.getElementById('test').id 和 $('#test').prop('id') 都能返回正确的id："test"。这篇文章主要介绍一下 property 和 attribute 的区别以及如何使用。

## Attribute
1、Attribute由HTML来定义，并不存在于DOM中，即：只要是HTML标签内定义的都是attribute。
```
<div id="test" class="button" custom-attr="1"></div>
document.getElementById('test').attributes;
// 返回：[custom-attr="hello", class="button", id="test"]
```

2、Attribute是String类型。对于上面的div，document.getElementById('test').getAttribute('custom-attr') 或 $('#test').attr('custom-attr') 都会返回string: "1"。

## Property
1、Property属于DOM，DOM的本质就是JavaScript中的一个object。我们可以像操作普通object一样读取、设置property，property可以是任意类型。
```
document.getElementById('test').foo = 1;
// 设置property：foo为number: 1
document.getElementById('test').foo;
// 读取property，返回number：1
$('#test').prop('foo');
// jQuery读取property，返回number：1

$('#test').prop('foo', {
    age: 23,
    name: 'John'
});
// jQuery设置property：foo为一个object
document.getElementById('test').foo.age;
// 返回number：23
document.getElementById('test').foo.name;
// 返回string："John"
```

2、非自定义attribute，如id、class、titile等，都会有对应的property映射。
```
<div id="test" class="button" foo="1"></div>

document.getElementById('test').id; 
// 返回string："test"
document.getElementById('test').className;
// 返回string："button"
document.getElementById('test').foo;
// 返回undefined，因为foo是自定义attribute
```
**注：由于 class 为JavaScript的保留关键字，所以通过property操作class时应使用 className。**

3、非自定义的property或attribute的变化多数是联动的。
```
var div = document.getElementById('test');
div.className = 'red-input';
div.getAttribute('class');
// 返回string："red-input"
div.setAttribute('class','green-input');
div.className;
// 返回string："green-input"
```

4、带有默认值的attribute不随property变化而变化。
```
<input id="search" value="foo" />

var input = document.getElementById('search');
input.value = 'foo2';
input.getAttribute('value');
// 返回string："foo"
```

## 最佳实践
使用JavaScript操作property更为方便、快捷，并且property支持各种不同的类型，尤其是对于布尔类型的attribute的自动转换，如：checked、disabled、selected等。

```
<input id="test" class="blue" type="radio" />
```

推荐用法
```
// 获取id
document.getElementById('test').id;
// 更改class
document.getElementById('test').className = 'red';
// 获取、设置状态
document.getElementById('test').checked;
document.getElementById('test').checked = true;
$('#test').prop('checked');
$('#test').prop('checked', true);
```

不推荐的用法
```
// 获取id
document.getElementById('test').getAttribute('id');
// 更改class
document.getElementById('test').setAttribute('class', 'red');
```