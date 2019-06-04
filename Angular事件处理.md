点击事件

```
<button (click)="onClickMe()">Click me!</button
```

通过$event对象取得用户输入

```
template: `
  <input (keyup)="onKey($event)">
  <p>{{values}}</p>
`
```

从一个模板引用变量中获得用户输入
```
@Component({
  selector: 'app-loop-back',
  template: `
    <input #box (keyup)="0">
    <p>{{box.value}}</p>
  `
})
export class LoopbackComponent { }
```

按键事件过滤

keyup.enter --> 只有当用户敲回车键时，才会调用事件处理器
```
@Component({
  selector: 'app-key-up3',
  template: `
    <input #box (keyup.enter)="onEnter(box.value)">
    <p>{{value}}</p>
  `
})
export class KeyUpComponent_v3 {
  value = '';
  onEnter(value: string) { this.value = value; }
}
```
