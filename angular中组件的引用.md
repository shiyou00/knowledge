## 前言
现在我有3个组件他们之间的关系是  
父组件：RefParentComponent  
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'ref-parent',
  templateUrl: './ref-parent.component.html',
  styleUrls: ['./ref-parent.component.css']
})
export class RefParentComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  mult(a,b){
    return a * b;
  }
}
```
子组件1：RefChildOneComponent  
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'ref-child-one',
  templateUrl: './ref-child-one.component.html',
  styleUrls: ['./ref-child-one.component.css']
})
export class RefChildOneComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  add(a,b){
    return a + b;
  }
}
```

子组件2：RefChildTwoComponent
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'ref-child-two',
  templateUrl: './ref-child-two.component.html',
  styleUrls: ['./ref-child-two.component.css']
})
export class RefChildTwoComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  sub(a,b){
    return a - b;
  }
}
```

现在我希望：
1. RefParentComponent 获取到 RefChildOneComponent 的引用
2. RefChildOneComponent 获取到 RefParentComponent 的引用
3. RefChildOneComponent 获取到 RefChildTwoComponent 的引用

这也是今天我们主要讲解的，组件中的引用

## 父组件中引用子组件
父组件HTML：
```
<ref-child-one></ref-child-one>
<ref-child-two></ref-child-two>
```
父组件TS：
```
import { Component, OnInit, ViewChild, AfterViewInit } from '@angular/core';
import { RefChildOneComponent } from "./ref-child-one/ref-child-one.component";
@Component({
  selector: 'ref-parent',
  templateUrl: './ref-parent.component.html',
  styleUrls: ['./ref-parent.component.css']
})
export class RefParentComponent implements OnInit, AfterViewInit {

  @ViewChild(RefChildOneComponent) childOne:RefChildOneComponent;

  constructor() { }

  ngOnInit() {
  }

  ngAfterViewInit(){
    console.log(this.childOne);
    this.childOne.add(1,2);
  }

  mult(a,b){
    return a * b;
  }
}
```

## 子组件中引用父组件
方法1：

```
import {Component, OnInit, Optional} from '@angular/core';
import {RefParentComponent} from "../ref-parent.component";

@Component({
  selector: 'ref-child-two',
  templateUrl: './ref-child-two.component.html',
  styleUrls: ['./ref-child-two.component.css']
})
export class RefChildTwoComponent implements OnInit {

  constructor(@Optional() public parentComponent: RefParentComponent) { }

  ngOnInit() {
    this.parentComponent.mult(1,2); // 引用父组件中的方法
    this.parentComponent.childOne.add(1,2); // 引用父组件中引用的子组件的方法
  }

  sub(a,b){
    return a - b;
  }

}
```

方法2： 
 
parent
```
export abstract class Parent {
  mult(a,b){};
}

```

RefParentComponent
```
import { Component, OnInit, ViewChild, AfterViewInit, forwardRef } from '@angular/core';
import { RefChildOneComponent } from "./ref-child-one/ref-child-one.component";
import { Parent } from "./parent";

@Component({
  selector: 'ref-parent',
  templateUrl: './ref-parent.component.html',
  styleUrls: ['./ref-parent.component.css'],
  // 父组件通过providers 把自己当service提供出去了。这样子组件可以直接注入该服务，就可获取父组件的引用
  providers: [{ provide: Parent, useExisting: forwardRef(() => RefParentComponent) }],
  // forwardRef: 允许引用尚未定义的引用，这里RefParentComponent尚未定义，因为是引用自己，自己在下方定义了。
})
export class RefParentComponent implements OnInit, AfterViewInit {

  @ViewChild(RefChildOneComponent) childOne:RefChildOneComponent;

  constructor() { }

  ngOnInit() {
  }

  ngAfterViewInit(){
    console.log(this.childOne);
    this.childOne.add(1,2);
  }

  mult(a,b){
    return a * b;
  }
}
```

RefChildOneComponent
```
import { Component, OnInit, Optional } from '@angular/core';
import { Parent } from "../parent";

