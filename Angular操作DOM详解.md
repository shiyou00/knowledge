## 前言
已经使用angular2+编写项目一段时间了，官网教程也看了一遍，但是一直以来好像对于angular是如何操作DOM好像还是一点都不懂，今天这篇文章主要讲解angular是如何操作DOM的。

新版本 Angular 需要在不同平台上运行，如 Browser 平台，Mobile 平台或者 Web Worker 平台，所以，就需要在特定平台的 API 和框架接口之间进行一层抽象（abstraction）。

Angular 中的这层抽象就包括这些引用类型：ElementRef、TemplateRef、ViewRef、ComponentRef 和 ViewContainerRef。

## @ViewChild
在探索 DOM 抽象类前，先了解下如何在组件/指令中获取这些抽象类。Angular 提供了一种叫做 DOM Query 的技术，主要来源于 @ViewChild 和 @ViewChildren 装饰器（decorators）。两者基本功能相同，唯一区别是 @ViewChild 返回单个引用，@ViewChildren 返回由 QueryList 对象包装好的多个引用。本文示例中主要以 ViewChild 为例，并且描述时省略 @。


通常这两个装饰器与模板引用变量（template reference variable）一起使用，模板引用变量仅仅是对模板（template）内 DOM 元素命名式引用（a named reference），类似于 html 元素的 id 属性。你可以使用模板引用（template reference）来标记一个 DOM 元素，并在组件/指令类中使用 ViewChild 装饰器查询（query）它，比如：

