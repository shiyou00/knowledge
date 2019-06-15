## 前言
上一节我们在angular2+项目中初步的使用了ui-router，也算是会用了吧，今天这节主要来理解下ui-router的几个重要概念

## States
ui-router提供的路由其实就是一个一个States状态对象组成的。ui-router管理这些状态之间的切换以及生命周期

每个状态等于一个URL，在进入URL之前，UI-Router首先获取任何先决条件例如：身份验证，所需数据(异步)，然后激活视图并更新URL。

### State对象
```
const helloState = {
  name: 'hello', // 状态的名称，可以直接使用状态名字找到该状态，相当于状态的id
  url: '/hello', // 浏览器访问的url
  component: HelloComponent, // 展示的组件
  params:{}, // 参数对象
  resolve:[] // 状态所需的实际数据(通常使用参数值从后端异步获取)
};
```

### 嵌套状态
与传统状态机中的状态不同，UI-Router状态可以嵌套在彼此内部。一个父状态可以有多个子状态，形成一个状态树。

[注意]子状态的视图通常在父状态创建的视图端口中呈现。这称为嵌套视图

## Views
状态使用视图，视图是一个UI组件，当状态被激活时，它被放置到一个viewport (`<ui-view>`)中。

视图可以嵌套在其他视图中。父状态的视图可以创建一个viewport (`<ui-view>`)，而嵌套状态可以在激活时用自己的视图填充该viewport。

上篇文章中，有一段代码：
```
<a uiSref="hello" uiSrefActive="active">Hello</a>
<a uiSref="about" uiSrefActive="active">About</a>

<ui-view></ui-view>
```
当点击哪个路由，该路由对应的组件则映射到`<ui-view></ui-view>`中，我们在界面就可以看到该组件了。

### 添加一个嵌套路由
代码都是接着上篇文章的代码

路由文件定义:
```
const parentState = {
  name: 'folderlist',
  url: '/folder',
  component: FolderListComponent
};

const childState = {
  name: 'folderlist.messagelist',
  url: '/message',
  component: MessageListComponent
};

const childTwoState = {
  name: 'folderlist.foolist',
  url: '/foo',
  component: FooListComponent
};
```

FolderListComponent定义
```
<div>
  <a uiSref="folderlist.messagelist" uiSrefActive="active" style="margin-right:20px;">messagelist</a>
  <a uiSref="folderlist.foolist" uiSrefActive="active">foolist</a>
  <ui-view></ui-view>
</div>
```

这样两个子路由就会去它的父路由组件中取查找`<ui-view></ui-view>`；

[完整代码](https://github.com/shiyou00/angular-ui-router)

## Urls
state中定义的URL实际上只是一个URL片段。  
每个状态只定义它自己的那部分。如果是嵌套路由时，会先读取父路由拼接在一起的。  

上面代码中如果想访问`childTwoState`这个状态，实际上访问的路由地址是：`/folder/foo`

## Parameters
在一个状态中，或者说一个具体的URL中我们经常是需要使用参数的。

基本写法：
```
const childTwoState = {
  url: '/foo/{fooId}', 第一种写法
  url: '/foo?fooId' 第二种写法
};
```

但是这样写也是不够的，我们还需要对参数项做一些配置
```
const childTwoState = {
  name: 'folderlist.foolist',
  url: '/foo?fooId',
  component: FooListComponent,
  params: {
      fooId: {
          type: 'string',
          value: "aabbcc", // <-- Default value aabbcc
          squash: true, // 压缩模式:省略URL中的默认参数值
      },
  }
};
```
type类型: string、int、bool、Date、json。  
value: 设置默认值  
squash: true/false,压缩模式:省略URL中的默认参数值  

参数的配置项是非常重要的，后续会详细讲解的。

## Resolve Data
简单的理解：    
当路由进入一个组件时，通过resolve可以实现准备好这个路由所需的数据；

当一个resolve请求的后台接口报错例如：401，404，500等错误时，将调用错误的钩子拒绝进入该路由组件；

当然一个组件可以同时注入多个resolve的；  

解析resolve的过程是异步的。如果一个resolve返回一个Promise，则将暂停转换，直到该Promise得到解决。因此，resolve数据参与转换生命周期。  

[重点]resolve系统实际上是一个异步的、分层的依赖注入系统。

```
var state = {
  name: 'user',
  url: '/user/:userId
  resolve: [
    {
      token: 'user',
      policy: { when: 'EAGER' }, // 
      deps: ['UserService', Transition],
      resolveFn: (userSvc, trans) => userSvc.fetchUser(trans.params().userId) },
    }
  ]
}
```
代码解析：   
token: 注入到组件数据的名字，在组件中可以通过 @Input() user的方式注入该resolve  
policy: 定义何时获取到 resolve 数据，`EAGER`(进入到组件前) 获取 `LAZY`(进入到组件后)    
deps: 该resolve依赖的服务  
resolveFn: resolve函数主体，通过它来获取后台返回的请求数据  

## Transitions
路由切换：一个状态(URL)切换到另外一个状态(URL)

声明周期：
    - before：跳转开始之前
    - start：跳转开始
    - exit：退出状态
    - retain：状态被保留(状态是活动的，既不退出也不进入)
    - enter：进入状态
    - finish：完成
    - success/error：跳转完成后的状态
    
Transition 生命周期挂钩：
    - 钩子可以在转换生命周期的任何阶段注册
    - 钩子可以改变转换

## 小结
通过简单了解关于ui-router的一些基本概念，我们能明白，一个url就是一个state状态对象，一个state状态对象包含：视图、url、参数、resolve等属性。当状态之间跳转的时候，又要经历一些生命周期以及触发一些钩子。在接下来的文章我会对每一个部分进行详细讲解和学习，并应用到实战项目中，加深对ui-router的理解。