@Component({
  selector: 'ref-child-one',
  templateUrl: './ref-child-one.component.html',
  styleUrls: ['./ref-child-one.component.css']
})
export class RefChildOneComponent implements OnInit {
  // 直接通过DI注入父组件提供的服务
  constructor(@Optional() public parent: Parent) { }

  ngOnInit() {
    console.log(this.parent.mult(1,2));
  }

  add(a,b){
    return a + b;
  }
}
```

## 兄弟组件引用
ref-parent/ref-parent.component.html
```
<ref-child-one #oneCom></ref-child-one>
<ref-child-two [com]="oneCom"> </ref-child-two>
```

ref-child-two
```
import {Component, OnInit, Optional,Input} from '@angular/core';
import {RefParentComponent} from "../ref-parent.component";

@Component({
  selector: 'ref-child-two',
  templateUrl: './ref-child-two.component.html',
  styleUrls: ['./ref-child-two.component.css']
})
export class RefChildTwoComponent implements OnInit {

  @Input() com; // 注入

  constructor(@Optional() public parentComponent: RefParentComponent) { }

  ngOnInit() {
    this.parentComponent.mult(1,2);
    this.parentComponent.childOne.add(1,2);
    console.log(this.com);
  }

  sub(a,b){
    return a - b;
  }

}
```

将 `ref-child-one` 组件实例注入到 `ref-child-two`中。

## 最佳实践
其实项目中避免不了组件之间的引用，但是最好的办法还是引入service。这也是angular胜过其他框架的地方

RefService
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class RefService {

  constructor() { }

  multSer(a,b){
    return a * b;
  }

  addSer(a,b){
    return a + b;
  }

  subSer(a,b){
    return a - b;
  }
}
```

组件中注入：
```
import { Component, OnInit, Optional } from '@angular/core';
import { Parent } from "../parent";
import { RefService } from "../../../shared/service/ref.service";

@Component({
  selector: 'ref-child-one',
  templateUrl: './ref-child-one.component.html',
  styleUrls: ['./ref-child-one.component.css']
})
export class RefChildOneComponent implements OnInit {

  constructor(
    @Optional() public parent: Parent,
    private ref : RefService
  ) { }

  ngOnInit() {
    console.log(this.ref.addSer(1,5));
  }

  add(a,b){
    return a + b;
  }
}
```

service + EventEmitter 实现多组件间数据实时交互

RefService
```
import { Injectable, EventEmitter } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class RefService {
  numChange: EventEmitter<number>;

  constructor() {
    this.numChange = new EventEmitter();
  }
}
```

RefChildOneComponent：发射数据
```
import { Component, OnInit, Optional } from '@angular/core';
import { Parent } from "../parent";
import { RefService } from "../../../shared/service/ref.service";

@Component({
  selector: 'ref-child-one',
  templateUrl: './ref-child-one.component.html',
  styleUrls: ['./ref-child-one.component.css']
})
export class RefChildOneComponent implements OnInit {

  constructor(
    @Optional() public parent: Parent,
    private ref : RefService
  ) { }

  ngOnInit() {
    // RefChildOneComponent 子组件不断发射数据
    let i = 0;
    setInterval(()=> {
      this.ref.numChange.emit(i++);
    }, 1000);
  }
}
```

RefChildTwoComponent：实时接收RefChildOneComponent发送过来的数据
```
import {Component, OnInit, Optional,Input} from '@angular/core';
import {RefParentComponent} from "../ref-parent.component";
import {RefService} from "../../../shared/service/ref.service";

@Component({
  selector: 'ref-child-two',
  templateUrl: './ref-child-two.component.html',
  styleUrls: ['./ref-child-two.component.css']
})
export class RefChildTwoComponent implements OnInit {

  constructor(
    private ref : RefService
  ) { }
  
  ngOnInit() {
    // RefChildTwoComponent 组件中不断接受来自 RefChildOneComponent 组件发射过来的数据
    this.ref.numChange.subscribe((value:number)=>{
      console.log(value);
    });
  }
}
```

这样在没有关联的两个组件中进行数据交互是非常方便的，之后引入rxjs配合angular的service依赖注入，将会给我们提供更加强大的跨组件交互能力。

## 小结
通过本文我们学习了组件间的一些引用实现。但是请尽量减少这样的写法，多引入service服务。