```
@Component({
    selector: 'sample',
    template: `
        <span #tref>I am span</span>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("tref", {read: ElementRef}) tref: ElementRef;

    ngAfterViewInit(): void {
        // outputs `I am span`
        console.log(this.tref.nativeElement.textContent);
    }
}
```

ViewChild 装饰器基本语法是：
```
@ViewChild([reference from template], {read: [reference type]});
```

上例中你可以看到，我把 tref 作为模板引用名称，并将 ElementRef 与该元素联系起来。第二个参数 read 是可选的，因为 Angular 会根据 DOM 元素的类型推断出该引用类型。例如，如果它（#tref）挂载的是类似 span 的简单 html 元素，Angular 返回 ElementRef；如果它挂载的是 template 元素，Angular 返回 TemplateRef。一些引用类型如 ViewContainerRef 就不可以被 Angular 推断出来，所以必须在 read 参数中显式申明。其他的如 ViewRef 不可以挂载在 DOM 元素中，所以必须手动在构造函数中编码构造出来。


现在，让我们看看应该如何获取这些引用，一起去探索吧。

## 获取DOM元素的方式

### ElementRef
> 这是最基本的抽象类，如果你查看它的类结构，就发现它只包含所挂载的元素对象，这对访问原生 DOM 元素很有用，比如：

```
// outputs `I am span`
console.log(this.tref.nativeElement.textContent);
```

然而，Angular 团队不鼓励这种写法，不但因为这种方式会暴露安全风险，而且还会让你的程序与渲染层（rendering layers）紧耦合，这样就很难在多平台运行你的程序。我认为这个问题并不是使用 nativeElement 而是使用特定的 DOM API 造成的，如 textContent。但是后文你会看到，Angular 实现了操作 DOM 的整体思路模型，这样就不再需要低阶 API，如 textContent。

使用 ViewChild装饰的 DOM 元素会返回 ElementRef，但是由于所有组件挂载于自定义 DOM 元素，所有指令作用于 DOM 元素，所以组件和指令都可以通过 DI（Dependency Injection）获取宿主元素的ElementRef 对象。比如：
```
@Component({
    selector: 'sample',
    ...
export class SampleComponent{
      constructor(private hostElement: ElementRef) {
             console.log(this.hostElement.nativeElement.outerHTML);
             //outputs <sample>...</sample>
      }
    ...
```

所以组件通过 DI（Dependency Injection）可以访问到它的宿主元素，但 ViewChild 装饰器经常被用来获取模板视图中的 DOM 元素。然而指令却相反，因为指令没有视图模板，所以主要用来获取指令挂载的宿主元素。

### TemplateRef
Angular 采用 template 标签这种方式，实现了 TemplateRef 抽象类来和 template 标签一起合作，看看它是如何使用的（译者注：ng-template 是 Angular 提供的类似于 template 原生 html 标签）：

```
@Component({
    selector: 'sample',
    template: `
        <ng-template #tpl>
            <span>I am span in template</span>
        </ng-template>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("tpl") tpl: TemplateRef<any>;

    ngAfterViewInit() {
        let elementRef = this.tpl.elementRef;
        // outputs `template bindings={}`
        console.log(elementRef.nativeElement.textContent);
    }
}
```

Angular 框架从 DOM 中移除 template 元素，并在其位置插入注释，这是渲染后的样子：
```
<sample>
    <!--template bindings={}-->
</sample>
```
[注意]`ng-template`里面的代码是不会被直接渲染出来的，只能获取到它的DOM元素，然后进行动态插入

TemplateRef 是一个结构简单的抽象类，它的 elementRef 属性是对其宿主元素的引用，还有一个 createEmbeddedView 方法。然而 createEmbeddedView 方法很有用，因为它可以创建一个视图（view）并返回该视图的引用对象 ViewRef。


### ViewRef
该抽象表示一个 Angular 视图（View），在 Angular 世界里，视图（View）是一堆元素的组合，一起被创建和销毁，是构建程序 UI 的基石。Angular 鼓励开发者把 UI 作为一堆视图（View）的组合，而不仅仅是 html 标签组成的树。

Angular 支持两种类型视图：

嵌入视图（Embedded View），由 Template 提供
宿主视图（Host View），由 Component 提供

### 创建嵌入视图
模板仅仅是视图的蓝图，可以通过之前提到的 createEmbeddedView 方法创建视图，比如：
```
ngAfterViewInit() {
    let view = this.tpl.createEmbeddedView(null);
}
```

### 创建宿主视图 ComponentFactoryResolver
宿主视图是在组件动态实例化时创建的，一个动态组件（dynamic component）可以通过 ComponentFactoryResolver 创建：

新增一个组件ModalContainerComponent
```
constructor(private injector: Injector,
            private r: ComponentFactoryResolver) {
    let factory = this.r.resolveComponentFactory(ModalContainerComponent);
    let componentRef = factory.create(injector);
    let view = componentRef.hostView;
}
```

在 Angular 中，每一个组件绑定着一个注入器（Injector）实例，所以创建 ModalContainerComponent 组件时传入当前组件（即 SampleComponent）的注入器。另外，别忘了，动态创建组件时需要在模块（module）或宿主组件的 EntryComponents 属性添加被创建的组件。

app.module.ts
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { OverlayModule } from '@angular/cdk/overlay';

import { AppComponent } from './app.component';
import { ModalContainerComponent } from './modal-container/modal-container.component';

@NgModule({
  declarations: [
    AppComponent,
    ModalContainerComponent
  ],
  imports: [
    BrowserModule,
    OverlayModule
  ],
  providers: [],
  bootstrap: [AppComponent],
  entryComponents: [ModalContainerComponent] // 这里一定要添加
})
export class AppModule { }
```

这里就不得不提到一句什么是entry component。以下是文档原文：
```
An entry component is any component that Angular loads imperatively by type.
所有通过类型进行命令式加载的组件都是入口组件。
```

现在，我们已经看到嵌入视图和宿主视图是如何被创建的，一旦视图被创建，它就可以使用 ViewContainer 插入 DOM 树中。下文主要探索这个功能。

## 渲染DOM的方式
> 把上面生成的View，挂载到视图容器上。

### ViewContainerRef
视图容器就是挂载一个或多个视图的容器。

首先需要说的是，任何 DOM 元素都可以作为视图容器，然而有趣的是，对于绑定 ViewContainer 的 DOM 元素，Angular 不会把视图插入该元素的内部，而是追加到该元素后面，这类似于 router-outlet 插入组件的方式。

通常，比较好的方式是把 ViewContainer 绑定在 ng-container 元素上，因为 ng-container 元素会被渲染为注释，从而不会在 DOM 中引入多余的 html 元素。下面示例描述在组建模板中如何创建 ViewContainer：

#### 操作视图
ViewContainer 提供了一些操作视图 API：
```
class ViewContainerRef {
    ...
    clear() : void // 清空视图容器中的视图
    insert(viewRef: ViewRef, index?: number) : ViewRef // 插入视图
    get(index: number) : ViewRef // 获取相应索引上的视图引用
    indexOf(viewRef: ViewRef) : number // 获取某个视图view的索引值
    detach(index?: number) : ViewRef // 清空某个索引下的视图引用
    move(viewRef: ViewRef, currentIndex: number) : ViewRef // 移动视图
}
```

从上文我们已经知道如何通过模板和组件创建两种类型视图，即嵌入视图和组件视图。一旦有了视图，就可以通过 insert 方法插入 DOM 中。
```
@Component({
    selector: 'sample',
    template: `
        <span>I am first span</span>
        <ng-container #vc></ng-container>
        <span>I am last span</span>
        <ng-template #tpl>
            <span>I am span in template</span>
        </ng-template>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("vc", {read: ViewContainerRef}) vc: ViewContainerRef; // 获取视图容器
    @ViewChild("tpl") tpl: TemplateRef<any>; // 获取模板引用
    
    ngAfterViewInit() {
        let view = this.tpl.createEmbeddedView(null); // 创建嵌入视图
        this.vc.insert(view); // 插入视图容器
    }
}
```
可以通过 `detach` 方法从视图中移除 DOM

#### 创建视图
> ViewContainer 也提供了手动创建视图 API ：

```
class ViewContainerRef {
    element: ElementRef
    length: number

    createComponent(componentFactory...): ComponentRef<C>
    createEmbeddedView(templateRef...): EmbeddedViewRef<C>
    ...
}
```
上面两个方法是个很好的封装，可以传入模板引用对象或组件工厂对象来创建视图，并将该视图插入视图容器中特定位置。

```
ngAfterViewInit() {
  this.vc.createEmbeddedView(this.tpl); // 创建view 并插入到容器中
}
```

### ngTemplateOutlet && ngComponentOutlet
尽管知道 Angular 操作 DOM 的内部机制是好事，但是要是有某种快捷方式就更好了啊。没错，Angular 提供了两种快捷指令：ngTemplateOutlet 和 ngComponentOutlet。

【ngTemplateOutlet】  
该指令会把 DOM 元素标记为 ViewContainer，并插入由模板创建的嵌入视图，从而不需要在组件类中显式创建该嵌入视图。这样，上面实例中，针对创建嵌入视图并插入 #vc DOM 元素的代码就可以重写：

```
@Component({
    selector: 'sample',
    template: `
        <span>I am first span</span>
        <ng-container [ngTemplateOutlet]="tpl"></ng-container>
        <span>I am last span</span>
        <ng-template #tpl>
            <span>I am span in template</span>
        </ng-template>
    `
})
export class SampleComponent {}
```
从上面示例看到我们不需要在组件类中写任何实例化视图的代码。非常方便，对不对。

【ngComponentOutlet】  
这个指令与 ngTemplateOutlet 很相似，区别是 ngComponentOutlet 创建的是由组件实例化生成的宿主视图，不是嵌入视图。你可以这么使用：

```
<ng-container *ngComponentOutlet="ModalContainerComponent"></ng-container>
```

## 小结
看似有很多新知识需要消化啊，但实际上 Angular 通过视图操作 DOM 的思路模型是很清晰和连贯的。你可以使用 ViewChild 查询模板引用变量来获得 Angular DOM 抽象类。

DOM 元素的最简单封装是 ElementRef。

对于模板，你可以使用 TemplateRef 来创建嵌入视图。

对于组件，可以使用 ComponentRef 来创建宿主视图，同时又可以使用 ComponentFactoryResolver 创建 ComponentRef。

这两个创建的视图（即嵌入视图和宿主视图）又会被 ViewContainerRef 管理。

最后，Angular 又提供了两个快捷指令自动化这个过程：ngTemplateOutlet 指令使用模板创建嵌入视图；ngComponentOutlet 使用动态组件创建宿主视图。
