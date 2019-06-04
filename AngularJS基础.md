## AngularJS是什么
> 完全使用js编写的客户端技术。同其他历史悠久的web技术(HTML、CSS)配合使用,使web应用开发比以往更简单、更快捷。AngularJS主要用于构建单页面Web应用。它通过增加开发人员和常见Web应用开发任务之间的抽象级别，使构建交互式的现代Web应用变得更加简单。

高级功能
- 解耦应用逻辑、数据模型和视图
- Ajax服务
- 依赖注入
- 浏览历史
- 测试

## 容易犯错的地方
【ng-model】  
AngularJS中的 ng-model 必须得放到对象里面，不然在流程控制语句里面会失效
```
<input ng-model="obj.name" />
```

## 常用概念集合

【指令】：将DOM元素增强为可复用的DOM组件的属性或元素

【值绑定】：模板语法`{{}}`可以将表达式绑定到视图上

【过滤器】：可以在视图中使用的函数，用来进行格式化

【表单控件】：用来检验用户输入的控件

【ng-app】  
ng-app属性声明所有被其包含的内容都属于这个AngularJS应用

【ng-controller】  
ng-controller DOM元素上的controller属性声明所有被它包含的元素都属于某个控制器

【$】  
Angular使用$预定义对象

【$rootScope】  
$rootScope是所有$scope对象的最上层

是AngularJS中最接近全局作用域的对象。在$rootScope上附加太多业务逻辑并不是好主意，这与污染JavaScript的全局作用域是一样的

【$scope】  
$scope对象：是一个树形结构的JavaScript对象，其中的属性可以被视图访问，也可以同控制器进行交互

$scope的所有属性，都可以自动被视图访问到

【angular.module()函数】  
angular.module('myApp',[]) 我们告诉Angular希望通过使用它来创建一个名为myApp的模块，该函数返回一个模块

【controller函数】  
定义了一个新的控制器。它接收两个参数  
第一个参数：定义了控制器的名称  
第二个参数：是一个函数定义，它定义了这个控制器将如何工作


## 常用指令
- ng-model 双向数据绑定指令

## 常用对象
【$scope】  
存储数据对象

【$timeout】  
定时器对象

## 最佳实践
【使用对象的属性进行数据绑定，优于使用对象进行数据绑定】

由于JavaScript自身的特点，以及它在传递值和引用时的不同处理方式，通常认为，在视图中通过对象的属性而非对象本身来进行引用绑定，是Angular中的最佳实践
```
<h1>Hello {{ clock.now }}!</h1> // 这样更好
<h1>Hello {{ clock }}!</h1> // 这样没有上面好
```

- 将控制器命名为[Name]Controller而不是[Name]Ctrl是一个最佳实践。

## AngularJS中的数据绑定
```
<!DOCTYPE html>
<html ng-app>
<head>
    <title>Hello World</title>

</head>
<body>
<input ng-model="name" type="text" placeholder="Your name"> <--1
<h1>Hello {{ name }}</h1>
<script src="../angular.js"></script>
</body>
</html>
```
1) 使用ng-modal指令将内部数据模型对象$scope中的name属性(property)绑定到了文本输入字段上

AngularJS则采用了完全不同的解决方案。它创建实时模板来代替视图，而不是将数据合并进模板之后更新DOM

AngularJS会记录数据模型所包含的数据在任何特定时间点的值（在Hello World例子中就是name的值），而不是原始值。

当AngularJS认为某个值可能发生变化时，它会运行自己的事件循环来检查这个值是否变“脏”。如果该值从上次事件循环运行之后发生了变化，则该值被认为是“脏”值。这也是Angular可以跟踪和响应应用变化的方式。

比如在游戏开发中就大量使用脏检查技术

## 模块
使用模块能给我们带来许多好处
- 保持全局命名空间的清洁
- 编写测试代码更容易，并能保持其清洁，以便更容易找到互相隔离的功能
- 易于在不同应用复用代码
- 使应用能够以任意顺序加载代码的各个部分

声明一个模块
```
angular.module('myApp', []);
```
参数1：模块名称  
参数2：依赖列表

** 这个方法相当于AngularJS模块的setter方法，是用来定义模块的。

angular.module('myApp') 这个方法相当于getter方法，用来获取对模块的引用


## 作用域
> 作用域(scope)是构成AngularJS应用的核心基础，作用域是视图和控制器之间的胶水。视图中的模板会和作用域进行连接，然后应用会对DOM进行设置以便将属性变化通知给AngularJS.

作用域是应用状态的基础。基于动态绑定，我们可以依赖视图在修改数据时立刻更新$scope，也可以依赖$scope在其发生变化时立刻重新渲染视图

