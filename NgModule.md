## NgModule
> 用于描述应用的各个部分如何组织在一起。每个应用有至少一个Angular模块、根模块就是你用来启动此应用的模块。按照惯例，它通常命名为AppModule

AppModule
```
/* JavaScript imports */
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';
 
import { AppComponent } from './app.component';
 
/* the AppModule class with the @NgModule decorator */
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
declarations -- 该应用所拥有的组件。

imports -- 导入BrowserModule以获取浏览器特有的服务，比如DOM渲染、无害化处理和位置(location)

providers -- 各种服务提供商

bootstrap -- 根组件，Angular创建它并插入index.html宿主页面


【declarations】  
该模块的 declarations 数组告诉 Angular 哪些组件属于该模块。 当你创建更多组件时，也要把它们添加到 declarations 中。

每个组件都应该（且只能）声明（declare）在一个 NgModule 类中。 如果你使用了未声明过的组件，Angular 就会报错。

能包含的类型：
- 组件 component
- 指令 directive
- 管道 pipe

【imports】  
模块的imports数组只会出现在@NgModule元数据中。它告诉Angular该模块想要正常工作，还需要哪些模块

【providers】  
providers 数组中列出了该应用所需的服务。当直接把服务列在这里时，它们是全应用范围的。当你使用特性模块和惰性加载时，它们是范围化的。

【bootstrap】  
放入根组件中

## 常用模块

| NgModule        | 导入自           | 为何使用  |
| ------------- |:-------------------------:| :-----:|
| BrowserModule | @angular/platform-browser | 当你要在浏览器中运行应用时 |
| CommonModule  | @angular/common           | 当你想要使用 NgIf 和 NgFor 时 |
|  FormsModule  | @angular/forms            |  当要构建模板驱动表单时（它包含 NgModel|

## 特性模块的分类

下面是特性模块的五个常用分类
- 领域特性模块
- 带路由的特性模块
- 路由模块
- 服务特性模块
- 可视部件特性模块

## 路由到的入口组件
所有路由组件都必须是入口组件。这需要你把同一个组件添加到两个地方（路由中和 entryComponents 中），但编译器足够聪明，可以识别出这里是一个路由定义，因此它会自动把这些路由组件添加到 entryComponents 中。

## entryComponents 数组
虽然 @NgModule 装饰器具有一个 entryComponents 数组，但大多数情况下你不用显式设置入口组件，因为 Angular 会自动把 @NgModule.bootstrap 中的组件以及路由定义中的组件添加到入口组件中。 虽然这两种机制足够自动添加大多数入口组件，但如果你要用其它方式根据类型来命令式的引导或动态加载某个组件，你就必须把它们显式添加到 entryComponents 中了。

## entryComponents 和编译器
> 对于生产环境的应用，你总是希望加载尽可能小的代码。 这些代码应该只包含你实际使用到的类，并且排除那些从未用到的组件。因此，Angular 编译器只会为那些可以从 entryComponents 中直接或间接访问到的组件生成代码。 这意味着，仅仅往 @NgModule.declarations 中添加更多引用，并不能表达出它们在最终的代码包中是必要的。


## 服务提供商
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root', // 指定该服务应该在根注入器中提供
})
export class UserService {
}

```
【提供商的作用域】
> 当你把服务提供商添加到应用的根注入器中时，它就在整个应用程序中可用了。 另外，这些服务提供商也同样对整个应用中的类是可用的 —— 只要它们有供查找用的服务令牌。

【provideIn 与 NgModule】
> 也可以规定某个服务只有在特定的 @NgModule 中提供。比如，如果你你希望只有当消费方导入了你创建的 UserModule 时才让 UserService 在应用中生效，那就可以指定该服务要在该模块中提供

```
import { Injectable } from '@angular/core';
import { UserModule } from './user.module';

@Injectable({
  providedIn: UserModule,
})
export class UserService {
}
```
