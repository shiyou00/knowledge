## 前言
通过前面对ui-router的基础知识学习，本文使用and-design实战后台管理系统。串联起前面学习的知识来。

[代码托管地址](https://github.com/shiyou00/angular-ui-router/tree/master/admin-uirouter)

## 初始化一个angular 6+ 项目

安装angular/cli
```
npm install -g angular/cli
安装好后：ng --version
```

创建一个angular项目
```
ng new taskmgr --style less // 样式默认使用less
```

初始化ant design for angular
```
ng add ng-zorro-antd
```

## 添加相应组件
app/pages/index
```
ng g c pages/index
```

该组件主要用来搭建界面框架


app/pages/manage
```
ng g c pages/manage
```

具体内容界面，展示与index界面里面的`ui-view`


