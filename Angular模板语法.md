
## 常见用法
- 如果必须读取目标元素上的属性或调用它的某个方法，得用另一种技术。 参见 API 参考手册中的 ViewChild 和 ContentChild。
- 动态改变样式和类名，请使用NgClass和NgStyle


### 插值表达式 => `{{...}}`
> Angular 对所有双花括号中的表达式求值，把求值的结果转换成字符串，并把它们跟相邻的字符串字面量连接起来。最后，把这个组合出来的插值结果赋给元素或指令的属性。

```
<p>{{ hero.name }}</p> // 插入标签
<p src={{hero.img}}></p> //插入属性
<p>{{i+1}}</p> //可以是表达式
<p>{{getVal()+i}}</p> //可以调用组件方法
```

### 模板表达式
> 模板表达式产生一个值。 Angular 执行这个表达式，并把它赋值给绑定目标的属性，这个绑定目标可能是 HTML 元素、组件或指令。

```
[property]="expression"
<span [hidden]="isUnchanged"></span>  //isUnchanged的引用的是组件的变量  
```

### 表达式上下文
> 典型的表达式上下文就是这个组件实例，它是各种绑定值的来源

```
<div *ngFor="let hero of heroes">{{hero.name}}</div>
// hero 是模板输入变量
<input #heroInput> {{heroInput.value}}
// #heroInput 是模板引用变量
```
表达式上下文变量由三者组成

