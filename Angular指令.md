
## 属性型指令
> 属性型指令用于改变一个DOM元素的外观或行为。

在Angular中有三种类型的指令：
1. 组件 - 拥有模板的指令
2. 结构型指令 - 通过添加和移除DOM元素改变DOM布局的指令
3. 属性型指令 - 改变元素、组件或其他指令的外观和行为的指令


## 拖拽指令(属性型指令)
```
import {
  Directive,
  HostListener,
  ElementRef,
  Renderer2,
  Input,
  EventEmitter,
  Output
} from "@angular/core";

@Directive({
  selector: "[app-draggable][draggedClass][dragData]" // 外部传递进来的数据
})
export class DragDirective {
  private _isDraggable = false;

  @Input("app-draggable")
  set isDraggable(val: boolean) {
    this._isDraggable = val;
    this.rd.setAttribute(this.el.nativeElement, "draggable", `${val}`); // 设置元素是否支持拖拽
  }
  get isDraggable() {
    return this._isDraggable;
  }

  @Input() draggedClass: string; // 传递进入的数据
  @Input() dragData: string;    // 传递进入的数据
  @Input() dragEnterClass: string; // 传递进入的数据
  @Output() startData = new EventEmitter(); // 输出的事件
  @Output() dropData = new EventEmitter(); // 输出的事件

  constructor(private el: ElementRef, private rd: Renderer2) {}
      
  @HostListener("dragstart", ["$event"]) // @HostListener 装饰器引用属性型指令的宿主元素
  onDragStart(ev: Event) {
    // 通过this.el来访问DOM元素
    if (this.el.nativeElement === ev.target) {
      // 通过this.rd来对DOM进行安全操作
      this.rd.addClass(this.el.nativeElement, this.draggedClass);
      this.startData.emit(this.dragData);
    }
  }

  @HostListener("dragend", ["$event"])
  onDragEnd(ev: Event) {
    if (this.el.nativeElement === ev.target) {
      this.rd.removeClass(this.el.nativeElement, this.draggedClass);
    }
  }

  @HostListener("dragenter", ["$event"])
  onDragEnter(ev: Event) {
    ev.preventDefault();
    ev.stopPropagation();
    if (this.el.nativeElement.className.indexOf("drag-start") > -1) {
      return false;
    }
    this.rd.addClass(this.el.nativeElement, this.dragEnterClass);
  }

  @HostListener("dragover", ["$event"])
  onDragOver(ev: Event) {
    ev.preventDefault();
    ev.stopPropagation();
  }

  @HostListener("dragleave", ["$event"])
  onDragLeave(ev: Event) {
    ev.preventDefault();
    ev.stopPropagation();
    this.rd.removeClass(this.el.nativeElement, this.dragEnterClass);
  }

  @HostListener("drop", ["$event"])
  onDrop(ev: Event) {
    ev.preventDefault();
    ev.stopPropagation();
    this.rd.removeClass(this.el.nativeElement, this.dragEnterClass);
    this.dropData.emit(this.dragData);
  }
}

```
