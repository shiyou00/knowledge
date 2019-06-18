## 前言
回顾上一节的内容，我们大致学习了ui-router有几个核心概念：`State`、`Urls`、`Parameters`、`Resolve`、`Transitions`。今天这篇文章主要讲解下State对象

## State对象
> ui-router提供的路由其实就是一个一个States状态对象组成的。ui-router管理这些状态之间的切换以及生命周期

> 每个状态等于一个URL，在进入URL之前，UI-Router首先获取任何先决条件例如：身份验证，所需数据(异步)，然后激活视图并更新URL。

一个基础的state对象
```
const helloState = {
  name: 'hello',
  url: '/hello',
  component: HelloComponent,
  params:{},
  resolve:[]
};
```

### name

【普通访问】  
这个state的名字，我们在组件中可以通过state的name属性访问到该路由
```
<a uiSref="hello" uiSrefActive="active">Hello</a>
```

【嵌套访问】
路由在正常的项目中一般都是多级嵌套的一个树形结构的。那么我们如何定义和访问一个嵌套路由呢？

定义一个嵌套路由
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
```

访问的话还是如上所诉，直接访问路由的name即可，

### url
在嵌套路由的访问中应该是要把父级路由拼接起来的，例如访问childState这路由的访问地址就是`http://localhost:4200/#/folder/message`

常见url的形式
```
url: "/home"
url: "/users/:userid"
url: "/books/{bookid:[a-zA-Z_-]}"
url: "/books/{categoryid:int}"
url: "/books/{publishername:string}/{categoryid:int}"
url: "/messages?before&after"
url: "/messages?{before:date}&{after:date}"
url: "/messages/:mailboxid?{before:date}&{after:date}"
```

### params
路由访问所需的参数

```
const aboutState = {
  name: 'about',
  url: '/about?id',
  component: AboutComponent,
  onEnter: function(trans, state) {
    console.log("Entering " , state , trans);
  },
  onExit: function(trans, state) {
    console.log("Leaving " + state.name);
  },
  onRetain: function(trans, state) {
    console.log(state.name + " is still active!");
  },
  params: {
    id: {
      value: 'aabcc123' // 设置默认的参数值
    }
  }
};
```

由于参数设置选项比较多，我们将在下一篇文章中详细讲解下params对象

### resolve
resolve获取到的Data数据，可以用于当前视图 或 `transition hooks`，或用于该state对象其它的resolve。或者用于该state嵌套的state对象里面resolve。

因为resolve函数可以返回一个promise，所以路由器会延迟进入状态，直到promise就绪。如果任何一个承诺被拒绝，转换就会因为一个错误而中止。

【ResolvableLiteral】
一个resolve必须是一个ResolvableLiteral对象

```
export interface ResolvableLiteral {
    token: any; // 依赖注入的token
    resolveFn: Function; // 解析系统将在调用此函数之前异步获取依赖项
    policy?: ResolvePolicy; // policy定义何时调用解析，以及是否等待async并打开数据
    deps?: any[]; // 依赖项数组
    data?: any; // 预处理数据
}
```

【普通使用】  

新建一个 getListResolve.ts 文件
```
import { ResolvableLiteral, Transition } from "@uirouter/core";
export async function resolveFn(
  trans: Transition,
) {
  return [
    {
      id: "123",
      name: "jack"
    },
    {
      id: "234",
      name: "joke"
    }
  ]
}
export const getListResolver: ResolvableLiteral = {
  token: "listData",
  deps: [Transition],
  resolveFn
};
```

在路由中引入：  
```
const childState = {
  name: 'messagelist',
  parent:'folderlist',
  url: '/message',
  component: MessageListComponent,
  resolve:[getListResolver]
};
```

然后到`MessageListComponent`组件中添加DI token
```
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'message-list',
  templateUrl: './message-list.component.html',
  styleUrls: ['./message-list.component.css']
})
export class MessageListComponent implements OnInit {

  @Input() listData; // 这样就把数据注入进来了

  constructor() { }
  ngOnInit() {
    console.log(this.listData);
  }
}
```

【用于其它resolve】  
新建一个resolve: `getMessageResolver`
```
import { ResolvableLiteral, Transition } from "@uirouter/core";
export async function resolveFn(
  trans: Transition,
  listData
) {
  return listData.push({
    id: "456",
    name: "tom"
  })
}
export const getMessageResolver: ResolvableLiteral = {
  token: "messageData",
  deps: [Transition,"listData"],
  resolveFn
};
```

