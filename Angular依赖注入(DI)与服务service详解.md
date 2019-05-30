## 前言
本文将重点介绍Angular中两个重要的概念 `service` 以及 `DI`

[本文代码github地址](https://github.com/shiyou00/angular-service)


## 服务
> 组件应该把诸如从服务器获取数据、验证用户输入或直接往控制台中写日志等工作委托给各种服务。通过把各种处理任务定义到可注入的服务类中，你可以让它被任何组件使用。 通过在不同的环境中注入同一种服务的不同提供商，你还可以让你的应用更具适应性。

> Angular 会通过依赖注入来帮你更容易地将应用逻辑分解为服务，并让这些服务可用于各个组件中。

### 创建服务
DI 框架让你能从一个可注入的服务类（独立文件）中为组件提供数据。

快速创建一个服务  
`ng g s shared/service/hero`  

/service-test/src/app/shared/service/hero.service.ts  
```
import { Injectable } from '@angular/core';
import { heroList } from "../mock";
@Injectable({
  providedIn: 'root'
})
export class HeroService {

  constructor() { }

  getHeroList(){
    return heroList;
  }
}
```
@Injectable() 对所有服务都是必须的。  
该服务对外提供了一个getHeroList方法


### 注入服务
你可以通过制定带有依赖类型的构造函数参数来要求 Angular 在组件的构造函数中注入依赖项。
```
constructor(heroService: HeroService)
```

创建一个组件`ng g c pages/hero-list`  

```
import { Component, OnInit } from '@angular/core';
import { HeroService } from "../../shared/service/hero.service";

@Component({
  selector: 'hero-list',
  templateUrl: './hero-list.component.html',
  styleUrls: ['./hero-list.component.css']
})
export class HeroListComponent implements OnInit {

  heroList;

  constructor(
    public heroService: HeroService
  ) { }

  ngOnInit() {
    this.heroList = this.heroService.getHeroList();
  }
}

html代码：
<div>
  <h6>英雄展示</h6>
  <ul>
    <li *ngFor="let hero of heroList">
      姓名：{{ hero.name }}
      年龄：{{ hero.age }}
    </li>
  </ul>
</div>
```

至此我们就可以使用该服务对外提供的方法例如：getHeroList()来获取英雄列表  
![](https://www.showdoc.cc/server/api/common/visitfile/sign/58659a168614397dce9e8dab2404334d?showdoc=.jpg)

那么多个组件我们都可以通过同样的方式注入进去即可共用了，这也是angular数据传递比较重要的方式

## Angular 中的依赖注入
> 依赖注入（DI）是一种重要的应用设计模式。Angular 有自己的 DI 框架，在设计应用时常会用到它，以提升它们的开发效率和模块化程度。
> DI 是一种编码模式，其中的类会从外部源中请求获取依赖，而不是自己创建它们。
> 在 Angular 中，DI 框架会在实例化该类时向其提供这个类所声明的依赖项。


### 多级依赖注入器
Angular 的依赖注入系统是多级的。 实际上，应用程序中有一个与组件树平行的注入器树（译注：平行是指结构完全相同且一一对应）。 你可以在组件树中的任何级别上重新配置注入器。


### 配置服务注入器
我们创建的类提供了一个服务。@Injectable() 装饰器把它标记为可供注入的服务，不过在你使用该服务的 provider 提供商配置好 Angular 的依赖注入器之前，Angular 实际上无法将其注入到任何位置。

你可以在三种位置之一设置元数据，以便在应用的不同层级使用提供商来配置注入器：

1. 在服务本身的 @Injectable() 装饰器中。
2. 在 NgModule 的 @NgModule() 装饰器中。
3. 在组件的 @Component() 装饰器中。

#### @Injectable() 进行配置
当你在服务自身的 @Injectable() 装饰器中指定提供商时（通常在应用的根一级`root`），CLI 生产模式构建时所用的优化工具可以执行摇树优化，它会移除没有用过的那些服务。摇树优化生成的包会更小。

当使用 providedIn:'root' 时，你是在配置应用的根注入器，也就是 AppModule 的注入器。 整个注入器树的真正的根是平台注入器，它是根注入器的父节点。 这可以让多个应用共享同一套平台配置。

```
@Injectable({
  providedIn: 'root' // 指定为根目录，还可以指定为具体模块
})
```

#### @NgModule() 进行配置
NgModule 级的提供商可以在 @NgModule() providers 元数据中指定，也可以在 @Injectable() 的 providedIn 选项中指定某个模块类（但根模块 AppModule 除外）。

你还可以在非根 NgModule 元数据的 providedIn 选项中配置一个模块级的提供商，以便把该服务的范围限定到该模块一级。 这和在 @Injectable() 元数据中指定一个非根模块是基本等效的，但以这种方式提供的服务无法被摇树优化掉。

```
@NgModule({
  imports: [CommonModule],
  declarations: [],
  providers: [HeroService],
  exports: []
})
```

如果某个模块是惰性加载的，那么请使用 @NgModule() 的 provides 选项。加载那个模块时，就会用这里的提供商来配置模块本身的注入器，而 Angular 会为该模块中创建的任何类注入相应的服务。如果你使用了 @Injectable() 中的 providedIn: MyLazyloadModule 选项，那么如果该提供商没有在别处用过，就可以在编译期间把它摇树优化掉。


#### @Component() 进行配置
NgModule 中每个组件都有它自己的注入器。 通过使用 @Component 元数据在组件级配置某个提供商，你可以把这个提供商的范围限定到该组件及其子组件

```
@Component({
  selector: 'hero-list',
  templateUrl: './hero-list.component.html',
  styleUrls: ['./hero-list.component.css'],
  providers: [HeroService]
})
```

#### 注入器树与服务实例
> 在某个注入器的范围内，服务是单例的。也就是说，在指定的注入器中最多只有某个服务的最多一个实例。除非你在子注入器中配置了另一个提供商。
 
[注意]这句话这也理解当我们注入范围选择了`root`那么在root的范围内该服务最多就一个实例，但是如果我们在`@Component`中又配置了一个提供商，那么就对该服务另起一个实例。  


#### 注入器冒泡
当一个组件申请获得一个依赖时，Angular 先尝试用该组件自己的注入器来满足它。 如果该组件的注入器没有找到对应的提供商，它就把这个申请转给它父组件的注入器来处理。 如果那个注入器也无法满足这个申请，它就继续转给它在注入器树中的父注入器。 这个申请继续往上冒泡 —— 直到 Angular 找到一个能处理此申请的注入器或者超出了组件树中的祖先位置为止。 如果超出了组件树中的祖先还未找到，Angular 就会抛出一个错误。

如果你在不同的层级上为同一个 DI 令牌注册了提供商，那么 Angular 所碰到的第一个注入器就会用来提供该依赖。 比如，如果提供商注册在该组件的本地注入器上，那么当该组件需要这个服务时，Angular 就不会去找能提供同一服务的其它提供商。

[注意]使用@Host()可以限制冒泡。当搜索提供商时，就会在组件宿主元素的注入器处停下。


## 依赖提供商
依赖提供商会使用 DI 令牌来配置注入器，注入器会用它来提供这个依赖值的具体的、运行时版本。 注入器依靠 "提供商配置" 来创建依赖的实例，并把该实例注入到组件、指令、管道和其它服务中。

```
providers: [Logger]
```
不过，你也可以用一个替代提供商来配置注入器，这样就可以指定另一些同样能提供日志功能的对象。  
- 你可以提供一个替代类。
- 你可以提供一个类似于 Logger 的对象。
- 你的提供商可以调用一个工厂函数来创建 logger。


### Provider 对象字面量
类提供商的语法实际上是一种简写形式，它会扩展成一个由 Provider 接口定义的提供商配置对象。
```
[{ provide: Logger, useClass: Logger }]
```

实际上面两种配置方式是相等的
```
[Logger] == [{ provide: Logger, useClass: Logger }]
```

### 替代类提供商
不同的类都可用于提供相同的服务。 比如，下面的代码告诉注入器，当组件使用 Logger 令牌请求日志对象时，给它返回一个 BetterLogger 实例
```
[{ provide: Logger, useClass: BetterLogger }]
```

EvenBetterLogger类
```
@Injectable()
export class EvenBetterLogger extends Logger {
  constructor(private userService: UserService) { super(); }

  log(message: string) {
    let name = this.userService.user.name;
    super.log(`Message to ${name}: ${message}`);
  }
}
```

### 别名类提供商
假设老的组件依赖于 OldLogger 类。OldLogger 和 NewLogger 的接口相同，但是由于某种原因，我们没法修改老的组件来使用 NewLogger。

当老的组件要使用 OldLogger 记录信息时，你可能希望改用 NewLogger 的单例来处理它。 在这种情况下，无论某个组件请求老的 logger 还是新的 logger，依赖注入器都应该注入这个 NewLogger 的单例。 也就是说 OldLogger 应该是 NewLogger 的别名。

如果你试图用 useClass 为 OldLogger 指定一个别名 NewLogger，就会在应用中得到 NewLogger 的两个不同的实例。
```
[ NewLogger,
  // Not aliased! Creates two instances of `NewLogger`
  { provide: OldLogger, useClass: NewLogger}]
```
要确保只有一个 NewLogger 实例，就要用 useExisting 来为 OldLogger 指定别名。
```
[ NewLogger,
  // Alias OldLogger w/ reference to NewLogger
  { provide: OldLogger, useExisting: NewLogger}]
```

### 值提供商
有时候，提供一个现成的对象会比要求注入器从类去创建更简单一些。 如果要注入一个你已经创建过的对象，请使用 useValue 选项来配置该注入器。

下面的代码定义了一个变量，用来创建这样一个能扮演 logger 角色的对象
```
// An object in the shape of the logger service
export function SilentLoggerFn() {}

const silentLogger = {
  logs: ['Silent logger says "Shhhhh!". Provided via "useValue"'],
  log: SilentLoggerFn
};
```

下面的提供商定义对象使用 useValue 作为 key 来把该变量与 Logger 令牌关联起来。
```
[{ provide: Logger, useValue: silentLogger }]
```

### 工厂提供商
hero.service.ts
```
constructor(
  private logger: Logger,
  private isAuthorized: boolean) { }

getHeroes() {
  let auth = this.isAuthorized ? 'authorized ' : 'unauthorized';
  this.logger.log(`Getting heroes for ${auth} user.`);
  return HEROES.filter(hero => this.isAuthorized || !hero.isSecret);
}
```

app-heroes
```
@Component({
  selector: 'app-heroes',
  providers: [ {   provide: HeroService,
                   useFactory: (logger: Logger, userService: UserService) => {
                                 return new HeroService(logger, userService.user.isAuthorized);
                               };,
                   deps: [Logger, UserService]
               }
             ]
})
```

### 预定义令牌与多提供商
Angular 提供了一些内置的注入令牌常量，你可以用它们来自定义系统的多种行为。

比如，你可以使用下列内置令牌来切入 Angular 框架的启动和初始化过程。 提供商对象可以把任何一个注入令牌与一个或多个用来执行应用初始化操作的回调函数关联起来。

- PLATFORM_INITIALIZER：平台初始化之后调用的回调函数。
- APP_BOOTSTRAP_LISTENER：每个启动组件启动完成之后调用的回调函数。这个处理器函数会收到这个启动组件的 ComponentRef 实例。
- APP_INITIALIZER：应用初始化之前调用的回调函数。注册的所有初始化器都可以（可选地）返回一个 Promise。所有返回 Promise 的初始化函数都必须在应用启动之前解析完。如果任何一个初始化器失败了，该应用就不会继续启动。

比如，当启动应用时，你可以使用同一个令牌注册多个初始化器。
```
export const APP_TOKENS = [
 { provide: PLATFORM_INITIALIZER, useFactory: platformInitialized, multi: true    },
 { provide: APP_INITIALIZER, useFactory: delayBootstrapping, multi: true },
 { provide: APP_BOOTSTRAP_LISTENER, useFactory: appBootstrapped, multi: true },
];
```

在其它地方，多个提供商也同样可以和单个令牌关联起来。 比如，你可以使用内置的 NG_VALIDATORS 令牌注册自定义表单验证器，还可以在提供商定义对象中使用 multi: true 属性来为指定的验证器令牌提供多个验证器实例。 Angular 会把你的自定义验证器添加到现有验证器的集合中。

### 可摇树优化的提供商
摇树优化是指一个编译器选项，意思是把应用中未引用过的代码从最终生成的包中移除。 如果提供商是可摇树优化的，Angular 编译器就会从最终的输出内容中移除应用代码中从未用过的服务。 这会显著减小你的打包体积。

> 理想情况下，如果应用没有注入服务，它就不应该包含在最终输出中。 不过，Angular 要能在构建期间识别出该服务是否需要。 由于还可能用 injector.get(Service) 的形式直接注入服务，所以 Angular 无法准确识别出代码中可能发生此注入的全部位置，因此为保险起见，只能把服务包含在注入器中。 因此，在 NgModule 或 组件级别提供的服务是无法被摇树优化掉的。

#### 创建可摇树优化的提供商
只要在服务本身的 @Injectable() 装饰器中指定，而不是在依赖该服务的 NgModule 或组件的元数据中指定，你就可以制作一个可摇树优化的提供商。
```
@Injectable({
  providedIn: 'root',
})
export class Service {
}
```

该服务还可以通过配置工厂函数来实例化
```
@Injectable({
  providedIn: 'root',
  useFactory: () => new Service('dependency'),
})
export class Service {
  constructor(private dep: string) {
  }
}
```

## 参考文献
- [官网api](https://angular.cn/guide/dependency-injection)