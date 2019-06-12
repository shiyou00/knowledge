## 前言
上一节我们在angular2+项目中初步的使用了ui-router，也算是会用了吧，今天这节主要来理解下ui-router的几个重要概念

## States
ui-router 提供基于状态的路由,应用程序的每个特性都定义为一种状态,一个状态在任何时候都是活跃的，ui-router管理状态之间的转换。

每个状态等于一个URL，在进入URL之前，UI-Router首先获取任何先决条件例如：身份验证，所需数据(异步)，然后激活视图并更新URL。

路由器状态是分层的，状态可以嵌套在其他状态中，形成树状结构。子状态可以从父状态继承数据和行为(例如身份验证)

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


## Urls

## Parameters


## Resolve Data


## Transitions
