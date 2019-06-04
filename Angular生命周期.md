每个组件都有一个被Angular管理的生命周期

Angular创建它，渲染它，创建并渲染它的子组件，在它被绑定的属性发生变化时检查它，并在它从DOM中被移除前销毁它。

### 声明周期的顺序
| 钩子          | 用途及时机           |
| ------------- |:-------------:|
| ngOnChanges() | 当被绑定的输入属性的值发生变化时调用,首次调用一定会发生在ngOnInit（）之前  |
| ngOnInit()    | 在Angular第一次显示数据绑定和设置指令/组件的输入属性之后，初始化指令/组件 |
| ngDoCheck()   | 检测，并在发生Angular无法或不愿意自己检测的变化时作出反应。在每个 Angular 变更检测周期中调用，ngOnChanges() 和 ngOnInit() 之后。      |
| ngAfterContentChecked | 每次完成被投影组件内容的变更检测之后调用。 |
| ngAfterViewInit() | 初始化完组件视图及其子视图之后调用。 |
| ngAfterViewChecked() | 每次做完组件视图和子视图的变更检测之后调用 |
| ngOnDestory() | 当Angular每次销毁指令/组件之前调用并清除。在这儿反订阅可观察对象和分离事件处理器，以防内存泄露 |

### OnInit()钩子
> 这里是放置复杂初始化逻辑的好地方

1. 在构造函数之后马上执行复杂的初始化逻辑
2. 在Angular设置完输入属性之后，对该组件进行准备

### OnChanges()钩子
> 一旦检测到该组件(或指令)的输入属性(@Input)发生了变化，Angular就会调用它的ngOnChanges()方法。

```
ngOnChanges(changes: SimpleChanges) {
  for (let propName in changes) {
    let chng = changes[propName];
    let cur  = JSON.stringify(chng.currentValue);
    let prev = JSON.stringify(chng.previousValue);
    this.changeLog.push(`${propName}: currentValue = ${cur}, previousValue = ${prev}`);
  }
}
```

### DoCheck()钩子
> 使用DoCheck钩子来检测那些Angular自身无法捕获的变更并采取行动

**虽然 ngDoCheck() 钩子可以可以监测到英雄的 name 什么时候发生了变化。但其开销很恐怖。 这个 ngDoCheck 钩子被非常频繁的调用 —— 在每次变更检测周期之后，发生了变化的每个地方都会调它。 在这个例子中，用户还没有做任何操作之前，它就被调用了超过二十次。**


### AfterView钩子
> AfterView例子展示了AfterViewInit()和AfterViewChecked钩子，Angular会在每次创建了组件的子视图后调用它们。

**AfterViewChecked钩子也是会被频繁调用**


### AfterContent钩子
> AfterContent钩子和AfterView相似。关键的不同点是子组件的类型不同。

- AfterView钩子所关心的是ViewChildren，这些子组件的元素标签会出现在该组件的模板里面
- AfterContent钩子所关心的是ContentChildren，这些子组件被Angular投影进该组件中。

代码展示：
```
export class AfterContentComponent implements AfterContentChecked, AfterContentInit {
  private prevHero = '';
  comment = '';

  // 获取子组件的内容
  @ContentChild(ChildComponent) contentChild: ChildComponent;
  // 子组件的内容初始化
  ngAfterContentInit() {
    // contentChild is set after the content has been initialized
    this.logIt('AfterContentInit');
    this.doSomething();
  }
  // 子组件内容发生变化时
  ngAfterContentChecked() {
    // contentChild is updated after the content has been checked
    if (this.prevHero === this.contentChild.hero) {
      this.logIt('AfterContentChecked (no change)');
    } else {
      this.prevHero = this.contentChild.hero;
      this.logIt('AfterContentChecked');
      this.doSomething();
    }
  }
  // ...
}
```