- 模板变量(如：let hero)
- 上下文变量(如：#heroInput)
- 组件成员变量

优先级：模板变量 > 上下文变量 > 组件成员变量


### 表达式指南

模板表达式能成就或毁掉一个应用，请遵循下列指南

1、没有可见的副作用
> 模板表达式除了目标属性的值以外，不应该改变应用的任何状态

2、执行迅速
> Angular 会在每个变更检测周期后执行模板表达式。 它们可能在每一次按键或鼠标移动后被调用。

3、非常简单
> 虽然也可以写出相当复杂的模板表达式，但不要那么写。

4、幂等性
> 最好使用幂等的表达式，因为它没有副作用，并且能提升 Angular 变更检测的性能。幂等的表达式应该总是返回完全相同的东西，直到某个依赖值发生改变。

### 模板语句
> 用来响应由绑定目标触发的事件

代码展示
```
<button (click)="deleteHero()"></button>
```
**特别之处在于它支持基本赋值 (=) 和表达式链 (; 和 ,)。**
```
<button (click)="deleteHero();ipt=''"></button>
```
**某些语法还是不适用：**
1. new操作符
2. ++ --
3. 操作并赋值 +=和-=
4. 位操作符 |和&

模板的 $event 对象、模板输入变量 (let hero)和模板引用变量 (#heroForm)传给了组件中的一个事件处理器方法
实际项目中，就是通过这样传递和组件进行交互的
```
<button (click)="onSave($event)">Save</button> // 传入$event对象
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button> //传入模板输入变量，而不是组件中hero属性
<form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>
```

特别注意：模板语句不能引用全局命名空间的任何东西。比如不能引用window或document,也不能使用console.log或math.max

### 数据绑定语法

根据数据流向分三类
1. 从数据源到视图
2. 从视图到数据源
3. 从视图到数据源再到视图

从数据源到视图
```
{{expression}}
[target] = "expression"
bind-target = "expression" //同上面一种写法一样，一般不这样写
```

从视图到数据源的单向绑定(事件绑定)
```
(target)="statement" ; (click)="add($event)"
on-target = "statement" ; // 一般不这样写
```

双向数据绑定
```
[(target)]="expression"
bindon-target="expression"
```

综上可以有这个简单的记忆方法
```
bind = []
on = ()
bindon = [()]
```


### 理解HTML attribute 与 DOM property的对比
> 想要理解Angular绑定如何工作，重点就是搞清楚 HTML attribute和DOM property 之间的区别

attribute是由HTML定义的。property是由DOM(Document Object Model)
1. 少量HTML attribute 和 property 之间有着1:1的映射，如id
2. 有些HTML attribute 没有对应的 property , 如 colspan
3. 有些DOM property 没有对应的 attribute，如 textContent
4. 大量HTML attribute 看起来映射到了 property 但却不像你想的那样

**attribute初始化DOM property,然后它们的任务就完成了。property的值可以改变；attribute的值不能改变**

例如
```
<input type="text" value="bob">
attribute指定了初始值bob
property则是当前值
```

重点：模板绑定是通过property和事件来工作的，而不是attribute

> 在Angular的世界中，attribute唯一的作用是用来初始化元素和指令的状态。当进行数据绑定时，只是在与元素和指令的property和事件打交道，而attribute就完全靠边站了。


### 绑定目标

属性绑定
```
<img [src]="heroImageUrl"> //绑定src属性
<app-hero-detail [hero]="currentHero"> //组件注入hero属性
<div [ngClass]="{'special':isSpecial}"></div> // class属性 
<span [innerHTML]='title'></span> //插入HTML
```

事件绑定
```
<button (click)="onSave()"></button> //元素的事件
<app-hero-detail (deleteRequest)="deleteHero()"></app-hero-detail> //组件的事件传递 子=>父的传递
<div (myClick)="clicked=$event" clickable></div> //指令的事件
```

双向数据绑定
> 它是属性绑定和事件绑定的语法糖

```
<app-sizer [(size)]="fontSizePx"></app-sizer>
<div [style.font-size.px]="fontSizePx">Resizable Text</div>
|
|
<app-sizer [size]="fontSizePx" (sizeChange)="fontSizePx=$event"></app-sizer>
//如果能在想<input>这样的HTML元素上使用双向数据绑定就更好了，可惜，原生HTML元素不遵循x值和xChange事件的模式。Angular以NgModel指令为桥梁，允许在表单元素上使用双向数据绑定
```

Attribute（attribute例外情况,一般情况下是不会使用到attribute绑定的，但是当一个元素没有属性可绑定的时候，就必须使用attribute绑定，例如table中的colspan/rowspan属性）
```
<button [attr.aria-label]="help">help</button>
<td colspan="{{i+1}}"></td> //会报模板解析错误
<td [attr.colspan]="i+1"></td> //会正确展示
```

css类,以下方法可以使用ngClass替代，一次性可以判断多个
```
<div [class.special]="isSpecial">special</div> //如果isSpecial为真就会添加special
```

样式
```
<button [style.color]="isSpecial?'red':'green'"></button>
带单位的绑定，如font-size
<button [style.font-size.em]="isSpecial?3:1">Big</button>
```

### 使用属性绑定(`[xx]=""`)还是插值表达式`{{}}`

```
<img src="{{imgUrl}}"> //插值表达式
<img [src]="imgUrl"> //属性绑定

```

正常来说用谁都是可以达到同样的效果，项目中还是选择一种统一的风格进行
**有一个特殊情况：数据类型不是字符串时，就必须使用属性绑定**


### 插入安全内容
> angular数据绑定对危险HTML有防备，不管是插值表达式还是属性绑定，都不会允许带有script标签的HTML泄露到浏览器

### 内置指令

#### 内置属性指令
> 属性指令会监听和修改其它HTML元素或组件的行为、元素属性(attribute)、DOM属性(property).它们通常会作为HTML属性的名称而应用在元素上

- NgClass - 添加或移除一组css类
- NgStyle - 添加或移除一组css样式
- NgModel - 双向绑定到HTML表单元素

### NgClass
> 通过绑定NgClass，可以同时添加或移除多个类

模板代码：
```
<div [ngClass]="currentClasses"></div>

```

组件代码：
```
this.currentClasses =  {
    'saveable': this.canSave,
    'modified': !this.isUnchanged,
    'special':  this.isSpecial
  };
  // 它将会根据三个其它组件的状态为true或false而添加或移除三个类
```

### NgStyle

模板代码：
```
<div [ngStyle]="currentStyles"></div>
```

组件代码：
```
currentStyles: {};
setCurrentStyles() {
  // CSS styles: set per current state of component properties
  this.currentStyles = {
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
}
```

### NgModel
> 使用`[(ngModel)]`双向绑定到表单元素，必须导入 FormsModule并把它添加到Angular模块的imports列表中。

代码展示：
```
import { NgModule } from '@angular/core';
import { BrowserModule }  from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // <--- JavaScript import from Angular

/* Other imports */

@NgModule({
  imports: [
    BrowserModule,
    FormsModule  // <--- import into the NgModule
  ],
  /* Other module metadata */
})
export class AppModule { }
```

使用如下：
```
<input [(ngModel)]="name">
```

同样可以使用以下代码
```
<input [value]="name" (input)="name=$event.target.value">
```

ngModel指令通过自己的输入属性ngModel和输出属性ngModelChange 隐藏了那些细节
```
<input [ngModel]="name" (ngModelChange)="name=$event">
```

使用这种ngModel展开形式有什么作用呢？  
例如：
```
<input [ngModel]="name" (ngModelChange)="setUppercaseName($event)">
```

**你不能把 `[(ngModel)]`用到非表单类的原生元素或第三方自定义组件上，除非写一个合适的值访问器**


### 内置结构型指令
> 结构型指令的职责是HTML布局。它们塑造或重塑DOM的结构，这通常是通过添加、移除和操纵它们所附加到的宿主元素来实现的。

要点  
- 结构型指令的名字前加上 “*”前缀
- 当没有合适的宿主元素放置指令时，可用 <ng-container>对元素进行分组
- 只能往一个元素上应用一个结构型指令

3个结构型指令  
- NgIf 根据条件把一个元素添加到DOM中或从DOM移除
- NgSwitch一组指令，用来在多个可选视图之间切换
- NgForOf 对列表中的每个条目重复套用同一个模板

NgIf
```
<app-hero-detail *ngIf="isActive"></app-hero-detail>
```

else写法
```
<app-hero-detail *ngIf="isActive;else temName"></app-hero-detail>
<ng-template #temName></ng-template>
```

**这和显示/隐藏不是一回事,如果需要显示隐藏可以控制类**
```
<div [class.hidden]="isSpecial"></div>
<div [class.hidden]="!isSpecial"></div> 
```

NgForOf
> NgFor是一个重复器指令

```
<div *ngFor="let hero of heros;let i=index">{{hero.name}}</div>
```

带trackBy的*ngFor
> ngFor 指令有时候会性能较差，特别是在大型列表中。 对一个条目的一丁点改动、移除或添加，都会导致级联的 DOM 操作。

组件中
```
trackByHeroes(index:number,hero:Hero):number{
    return hero.id;
}
```

模板中
```
<div *ngFor="let hero of heroes; trackBy: trackByHeroes">
  ({{hero.id}}) {{hero.name}}
</div>
```

相当于使用id对列表进行标记，这样对列表进行删除时，可以不用全部替换DOM了

NgSwitch指令
> NgSwitch指令类似于js的switch语句，它可以从多个可能的元素中根据switch条件来显示某一个。Angular只会把选中的元素放进DOM中。

```
<div [ngSwitch]="currentHero.emotion">
    <app-happy-hero *ngSwitchCase="'happy'"></app-happy-hero>
    <app-sad-hero *ngSwitchCase="'sad'" [hero]="currentHero"></app-sad-hero>
    <app-unknown-hero  *ngSwitchDefault  [hero]="currentHero"></app-unknown-hero>
</div>
```

### ng-template
> 它是一个Angular元素，用来渲染HTML，它永远不会直接显示出来，事实上，在渲染视图之前，Angular会把 ng-template 及其内容替换为一个注释。

> 如果没有结构型指令，而仅仅把一些别的元素包装进 ng-template 中，那些元素就是不可见的

```
<p>Hip!</p>
<ng-template>
  <p>Hip!</p>
</ng-template>
<p>Hooray!</p>
```
Hip! 是不显示的

### ng-container 把一些兄弟元素归为一组
> 通常都要有一个根元素作为结构型指令的数组。列表元素（<li>）就是一个典型的供NgFor使用的宿主元素


### 模板引用变量(#var)
> 通常用来引用模板中的某个DOM元素，它还可以引用Angular组件或指令或Web Component

```
<input #phone placeholder="phone number">
//#phone的意思就是声明一个名叫phone的变量来引用input元素
```

可以使用ref替代#
```
<input ref-phone placeholder="phone number">
```

**我们可以在模板中的任何地方引用模板引用变量**
```
<button (click)="callPhone(phone.value)"></button>
```


### 输入和输出属性
输入属性：@Input 当它通过“属性绑定”的形式被绑定时，值会“流入”这个属性

输出属性：@Output 这个属性几乎总是返回EventEmitter。当它通过“事件绑定”的形式被绑定时，值会“流出”这个属性

组件中的书写方式
```
@Input() hero:Hero;
@Output() deleteRequest = new EventEmitter<Hero>();
//定义别名
@Output('myClick') clicks = new EventEmitter<Hero>();
```

指令中的书写方式
```
@Component({
    inputs:['hero'],
    outputs:['myDelete:deleteRequest'],//这个myDelete是别名
})
```

### 管道操作符

uppercase管道操作符
```
<div>Title through uppercase pipe:{{title | uppercase}}</div> 
```

多个管道操作符并行使用
```
<div>Title through uppercase pipe:{{title | uppercase | lowercase}}</div> 
```

使用参数的管道
```
<div>Birthdate:{{currentHero?.birthdate | date:'longDate'}}</div>
```

json管道对调试绑定特别有用：
```
<div>{{currentHero | json}}</div>
```

#### 安全导航操作符(?.)和空属性路径
> 用来保护出现在属性路径中 null 和 undefined 值。

```
<div>The current hero's name is {{currentHero.name}}</div> 
假设currentHero中没有name属性，则会报错，整个视图错误
```

使用安全导航操作符,就可以避免视图错误，但是如果是必须存在的值，还是不应该使用安全导航操作符
```
<div>The current hero's name is {{currentHero?.name}}</div>
```

使用NgIf代码环绕它来解决这个问题
```
<div *ngIf="nullHero">The null hero's name is {{nullHero.name}}</div>
```

亦或使用 && 来解决
```
The null hero's name is {{nullHero && nullHero.name}}
```

### 非空断言操作符(!)
> 在 TypeScript 2.0 中，你可以使用 --strictNullChecks 标志强制开启严格空值检查。TypeScript 就会确保不存在意料之外的 null 或 undefined。

这种模式下，有类型的变量默认是不允许 null 或 undefined 值的，如果有未赋值的变量，或者试图把 null 或 undefined 赋值给不允许为空的变量，类型检查器就会抛出一个错误。

如果类型检查器在运行期间无法确定一个变量是 null 或 undefined，那么它也会抛出一个错误。 你自己可能知道它不会为空，但类型检查器不知道。 所以你要告诉类型检查器，它不会为空，这时就要用到非空断言操作符。

```
<!--No hero, no text -->
<div *ngIf="hero">
  The hero's name is {{hero!.name}}
</div>
```
在Angular编译器把你的模板转换成TypeScript代码时，这个操作符会防止TypeScript报告"hero.name"可能为null或undefined的错误


### 类型转换函数 $any ($any(<表达式>))

> 有时候，绑定表达式可能会报类型错误、并且它不能或很难指定类型。要消除这种报错，你可以使用 $any 转换函数来把表达式转换成 any 类型

```
<div>
  The hero's marker is {{$any(hero).marker}}
</div>
```
当 Angular 编译器把模板转换成 TypeScript 代码时，$any 表达式可以防止 TypeScript 编译器报错说 marker 不是 Hero 接口的成员。
