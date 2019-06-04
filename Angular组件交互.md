## 父 -> 子

### 通过输入型把数据从父组件传到子组件
父组件
```
    <app-hero-child *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </app-hero-child>
```

子组件
```
export class HeroChildComponent {
  @Input() hero: Hero;
  @Input('master') masterName: string;
}
```
给master属性起了一个别名masterName


### 通过setter截听输入属性值的变化

父组件
```
<app-name-child *ngFor="let name of names" [name]="name"></app-name-child>
```

子组件
```
export class NameChildComponent {
  private _name = '';
 
  @Input()
  set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }
 
  get name(): string { return this._name; }
}
```

### 通过ngOnChanges()来截听输入属性值得变化

> 使用OnChanges生命周期钩子接口的ngOnChanges()方法来监测输入属性值得变化并做出回应。

父组件
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-version-parent',
  template: `
    <h2>Source code version</h2>
    <button (click)="newMinor()">New minor version</button>
    <button (click)="newMajor()">New major version</button>
    <app-version-child [major]="major" [minor]="minor"></app-version-child>
  `
})
export class VersionParentComponent {
  major = 1;
  minor = 23;

  newMinor() {
    this.minor++;
  }

  newMajor() {
    this.major++;
    this.minor = 0;
  }
}
```

子组件
```
export class VersionChildComponent implements OnChanges {
  @Input() major: number;
  @Input() minor: number;
  changeLog: string[] = [];
 
  ngOnChanges(changes: {[propKey: string]: SimpleChange}) {
    let log: string[] = [];
    for (let propName in changes) {
      let changedProp = changes[propName];
      let to = JSON.stringify(changedProp.currentValue);
      if (changedProp.isFirstChange()) {
        log.push(`Initial value of ${propName} set to ${to}`);
      } else {
        let from = JSON.stringify(changedProp.previousValue);
        log.push(`${propName} changed from ${from} to ${to}`);
      }
    }
    this.changeLog.push(log.join(', '));
  }
}

```
---------

## 子 -> 父

### 父组件监听子组件的事件
子组件
```
export class VoterComponent {
  @Input()  name: string;
  @Output() voted = new EventEmitter<boolean>();//==>1 声明EventEmitter
  didVote = false;
 
  vote(agreed: boolean) {
    this.voted.emit(agreed); //==>2 向外发射事件
    this.didVote = true;
  }
}
```

父组件
```
import { Component }      from '@angular/core';

@Component({
  selector: 'app-vote-taker',
  template: `
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>
    <app-voter *ngFor="let voter of voters"
      [name]="voter"
      (voted)="onVoted($event)"> //1==>父组件使用onVoted($event)进行接收
    </app-voter>
  `
})
export class VoteTakerComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Mr. IQ', 'Ms. Universe', 'Bombasto'];
  //2==>定义接收函数以及参数
  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```

### 父组件与子组件通过“本地变量”互动
> 父组件不能使用数据绑定来读取子组件的属性或调用子组件的方法。但可以在父组件模板里，新建一个本地变量来代表子组件，然后利用这个变量来读取子组件的属性和调用子组件的方法

子组件
```
import { Component, OnDestroy, OnInit } from '@angular/core';

@Component({
  selector: 'app-countdown-timer',
  template: '<p>{{message}}</p>'
})
export class CountdownTimerComponent implements OnInit, OnDestroy {

  intervalId = 0;
  message = '';
  seconds = 11;

  clearTimer() { clearInterval(this.intervalId); }

  ngOnInit()    { this.start(); }
  ngOnDestroy() { this.clearTimer(); }

  start() { this.countDown(); }
  stop()  {
    this.clearTimer();
    this.message = `Holding at T-${this.seconds} seconds`;
  }

  private countDown() {
    this.clearTimer();
    this.intervalId = window.setInterval(() => {
      this.seconds -= 1;
      if (this.seconds === 0) {
        this.message = 'Blast off!';
      } else {
        if (this.seconds < 0) { this.seconds = 10; } // reset
        this.message = `T-${this.seconds} seconds and counting`;
      }
    }, 1000);
  }
}
```

父组件
```
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-lv',
  template: `
  <h3>Countdown to Liftoff (via local variable)</h3>
  <button (click)="timer.start()">Start</button>//2==>引用子组件方法
  <button (click)="timer.stop()">Stop</button>//3==>引用子组件方法
  <div class="seconds">{{timer.seconds}}</div>//4==>引用子组件变量
  <app-countdown-timer #timer></app-countdown-timer>//1==>定义变量
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownLocalVarParentComponent { }
```

