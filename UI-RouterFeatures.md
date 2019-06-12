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

## Parameters

## Resolve Data

## Transitions