AngularJS将$scope设计成和DOM类似的结构，因此$scope可以进行嵌套，也就是说我们可以引用父级$scope中的属性

$scope的所有属性，都可以自动被视图访问到

### $scope生命周期
【创建】  
> 在创建控制器或指令时，AngularJS会用$injector创建一个新的作用域，并在这个新建的控制器或指令运行时将作用域传递进去

【链接】  
当Angular开始运行时，所有的$scope对象都会附加或者链接到视图中。所有创建$scope对象的函数也会将自身附加到视图中。这些作用域将会注册当Angular应用上下文中发生变化时需要运行的函数

【更新】  
当事件循环运行时，它通常执行在顶层$scope对象上，每个子作用域都执行自己的脏值检测，每个监控函数都会检查变化。如果检测到任意变化，$scope对象就会触发指定的回调函数

【销毁】  
当一个$scope在视图中不再需要时，这个作用域将会清理和销毁自己


## 控制器
> 控制器在AngularJS中的作用是增强视图。AngularJS中的控制器是一个函数，用来向视图的作用域中添加额外的功能。我们用它来给作用域对象设置初始状态，并添加自定义行为

第一个控制器代码示例：
```
app.controller('FirstController', function($scope) { 
    $scope.counter = 0;
    $scope.add = function(amount) { $scope.counter += amount; }; 
    $scope.subtract = function(amount) { $scope.counter -= amount;  };
});
```

### 控制器嵌套(作用域包含作用域)
> AngularJS应用的任何一个部分，无论它渲染在哪个上下文中，都有父级作用域存在。对于ng-app所处的层级来讲，它的父级作用域就是$rootScope

**有一个例外：在指令内部创建的作用域被称作孤立作用域**
除了孤立作用域外，所有的作用域都通过原型继承而来，也就是说它们都可以访问父级作用域。

## 表达式
表达式和eval(javascript:;)非常相似，但是由于表达式由AngularJS来处理，它们有以下显著不同的特性：
- 所有的表达式都在其所属的作用域内部执行，并有访问本地$scope的权限
- 如果表达式发生了TypeError和ReferenceError并不会抛出异常
- 不允许使用任何流程控制功能(条件控制，例如if/else)但是可以使用三目
- 可以接受过滤器和过滤器链

```
{{ expression }}
```
当使用 `{{ }}` 表示法的时候，便设置了一个$watch。简而言之，这个$watch函数会监控$scope上的属性。当属性以任何方式改变时，它就会调用相应的函数。你也可以在$scope上的任意属性发生变化时用$watch函数直接执行某个自定义函数。

### 手动解析AngularJS表达式
> AngularJS通过$parse这个内部服务来进行表达式的运算，这个服务能够访问当前所处的作用域。这个过程允许我们访问定义在$scope上的原始JavaScript数据和函数。

代码展示：
```
<div ng-controller="MyController">
    <input ng-model="expr"
           type="text"
           placeholder="Enter an expression" />
    <h2>{{ parsedValue }}</h2>
</div>

angular.module("myApp", [])
.controller('MyController', function($scope,$parse) {
    $scope.$watch('expr', function(newVal, oldVal, scope) {
        console.log(scope);
        if (newVal !== oldVal) {
            // 用该表达式设置parseFun
            var parseFun = $parse('expr');
            // 获取经过解析后表达式的值
            $scope.parsedValue = parseFun(scope);
        }
    });
});
```

## 过滤器

基础使用
```
{{ name | uppercase }}
```

也可以通过$filter来调用过滤器
```
app.controller('DemoController', ['$scope', '$filter',
  function($scope, $filter) {

    $scope.name = $filter('lowercase')('Ari');
}]);
```

过滤器增加参数
```
<!-- 显示：123.46 -->
{{ 123.456789 | number:2 }}
```

【常用过滤器】  
currency:货币过滤器  
date:日期过滤  
filter：给定数组中选择一个子集，并将其生成一个新数组返回。经常在搜索中使用；  
同时也支持对象过滤  
也支持自定义函数过滤  
json：将一个JSON或JavaScript对象转换成字符串。这种转换对调试非常有帮助  
limitTo：限制数组或字符串的长度  
lowercase：转换为小写  
number：过滤器将数字格式化成文本。它的第二个参数是可选的，用来控制小数点后截取的位数  
orderBy：过滤器可以用表达式对指定的数组进行排序  
uppercase：字母转大写

【自定义过滤器】
```
angular.module('myApp.filters', [])
.filter('capitalize', function() {
  return function(input) {
    // input是我们传入的字符串
    if (input) {
      return input[0].toUpperCase() + input.slice(1);
    }
  }
});
```

