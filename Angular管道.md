## 管道
> 通过引入Angular管道，你可以把这种简单的“显示-值”转换器声明在HTML中，管道把数据作为输入，然后转换它，给出期望的输出。

## 使用管道
```
<p>the hero birthday is {{birthday | date}}</p>
```

## 内置管道

[所有管道](https://www.angular.cn/api?status=stable&type=pipe)
- DatePipe 日期转换
- UpperCasePipe 单词大写
- LowerCasePipe 单词小写
- CurrencyPipe  货币
- PercentPipe 百分数
- AsyncPipe 接收一个promise或Observable作为输入，并且自动订阅这个输入，最终返回它们给出的值

## 管道参数化

```
<p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
```
** HH大写时是24小时制 **

## 链式管道

```
<p>
The chained hero's birthday is
{{  birthday | date:'fullDate' | uppercase}}
</p>
```

## 自定义管道

```
import { Pipe, PipeTransform } from '@angular/core';
/*
 * Raise the value exponentially
 * Takes an exponent argument that defaults to 1.
 * Usage:
 *   value | exponentialStrength:exponent
 * Example:
 *   {{ 2 | exponentialStrength:10 }}
 *   formats to: 1024
*/
@Pipe({name: 'exponentialStrength'})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent: string): number {
    let exp = parseFloat(exponent);
    return Math.pow(value, isNaN(exp) ? 1 : exp);
  }
}
```
自定义管道请注意以下几点：

- 你使用自定义管道的方式和内置管道完全相同。
- 你必须在 AppModule 的 declarations 数组中包含这个管道。

## 管道与变更检测
> Angular 通过变更检测过程来查找绑定值的更改，并在每一次 JavaScript 事件之后运行：每次按键、鼠标移动、定时器以及服务器的响应。 这可能会让变更检测显得很昂贵，但是 Angular 会尽可能降低变更检测的成本。

> 当使用管道时，Angular 会选用一种更简单、更快速的变更检测算法。


引用是Angular所关心的一切。从Angular的角度来看，这是同一个数组，没有变换，也就不需要更新显示


## 纯pure管道与非纯(impure)管道
> 有两类管道：纯的与非纯的。 默认情况下，管道都是纯的。以前见到的每个管道都是纯的。 通过把它的 pure 标志设置为 false，你可以制作一个非纯管道


【纯管道】  
Angular只有在它检测到输入值发生了纯变更时才会执行纯管道。纯变更是指对原始类型值(String,Number,Boolean,Symbol)的更改，或者对对象引用的更改

Angular 会忽略(复合)对象内部的更改。 如果你更改了输入日期(Date)中的月份、往一个输入数组(Array)中添加新值或者更新了一个输入对象(Object)的属性，Angular 都不会调用纯管道

【非纯管道】  
Angular会在每个组件的变更检测周期中执行非纯管道。非纯管道可能会被调用很多次，和每个按键或每次鼠标移动一样频繁。**必须小心翼翼的实现非纯管道。 一个昂贵、迟钝的管道将摧毁用户体验**


纯管道-->非纯管道

纯管道
```
import { Pipe, PipeTransform } from '@angular/core';

import { Flyer } from './heroes';

@Pipe({ name: 'flyingHeroes' })
export class FlyingHeroesPipe implements PipeTransform {
  transform(allHeroes: Flyer[]) {
    return allHeroes.filter(hero => hero.canFly);
  }
}
```

非纯管道
```
@Pipe({
  name: 'flyingHeroesImpure',
  pure: false // 通过这个值得改变，改变了angular变更检测的方式
})
export class FlyingHeroesImpurePipe extends FlyingHeroesPipe {}
```

## 非纯AsyncPipe
> Angular 的 AsyncPipe 是一个有趣的非纯管道的例子。 AsyncPipe 接受一个 Promise 或 Observable 作为输入，并且自动订阅这个输入，最终返回它们给出的值

```
import { Component } from '@angular/core';

import { Observable, interval } from 'rxjs';
import { map, take } from 'rxjs/operators';

@Component({
  selector: 'app-hero-message',
  template: `
    <h2>Async Hero Message and AsyncPipe</h2>
    <p>Message: {{ message$ | async }}</p>
    <button (click)="resend()">Resend</button>`,
})
export class HeroAsyncMessageComponent {
  message$: Observable<string>;

  private messages = [
    'You are my hero!',
    'You are the best hero!',
    'Will you be my hero?'
  ];

  constructor() { this.resend(); }

  resend() {
    this.message$ = interval(500).pipe(
      map(i => this.messages[i]),
      take(this.messages.length)
    );
  }
}
```

这个 Async 管道节省了组件的样板代码。 组件不用订阅这个异步数据源，而且不用在被销毁时取消订阅(如果订阅了而忘了反订阅容易导致隐晦的内存泄露)。
