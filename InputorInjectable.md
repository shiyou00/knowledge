## 提纲
1. 简单了解下input是什么，有什么作用
2. 简单了解下inject是什么，有什么作用
3. 基于他们的雷同之处，我们该如何做出选择


## 前言
在组件化开发的项目中，对于我们来说组件之间的传值是一个核心话题，父->子传值、子->父传值、兄弟组件传值。今天这篇文章不打算全部覆盖传值方式，主要来学习下input的传值方式和inject的传值方式

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

## @Inject()



## 参考文献