## 表单验证（第七章）
```
<form name="form" novalidate>
  <label name="email">Your email</label>
  <input type="email"
        name="email"
        ng-model="email" placeholder="Email Address" />
</form>
```
novalidate：屏蔽浏览器对表单的默认验证行为

【必填项】
```
<input type="text" required />
```

【最小长度】
```
<input type="text" ng-minlength="5" />
```

【最大长度】
```
<input type="text" ng-maxlength="20" />
```

【模式匹配】匹配指定的正则表达式
```
<input type="text" ng-pattern="[a-zA-Z]" />
```

【电子邮件】
```
<input type="email" name="email" ng-model="user.email" />
```

【数字】
```
<input type="number" name="age" ng-model="user.age" />
```

【URL】
```
<input type="url" name="homepage" ng-model="user.facebook_url" />
```

### 在表单中控制变量
```
formName.inputFieldName.property : 访问表单字段属性

formName.inputFieldName.$pristine : 未修改的表单

formName.inputFieldName.$dirty : 修改过的表单

formName.inputFieldName.$valid : 合法的表单

formName.inputFieldName.$invalid : 不合法的表单
```
【错误】  
$error对象。它包含当前表单的所有验证内容，以及它们是否合法的信息
```
formName.inputfieldName.$error
```

### 表单css样式
AngularJS处理表单时，会根据表单当前的状态添加一些CSS类
```
.ng-pristine {}
.ng-dirty {}
.ng-valid {}
.ng-invalid {}
```

【$parsers】  
使用$parsers数组是实现自定义验证的途径之一

```
angular.module('myApp')
.directive('oneToTen', function() {
return {
 require: '?ngModel',
 link: function(scope, ele, attrs, ngModel) {
   if (!ngModel) return;
     ngModel.$parsers.unshift(
       function(viewValue) {
         var i = parseInt(viewValue);

         if (i >= 0 && i < 10) {
           ngModel.$setValidity('oneToTen', true);
           return viewValue;
         } else {
             ngModel.$setValidity('oneToTen', false);
             return undefined;
         }
     });
  }
};
});
```


## 指令 
第一个简单指令

HTML使用
```
<my-directive></my-directive>
```
指令代码
```
angular.module("myApp", [])
    .directive('myDirective',function () {
        return {
            restrict: 'E', // 元素（E）、属性（A）、类（C）或注释（M）
            replace: true, // 将自定义标签从DOM中移除
            template: '<a href="http://google.com"> Click me to go to Google</a>' // 自定义HTML
        }
    });
```

注意：  
指令命名应该遵循驼峰命名，HTML中可以羊肉串的写法


向指令中传递数据
```
<div my-directive
    my-url="http://google.com"
    my-link-text="Click me to go to Google">
</div>

angular.module("myApp", [])
    .directive('myDirective',function () {
        return {
            restrict: 'E', // 元素（E）、属性（A）、类（C）或注释（M）
            replace: true, // 将自定义标签从DOM中移除
            scope:{
                myUrl: '@', // 绑定策略
                myLinkText: '@' // 我们用属性将数据从DOM中复制到指令的隔离作用域
            },
            template: '<a href="{{ myUrl }}"> {{myLinkText}}</a>' // 自定义HTML
        }
    });
```

### 内置指令

【基础ng属性指令】
- ng-href 创建动态的URL
- ng-src
- ng-disabled 禁用，常用在表单上面
- ng-checked 是否选中
- ng-readonly 是否只读
- ng-selected option选中的值
- ng-class
- ng-style

【在指令中使用子作用域】  
ng-app和ng-controller是特殊的指令，因为它们会修改嵌套在它们内部的指令的作用域

ng-app为AngularJS应用创建$rootScope，ng-controller则会以$rootScope或另外一个ng-controller的作用域为原型创建新的子作用域

【ng-include】  
使用ng-include可以加载、编译并包含外部HTML片段到当前的应用中。

要记住，使用ng-include时AngularJS会自动创建一个子作用域。如果你想使用某个特定的作用域，例如ControllerA的作用域，必须在同一个DOM元素上添加ng-controller="ControllerA"指令，这样当模板加载完成后，不会像往常一样从外部作用域继承并创建一个新的子作用域

```
<div ng-include="'/myTemplateName.html'"
     ng-controller="MyController"
     ng-init="name = 'World'">
    Hello {{ name }}
</div>
```

【ng-view】  
用来设置将被路由管理和放置在HTML中的视图的位置

【ng-if】  
使用ng-if指令可以完全根据表达式的值在DOM中生成或移除一个元素。如果赋值给ng-if的表达式的值是false，那对应的元素将会从DOM中移除，否则对应元素的一个克隆将被重新插入DOM中。

