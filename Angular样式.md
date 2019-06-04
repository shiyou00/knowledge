## 组件样式
> Angular 应用使用标准的 CSS 来设置样式。这意味着你可以把关于 CSS 的那些知识和技能直接用于 Angular 程序中，例如：样式表、选择器、规则以及媒体查询等

另外，Angular 还能把组件样式捆绑在组件上，以实现比标准样式表更加模块化的设计。

## 使用组件样式
> 对你编写的每个Angular组件来说，除了定义HTML模板之外，还要定义用于模板的css样式、指定任意的选择器、规则和媒体查询

## 范围化样式
> 在@Component的元数据中指定的样式只会对该组件的模板生效

它们既不会被模板中嵌入的组件继承，也不会被通过内容投影(ng-content)嵌进来的组件集成，这种范围限制就是所谓的样式模块化特性

- 可以使用对每个组件最有意义的css类名和选择器
- 类名和选择器是仅属于组件内部的，它不会和应用中其它地方的类名和选择器出现冲突
- 组件的样式不会因为别的地方修改了样式而被意外改变
- 你可以让每个组件的css代码和它的TypeScript、HTML代码放在一起，这将促成清爽整洁的项目结构
- 将来你可以修改或移除组件的css代码，而不用遍历整个应用来看它有没有被别处用到，只要看看当前组件就可以了


## 特殊的选择器

:host选择器(YW:没有完全理解清楚如何使用)
> 使用 :host 伪类选择器，用来选择组件宿主元素中的元素

```
:host(.active){
    border:1px solid #ddd;
}
```
只有当宿主元素同时带有 active css类的时候才会生效

:host-context选择器
> 有时候，基于某些来自组件视图外部的条件应用样式是很有用的。例如，在文档的`<body>`元素上可能有一个用于表示样式主题(theme)的css类，你应当基于它来决定组件的样式。

```
:host-context(.theme-light) h2 {
  background-color: #eef;
}
```
它在当前组件宿主元素的祖先节点中查找CSS类，直到文档的根节点为止，在与其它选择器组合使用时，它非常有用

上面例子：只有当某个祖先元素有CSS类 theme-light时，才会把background样式应用到组件内部的所有`<h2>`元素中

### 把样式加载进组件中

1. 设置 styles 或 styleUrls元数据
2. 内联在模板的HTML中
3. 通过css文件导入

元数据中的样式
```
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }'] // 直接写入样式
  styleUrls: ['./hero-app.component.css'] // 元数据中引入样式文件
})
export class HeroAppComponent {
/* . . . */
}
```
**可以指定多个样式文件，也可以组合使用 style和styleUrls**

模板内联样式
```
@Component({
  selector: 'app-hero-controls',
  template: `
    <style>
      button {
        background-color: white;
        border: 1px solid #777;
      }
    </style>
     <link rel="stylesheet" href="../assets/hero-team.component.css">
    <h3>Controls</h3>
    <button (click)="activate()">Activate</button>
  `
})

```
直接模板中使用 `<style></style>`
或者直接使用 link标签

### css中 @import 语法
```
@import './hero-details-box.css';
```
css中通过@import导入其它css文件，相对路径