看下state中是如何使用的
```
const childState = {
  name: 'messagelist',
  parent:'folderlist',
  url: '/message',
  component: MessageListComponent,
  resolve:[getListResolver,getMessageResolver]
};
```

现在可以看到`getMessageResolver`中的deps依赖中是可以直接使用`getListResolver`中的token直接注入的。

【用于嵌套state中的resolve】  

父路由：
```
const parentState = {
  name: 'folderlist',
  url: '/folder',
  component: FolderListComponent,
  resolve:[getListResolver]
};
```

子路由：
```
const childState = {
  name: 'messagelist',
  parent:'folderlist',
  url: '/message',
  component: MessageListComponent,
  resolve:[getMessageResolver]
};
```

getMessageResolver
```
import { ResolvableLiteral, Transition } from "@uirouter/core";

export async function resolveFn(
  trans: Transition,
  listData
) {
  return listData.pop();
}

export const getMessageResolver: ResolvableLiteral = {
  token: "messageData",
  deps: [Transition,"listData"],
  resolveFn
};
```

getMessageResolver中依赖可以直接使用getListResolver中返回的数据，这就是嵌套state中resolve的使用方法。

### Abstract
抽象状态永远不能被直接激活。使用抽象状态向子状态提供继承属性(url、解析、数据等)。

```
const parentState = {
  name: 'folderlist',
  url: '/folder',
  abstract: true,
  component: FolderListComponent
};
```
这样我们直接访问：`http://localhost:4200/#/folder`是会报错的，上面说了：抽象状态永远不能被直接激活。  
但是我们可以直接访问它的子路由，并且子路由会继承父路由的相关数据的。


### parent
父路由
```
const parentState = {
  name: 'folderlist',
  url: '/folder',
  component: FolderListComponent
};
```

子路由
```
const childState = {
  name: 'folderlist.messagelist',
  url: '/message',
  component: MessageListComponent
};
```

那么我们如果要调整到这个子路由时，就必须要这样写：
```
<a uiSref="folderlist.messagelist" uiSrefActive="active" style="margin-right:20px;">messagelist</a>
```

parent属性给我们带来另外一种方式：
```
const childState = {
  name: 'messagelist',
  parent:'folderlist',
  url: '/message',
  component: MessageListComponent
};
```

现在可以这样访问：
```
<a uiSref="messagelist" uiSrefActive="active" style="margin-right:20px;">messagelist</a>
```

### redirectTo
如果定义了此属性，将根据属性的值重定向

```
const parentState = {
  name: 'folderlist',
  url: '/folder',
  component: FolderListComponent,
  redirectTo: 'folderlist.foolist'
};
```
这样写的话，当我们访问`folderlist`路由时，会直接定向到`folderlist.foolist`  
如果`redirectTo: 'folderlist.foolist'`是一个不存在的值，则该路由会不起作用

redirectTo的常用方法：
```
// 跳转到'A.B'路由
.state('A', {
  redirectTo: 'A.B' 
})

// 跳转到'C.D'路由，并且带上参数
.state('C', {
  redirectTo: { state: 'C.D', params: { foo: 'index' } }
})

// 跳转到路由"A"
.state('E', {
  redirectTo: () => "A"
})

// 根据参数判断要跳转的路由
.state('F', {
  redirectTo: (trans) => {
    if (trans.params().foo < 10)
      return { state: 'F', params: { foo: 10 } };
  }
})

// 向后台获取要跳转的路由
.state('G', {
  redirectTo: (trans) => {
    let svc = trans.injector().get('SomeAsyncService')
    let promise = svc.getAsyncRedirectTo(trans.params.foo);
    return promise;
  }
})

// 根据resolve Data 来判断是跳转到登录还是其它界面
.state('G', {
  redirectTo: (trans) => {
    // getAsync tells the resolve to load
    let resolvePromise = trans.injector().getAsync('SomeResolve')
    return resolvePromise.then(resolveData => resolveData === 'login' ? 'login' : null);
  }
})
```

### hook

【onEnter】  
> 当进入某个路由的时候触发的钩子函数

```
const aboutState = {
  name: 'about',
  url: '/about',
  component: AboutComponent,
  onEnter: function(trans, state) {
    console.log("Entering " + state.name);
  }
};
```
当进入about路由时，会触发onEnter函数

【onExit】
> 当离开一个路由时触发

```
const aboutState = {
  name: 'about',
  url: '/about',
  component: AboutComponent,
  onEnter: function(trans, state) {
    console.log("Entering " , state , trans);
  },
  onExit: function(trans, state) {
    console.log("Leaving " + state.name);
  }
};
```