【ng-repeat】  
ng-repeat用来遍历一个集合或为集合中的每个元素生成一个模板实例  
- $index：遍历的进度（0...length-1）。
- $first：当元素是遍历的第一个时值为true。
- $middle：当元素处于第一个和最后元素之间时值为true。
- $last：当元素是遍历的最后一个时值为true。
- $even：当$index值是偶数时值为true。
- $odd：当$index值是奇数时值为true。

代码示例
```
<ul ng-controller="PeopleController">
    <li ng-repeat="person in people" ng-class="{even: !$even, odd: !$odd}">
        {{person.name}} lives in {{person.city}}
    </li>
</ul>

.odd {
    background-color: blue;
}
.even {
    background-color: red;
}

angular.module('myApp',[])
.controller('PeopleController',function($scope) {
    $scope.people = [
        {name: "Ari", city: "San Francisco"},
        {name: "Erik", city: "Seattle"}
    ];
})
```

【ng-init】  
ng-init指令用来在指令被调用时设置内部作用域的初始状态。

【ng-bind】  
尽管可以在视图中使用`{{}}`模板语法，我们也可以通过ng-bind指令实现同样的行为

【ng-cloak】  
除使用ng-bind来避免未渲染元素闪烁，还可以在含有`{{}}`的元素上使用ng-cloak指令
```
<body ng-init="greeting='HelloWorld'">
    <p ng-cloak>{{ greeting }}</p>
</body>
```

【ng-bind-template】  
同ng-bind指令类似，ng-bind-template用来在视图中绑定多个表达式
```
<div ng-bind-template="{{message}}{{name}}">
</div>
```

【ng-model】  
ng-model指令用来将input、select、textarea或自定义表单控件同包含它们的作用域中的属性进行绑定。

【ng-show】【ng-hide】  
根据所给的表达式的值来显示或隐藏HTML元素。

【ng-change】  
这个指令会在表单输入发生变化时计算给定表达式的值。因为要处理表单输入，这个指令要和ngModel联合起来使用

```
<div ng-controller="EquationController">
    <input type="text"
        ng-model="equation.x"
        ng-change="change()" />
    <code>{{ equation.output }}</code>
</div>


angular.module('myApp',[])
.controller('EquationController',function($scope) {
    $scope.equation = {};
    $scope.change = function() {
        $scope.equation.output
            = parseInt($scope.equation.x) + 2;
    };
});
```

【ng-form】  
用来在一个表单内部嵌套另一个表单。普通的HTML `<form>`标签不允许嵌套，但ng-form可以

这意味着内部所有的子表单都合法时，外部的表单才会合法。这对于用ng-repeat动态创建表单是非常有用的

【ng-click】  
用来指定一个元素被点击时调用的方法或表达式

【ng-select】
```
<div ng-controller="CityController">
    <select ng-model="city"
    ng-options="city.name for city in cities">
        <option value="">Choose City</option>
    </select>
    Best City: {{ city.name }}
</div>

angular.module('myApp',[])
.controller('CityController',function($scope) {
    $scope.cities = [
      {name: 'Seattle'},
      {name: 'San Francisco'},
      {name: 'Chicago'},
      {name: 'New York'},
      {name: 'Boston'}
    ];
});
```
【ng-submit】  
ng-submit用来将表达式同onsubmit事件进行绑定。这个指令同时会阻止默认行为（发送请求并重新加载页面），但前提是表单不含有action属性。

【ng-class】  
使用ng-class 动态设置元素的类
```
<div ng-controller="LotteryController">
    <div ng-class="{red: x > 5}"
        ng-if="x > 5"> 
        You won!
    </div>
    <button ng-click="x = generateNumber()"
        ng-init="x = 0">
        Draw Number
    </button>
    <p>Number is: {{ x }}</p>
</div>

.red {
    background-color: red;
}

angular.module('myApp',[])
.controller('LotteryController', function($scope) {
    $scope.generateNumber = function() {
        return Math.floor((Math.random()*10)+1);
    };
});
```

### 自定义指令
基础框架
```
angular.module('myApp', [])
.directive('myDirective', function ($timeout, UserDefinedService) {
    // 指令定义放在这里
    return {
        // 通过设置项来定义指令
    }
});
```
myDirective ： 指令名字  
这个函数返回一个对象，其中定义了指令的全部行为。$compile服务利用这个方法返回的对象，在DOM调用指令时来构造指令的行为。

