## 前言
在组件化开发的项目中，对于我们来说组件之间的传值是一个核心话题，父->子传值、子->父传值、兄弟组件传值。今天这篇文章不打算全部覆盖传值方式，主要来学习下input的传值方式和Injectable的传值方式

## @Input()
> 一个装饰器，用来把某个类字段标记为输入属性，并且提供配置元数据。 声明一个可供数据绑定的输入属性，在变更检测期间，Angular 会自动更新它。


`@Input()`的工作方式：  
父组件
```
@Component({
  selector: 'app',
  template: `
    <bank-account [bankName]="'RBC'" [account-id]="'4747'"></bank-account>
  `
})

class App {}
```

子组件
```
@Component({
  selector: 'bank-account',
  template: `
    Bank Name: {{bankName}}
    Account Id: {{id}}
  `
})
class BankAccount {
  @Input() bankName: string;
  @Input('account-id') id: string;
  normalizedBankName: string;
}
```

对就是这么简单，主要就是用于父子之间数据传递。没有其它更加复杂的东西

## Injectable
标记性元数据，表示一个类可以由 Injector 进行创建。

看一个service
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
表示该服务是一个可以注入到组件中的服务    
[[原创]Angular依赖注入(DI)与服务service详解](Angular依赖注入与服务service详解.md)

那么在组件中我们可以这样使用服务：
```
import { Component, OnInit } from '@angular/core';
import { HeroService } from "../../shared/service/hero.service";

@Component({
  selector: 'hero-list',
  templateUrl: './hero-list.component.html',
  styleUrls: ['./hero-list.component.css'],
  providers: [HeroService]
})
export class HeroListComponent implements OnInit {

  heroList;

  constructor(
    public heroService: HeroService //constructor中注入该服务即可
  ) { }

  ngOnInit() {
    this.heroList = this.heroService.getHeroList();
  }
}
```

注入服务的概念也是比较简单的，其本意是分离组件对数据的操作，尽量让数据在service处理和保存，由于service是单例模式的，这样可以多个组件同时注入同一个service共用一类数据。达到组件间数据传递的效果。   
[注意]使用service注入数据的方式配合rxjs使用可以事半功倍。后续也会专门写一系列文章来讲解rxjs   

## 该使用哪个呢
在 Angular 应用中，通过 Service 来管理状态，把需要共享的数据通过 Observable 包装起来，提供相应的订阅接口即可。这样的状态流非常的简单清晰，易于维护。  
对于组件通信，在 Angular 中有多种方式，我建议的方式是：  

对于父 => 子，使用 @Input()  
对于子 => 父，使用 @Output() EventEmitter。(其实 EventEmitter 就是一个 Observable 对象)  
对于其他情况，在没有特殊需求的条件下，尽可能的使用 Service + Rxjs 的方式通信，让组件解耦。  