### 父组件调用@ViewChild()
> “本地变量”方法是个简单便利的方法。但是它有局限性。因为父组件-子组件的连接必须全部在父组件的模板中进行。父组件本身的代码对子组件没有访问权

> 如果父组件的类需要读取子组件的属性值或调用子组件的方法，就不能使用“本地变量”方法

当父组件类需要这种访问时，可以把子组件作为ViewChild，注入到父组件里面

子组件代码同上面一样

父组件代码
```
import { AfterViewInit, ViewChild } from '@angular/core';//1==>引入对象
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-vc',
  template: `
  <h3>Countdown to Liftoff (via ViewChild)</h3>
  <button (click)="start()">Start</button>
  <button (click)="stop()">Stop</button>
  <div class="seconds">{{ seconds() }}</div>
  <app-countdown-timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownViewChildParentComponent implements AfterViewInit {//2==>挂上AfterViewInit生命周期的钩子

  @ViewChild(CountdownTimerComponent)//2==>把子组件注入到父组件中
  private timerComponent: CountdownTimerComponent;//3==>定义相应的变量接收

  seconds() { return 0; }

  ngAfterViewInit() {
    //5==>
    ngAfterViewInit() 生命周期钩子是非常重要的一步。被注入的计时器组件只有在 Angular 显示了父组件视图之后才能访问，所以它先把秒数显示为 0.

    然后 Angular 会调用 ngAfterViewInit 生命周期钩子，但这时候再更新父组件视图的倒计时就已经太晚了。Angular 的单向数据流规则会阻止在同一个周期内更新父组件视图。应用在显示秒数之前会被迫再等一轮。

    使用 setTimeout() 来等下一轮，然后改写 seconds() 方法，这样它接下来就会从注入的这个计时器组件里获取秒数的值。
    setTimeout(() => this.seconds = () => this.timerComponent.seconds, 0);
  }

  start() { this.timerComponent.start(); }//4==>直接调用子组件的方法
  stop() { this.timerComponent.stop(); }
}
```

### 父组件和子组件通过服务来通讯(rxjs)
> 引用message组件的代码

message.service.ts
```
import { Injectable } from "@angular/core";
import { Subject } from "rxjs";
import { IType } from "./IMessage";
@Injectable({
  providedIn: "root"
})
export class MessageService {
  private messages = new Subject<string>();
  messages$ = this.messages.asObservable();
  constructor() {}
  // 主要是负责对外提供接口方法，当外部调用时，传入了相应值，子组件只需要监听该流即可
  success(content: string) {
    this.next(content, IType.success);
  }
  info(content: string) {
    this.next(content, IType.info);
  }
  error(content: string) {
    this.next(content, IType.error);
  }
  private next(content: string, type: IType) {
    this.messages.next(
      JSON.stringify({
        content,
        type
      })
    );
  }
}

```
message-container.component.ts
```
import { Component, OnDestroy, OnInit } from "@angular/core";
import { IMessage, IType } from "../IMessage";
import { MessageService } from "../message.service";
import { Subscription } from "rxjs";

@Component({
  selector: "message-container",
  templateUrl: "./message-container.component.html",
  styleUrls: ["./message-container.component.scss"]
})
export class MessageContainerComponent implements OnInit, OnDestroy {
  messages: IMessage[] = [];
  constructor(private messageService: MessageService) {}
  subscription: Subscription;
  ngOnInit() {
    //1==> 子组件进行监听流的数据，当有数据推送到流中，即往数组中推送增加一条信息，并展示到界面中，这样就实现了，通过service服务来通讯
    this.subscription = this.messageService.messages$.subscribe(val => {
      this.create(content, type);
    });
  }
  ngOnDestroy() {
    this.subscription.unsubscribe();//退订订阅非常重要，防止内存泄露
  }
  create(content: string, type: IType) {
  }
  removeMessage(messageId: number) {
  }
  _generateMessageId() {
  }
}
```