定义一个指令会使用到的全部设置
```
angular.module('myApp', [])
.directive('myDirective', function() {
    return {
        restrict: String, // 元素还是属性 E（元素）A（属性，默认值）C（类名）M（注释）可以复选‘EA’
        priority: Number, // 优先级（如果一个元素上具有两个优先级相同的指令，声明在前面的那个会被优先调用，如果设置了优先级的话，则优先级高的先执行） 默认值0
        terminal: Boolean, // 这个参数用来告诉AngularJS停止运行当前元素上比本指令优先级低的指令。但同当前指令优先级相同的指令还是会被执行。
        template: String or Template Function: 模板
        function(tElement, tAttrs) (...},
        templateUrl: String, // 一个代表外部HTML文件路径的字符串
        replace: Boolean or String, // 将自定义标签从DOM中移除 
        scope: Boolean or Object, // 当scope设置为true时，会从父作用域继承并创建一个新的作用域对象。传一个{}空对象的话就会创建一个隔离作用域
        transclude: Boolean,
        controller: String or function(scope, element, attrs, transclude, otherInjectables) { ... },  // controller参数可以是一个字符串或一个函数。当设置为字符串时，会以字符串的值为名字，来查找注册在应用中的控制器的构造函数.或者包含一个内联控制器
        controllerAs: String, // 控制器别名，这个参数看起来好像没什么大用，但它给了我们可以在路由和指令中创建匿名控制器的强大能力。这种能力可以将动态的对象创建成为控制器，并且这个对象是隔离的、易于测试的。
        require: String,
        link: function(scope, iElement, iAttrs) { ... }, // 链接函数负责将作用域和DOM进行链接。用link函数创建可以操作DOM的指令。
        compile: // 返回一个对象或连接函数，如下所示： // 编译函数负责对模板DOM进行转换。
            function(tElement, tAttrs, transclude) {
                return {
                    pre: function(scope, iElement, iAttrs, controller) { ... },
                    post: function(scope, iElement, iAttrs, controller) { ... }
                }
                // 或者
                return function postLink(...) { ... }
            }
    };
});
```

### 指令绑定策略
【本地作用域属性@ (or @attr)】  
使用@符号将本地作用域同DOM属性的值进行绑定。指令内部作用域可以使用外部作用域的变量

【双向绑定= (or =attr)】  
通过=可以将本地作用域上的属性同父级作用域上的属性进行双向的数据绑定。

【父级作用域绑定& (or &attr)】  
通过&符号可以对父级作用域进行绑定，以便在其中运行函数。意味着对这个值进行设置时会生成一个指向父级作用域的包装函数。

```
scope: {
    ngModel: '=', // 将ngModel同指定对象绑定
    onSend: '&',  // 将引用传递给这个方法
    fromName: '@' // 储存与fromName相关联的字符串
}
```

### 指令内部controller

controller参数可以是一个字符串或一个函数。当设置为字符串时，会以字符串的值为名字，来查找注册在应用中的控制器的构造函数
```
angular.module('myApp', [])
.directive('myDirective',  function() {
    restrict: 'A', // 始终需要
    controller: 'SomeController'
})

// 应用中其他的地方，可以是同一个文件或被index.html包含的另一个文件
angular.module('myApp')
.controller('SomeController', function($scope, $element, $attrs, $transclude) {
    // 控制器逻辑放在这里
});
```

可以在指令内部通过匿名构造函数的方式来定义一个内联的控制器：
```
angular.module('myApp',[])
.directive('myDirective', function() {
    restrict: 'A',
    controller: function($scope, $element, $attrs, $transclude) {
        // 控制器逻辑放在这里
    }
});

```
我们可以将任意可以被注入的AngularJS服务传递给控制器。

【$scope】  
与指令元素相关联的当前作用域

【$element】  
当前指令对应的元素

【$attrs】  
由当前元素的属性组成的对象，例如下面的元素
```
<div id="aDiv" class="box"></div>

具有如下的属性对象：
{
    id:"aDiv",
    class:"box"
}
```
【$transclude】  
嵌入链接函数会与对应的嵌入作用域进行预绑定。  
transclude链接函数式实际被执行作用来克隆元素和操纵DOM函数

```
angular.module('myApp')
.directive('link', function() {
    return {
        restrict: 'EA',
        transclude: true,
        controller: function($scope, $element, $transclude, $log) {
            $transclude(function(clone) {
                var a = angular.element('<a>');
                a.attr('href', clone.text());
                a.text(clone.text());
                $log.info("Created new a tag in link directive");
                $element.append(a);
            });
        }
    };
});
```
指令的控制器和link函数可以进行互换。控制器主要是用来提供可在指令间复用的行为，但链接函数只能在当前内部指令中定义行为，且无法在指令间复用。

由于指令可以require其他指令所使用的控制器，因此控制器常被用来放置在多个指令间共享的动作

如果我们希望将当前指令的API暴露给其他指令使用，可以使用controller参数，否则可以使用link来构造当前指令元素的功能性。如果我们使用了scope.$watch()或者想要与DOM元素做实时的交互，使用链接会是更好的选择。


【require】

require参数可以被设置为字符串或数组，字符串代表另外一个指令的名字。require会将控制器注入到其值所指定的指令中，并作为当前指令的链接函数的第四个参数。

字符串或数组元素的值是会在当前指令的作用域中使用的指令名称

指令定义只会查找定义在指令当前用域中的ng-model=""。
```
//...
restrict: 'EA',
require: 'ngModel'
//...

<!-- 指令会在本地作用域查找ng-model -->
<div my-directive ng-model="object"></div>

```
require参数的值可以用下面的前缀进行修饰，这会改变查找控制器时的行为：

？如果在当前指令中没有找到所需要的控制器，会将null作为传给link函数的第四个参数。

^ 如果添加了^前缀，指令会在上游的指令链中查找require参数所指定的控制器。

?^ 将前面两个选项的行为组合起来，我们可选择地加载需要的指令并在父指令链中进行查找。

没有前缀 如果没有前缀，指令将会在自身所提供的控制器中进行查找，如果没有找到任何控制器（或具有指定名字的指令）就抛出一个错误


【link】
```
// require 'SomeController',
link: function(scope, element, attrs, SomeController) {
    // 在这里操作DOM，并且可以访问依赖指令的控制器
}
```
如果指令定义中有require选项，函数签名中会有第四个参数，代表控制器或者所依赖的指令的控制器。

如果require选项提供了一个指令数组，第四个参数会是一个由每个指令所对应的控制器组成的数组。

scope 指令用来在其内部注册监听器的作用域

iElement iElement参数代表实例元素，指使用此指令的元素。在postLink函数中我们应该只操作此元素的子元素，因为子元素已经被链接过了。

iAttrs iAttrs参数代表实例属性，是一个由定义在元素上的属性组成的标准化列表，可以在所有指令的链接函数间共享。会以JavaScript对象的形式进行传递。

controller controller参数指向require选项定义的控制器。如果没有设置require选项，那么controller参数的值为undefined。


【自定义验证器】
```
angular.module('validationExample', [])
.directive('ensureUnique',function($http) {
    return {
        require: 'ngModel',
        link: function(scope, ele, attrs, c) {
            scope.$watch(attrs.ngModel, function() {
                $http({
                    method: 'POST',
                    url: '/api/check/' + attrs.ensureUnique,
                    data: {
                    field': attrs.ensureUnique
                }).success(function(data,status,headers,cfg) {
                    c.$setValidity('unique', data.isUnique);
                }).error(function(data,status,headers,cfg) {
                    c.$setValidity('unique', false);
                });
            });
        }
    };
});
```

## AngularJS模块加载
AngularJS模块可以在被加载和执行之前对其自身进行配置。我们可以在应用的加载阶段应用不同的逻辑组。

【配置】可以使用来配置路由  
在模块的加载阶段，AngularJS会在提供者注册和配置的过程中对模块进行配置。在整个AngularJS的工作流中，这个阶段是唯一能够在应用启动前进行修改的部分。
```
angular.module('myApp', [])
  .config(function($provide) {
  });
```

当对模块进行配置时，需要格外注意只有少数几种类型的对象可以被注入到config()函数中：提供者和常量。如果我们将一个服务注入进去，会在真正对其进行配置之前就意外地把服务实例化了。

这种对配置服务进行严格限制的另外一个副作用就是，我们只能注入用provider()语法构建的服务，其他的则不行。

【运行块】  
和配置块不同，运行块在注入器创建之后被执行,它是所有AngularJS应用中第一个被执行的方法

运行块是AngularJS中与main方法最接近的概念。

运行块通常用来注册全局的事件监听器。例如，我们会在.run()块中设置路由事件的监听器以及过滤未经授权的请求。

假设我们需要在每次路由发生变化时，都执行一个函数来验证用户的权限，放置这个功能唯一合理的地方就是run方法：
```
angular.module('myApp', ['ngRoute'])
.run(function($rootScope, AuthService) {
    $rootScope.$on('$routeChangeStart', function(evt, next, current) {
        // 如果用户未登录
        if (!AuthService.userLoggedIn()) {
            if (next.templateUrl === "login.html") {
                // 已经转向登录路由因此无需重定向
            } else {
                $location.path('/login');
            }
        }
    });
});

```


## 依赖注入
一个对象通常有三种方式可以获得对其依赖的控制权

1. 在内部创建依赖
2. 通过全局变量进行引用
3. 在需要的地方通过参数进行传递

依赖注入是通过第三种方式实现的。依赖注入是一种设计模式，它可以去除对依赖关系的硬编码，从而可以在运行时改变甚至移除依赖关系。

AngularJS使用$injetor（注入器服务）来管理依赖关系的查询和实例化。事实上，$injetor负责实例化AngularJS中所有的组件，包括应用的模块、指令和控制器等。

依赖注入简单示例：  
1、声明一个模块和一个控制器
```
angular.module('myApp', [])
.factory('greeter', function() {
    return {
        greet: function(msg) {alert(msg);}
    }
})
.controller('MyController',
    function($scope, greeter) {
        $scope.sayHello = function() {
            greeter.greet("Hello!");
        };
});
```

2、当AngularJS实例化这个模块时，会查找greeter并自然而然地把对它的引用传递进去
```
<div ng-app="myApp">
    <div ng-controller="MyController">
        <button ng-click="sayHello()">Hello</button>
    </div>
</div>
```

3、内部的实现流程
```
// 使用注入器加载应用
var injector = angular.injector(['ng', 'myApp']);
// 通过注入器加载$controller服务：var $controller = injector.get('$controller');
var scope = injector.get('$rootScope').$new(); 
// 加载控制器并传入一个作用域，同AngularJS在运行时做的一样
var MyController = $controller('MyController', {$scope: scope})
```
上面的代码中并没有说明是如何找到greeter的，但是它的确能正常工作，因为$injector会负责为我们查找并加载它。


【显式注入声明】
```
var aControllerFactory =
function aController($scope, greeter) {
    console.log("LOADED controller", greeter);
    // ... 控制器
};
aControllerFactory.$inject = ['$scope', 'greeter']; 
// Greeter服务
var greeterService = function () {
  console.log("greeter service");
}
// 我们应用的控制器
angular.module('myApp', [])
  .controller('MyController', aControllerFactory)
  .factory('greeter', greeterService);
// 获取注入器并创建一个新的作用域
var injector = angular.injector(['ng', 'myApp']),
    controller = injector.get('$controller'),
    rootScope = injector.get('$rootScope'),
    newScope = rootScope.$new();
// 调用控制器
controller('MyController', {$scope: newScope});
```

【行内注入声明】
```
angular.module('myApp')
.controller('MyController', ['$scope', 'greeter', function($scope, greeter) {
}]);

```


## 服务
服务提供了一种能在应用的整个生命周期内保持数据的方法，它能够在控制器之间进行通信，并且能保证数据的一致性。

服务是一个单例对象，在每个应用中只会被实例化一次(被$injector实例化)并且是延迟加载的(需要时才会被创建)

以AngularJS的$http服务为例，它提供了对浏览器的XMLHttpRequest对象的底层访问功能，我们可以通过$http的API同XMLHttpRequest进行交互，而不需要因为调用这些底层代码而污染应用

```
// 示例服务，在应用的整个生命周期内保存current_user
angular.module('myApp', [])
.factory('UserService', function($http) {
    var current_user;

    return {
        getCurrentUser: function() {
            return current_user;
        },
        setCurrentUser: function(user) {
            current_user = user;
        }
    };
});
```

【注册一个服务】
使用angular.module的factory API创建服务，是最常见也是最灵活的方式：
```
angular.module('myApp.services', [])
.factory('githubService', function() {
    var serviceInstance = {}; 
    // 我们的第一个服务，返回serviceInstance;
});
```

编写第一个服务应用
```
angular.module('myApp.services', [])
.factory('githubService', function($http) {
    var githubUrl = 'https://api.github.com';

    var runUserRequest = function(username, path) {
        // 从使用JSONP调用 Github API的$http服务中返回promise
        return $http({
            method: 'JSONP',
            url: githubUrl + '/users/' +
                  username + '/' +
                  path + '?callback=JSON_CALLBACK'
        });
    };
    // 返回一个events函数的服务对象
    return {
        events: function(username) {
            return runUserRequest(username, 'events');
        }
    };
});
```


在应用中使用服务
```
angular.module('myApp', ['myApp.services'])
.controller('ServiceController', function($scope, githubService) {
    // 可以在对象上调用events函数
    $scope.events = githubService.events('auser');
});
```


【创建服务的设置项】  
在AngularJS应用中，factory()方法是用来注册服务的最常规方式，同时还有其他一些API可以在特定情况下帮助我们减少代码量。

共有5种方法用来创建服务：

- factory()
- service()
- constant()
- value()
- provider()

【service】  
使用service()可以注册一个支持构造函数的服务，它允许我们为服务对象注册一个构造函数。

service()方法接受两个参数。

- name（字符串）要注册的服务名称。
- constructor（函数）

```
var Person = function($http) {
    this.getName = function() {
        return $http({ method: 'GET', url: '/api/user'});
     };
};
angular.service('personService', Person);
```

【constant】  
可以将一个已经存在的变量值注册为服务，并将其注入到应用的其他部分当中。例如，假设我们需要给后端服务一个apiKey，可以用constant()将其当作常量保存下来。

```
angular.module('myApp') .constant('apiKey','123123123')

angular.module('myApp')
.controller('MyController', function($scope, apiKey) {
    // 可以像上面一样用apiKey作为常量
    //用123123123作为字符串的值
    $scope.apiKey = apiKey;
});

```

【value】  
如果服务的$get方法返回的是一个常量，那就没要必要定义一个包含复杂功能的完整服务，可以通过value()函数方便地注册服务。

```
angular.module('myApp')
.value('apiKey','123123123');
```

【value()和constant()该用谁】  
value()方法和constant()方法之间最主要的区别是，常量可以注入到配置函数中，而值不行。

```
angular.module('myApp', [])
.constant('apiKey', '123123123')
.config(function(apiKey) {
    // 在这里apiKey将被赋值为123123123
    //就像上面设置的那样
})
.value('FBid','231231231')
.config(function(FBid) {
    // 这将抛出一个错误，未知的provider: FBid
    // 因为在config函数内部无法访问这个值
});
```

【decorator】  
$provide服务提供了在服务实例创建时对其进行拦截的功能，可以对服务进行扩展，或者用另外的内容完全代替它。

装饰器是非常强大的，它不仅可以应用在我们自己的服务上，也可以对AngularJS的核心服务进行拦截、中断甚至替换功能的操作。事实上AngularJS中很多功能的测试就是借助$provide.decorator()建立的。

例如，我们想给之前定义的githubService服务加入日志功能，可以借助decorator()函数方便地实现这个功能，而不需要对原始的服务进行修改。

```
var githubDecorator = function($delegate,$log) {
    var events = function(path) {
        var startedAt = new Date();
        var events = $delegate.events(path); 
            // 事件是一个promise
            events.finally(function() {
            $log.info("Fetching events" +
                " took " +
                (new Date() - startedAt) + "ms");
         });
         return events;
    };

    return {
        events: events
    };
};

angular.module('myApp')
.config(function($provide) {
    $provide.decorator('githubService',githubDecorator); //对服务进行拦截
});
```

## 事件
Angular应用也可以响应Angular事件。这使我们可以在应用中嵌套的各组件之间进行通信，即使这些组件在创建时并未考虑到其他组件。
> 注意，Angular事件系统并不与浏览器的事件系统相通，这意味着，我们只能在作用域上监听Angular事件而不是DOM事件。

【事件传播】  
> 因为作用域是有层次的，所以我们可以在作用域链上传递事件。

使用$emit来冒泡事件
```
// 发送一个事件
//我们的用户以当前user登录了
scope.$emit('user:logged_in', scope.user);
```

使用$broadcast向下传递事件
```
// 等等，购物车去结账了
// 当购物车在结账的时候
// 下面所有的指令都应当禁用自己
scope.$broadcast('cart:checking_out', scope.cart);
```

在$broadcast()方法上，每个注册了监听器的子作用域都会收到这个信息。事件传播到所有的指令和当前作用域的间接作用域上，并且一路往下调用每个监听器。

事件的监听
> 要监听一个事件，我们可以使用$on()方法。这个方法为具有某个特定名称的事件注册了一个监听器。事件名称就是在Angular中触发的事件类型。

```
scope.$on('$routeChangeStart',
    function(evt, next, current) {
        // 一个新的路由被触发了
});
```

完整代码实例：

```
<body ng-app="myApp">
    <div ng-controller="fatherCtrl">
        <input type="text" ng-model="name" ng-change="nameOnChange()">
        <h1>Father: {{name}}!</h1>
        <div ng-controller="sonCtrl">
             <input type="text" ng-model="name" ng-change="nameOnChange()"> 
             <h1>Son: {{name}}</h1>
        </div>
    </div>
</body>

var app = angular.module ('myApp', []);
app.controller('fatherCtrl', function ($scope) {
    $scope.name = "father";
    $scope.$on ('test', function(e, newName) {
        $scope.name = newName;
    });
});
app.controller('sonCtrl', function ($scope) {
    $scope.name = "son";
    $scope.nameOnChange = function () {
        $scope.$emit ('test', $scope.name);
    }
});

var app = angular.module ('myApp', []);
app.controller('fatherCtrl', function ($scope) {
    $scope.name = "father";
    $scope.nameOnChange = function () {
        $scope.$broadcast ('test', $scope.name);
    }
});
app.controller('sonCtrl', function ($scope) {
    $scope.name = "son";
    $scope.$on ('test', function(e, newName) {
        $scope.name = newName;
    });
    $scope.nameOnChange = function () {
        // $scope.$emit ('test', $scope.name);
    }
});
```
