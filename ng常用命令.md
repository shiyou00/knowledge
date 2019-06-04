## 常用指令
1. 创建一个工程：ng new projectName
2. 创建一个模块：ng generate module app-routing || ng g m moduleName
3. 创建一个组件：ng generate component componentName || ng g c componentName
4. 创建一个指令：ng g d directiveName
5. 创建一个路由：ng generate module app-routing --flat --module=app || ng g m app-routing --flat --module=app
6. 启动服务：ng serve
7. 构建一个项目：ng build -prod（生产环境打包）
8. 查询ng版本：ng -v
9. 生成一个测试文件：ng g class message/message --spec=true

## 通用后缀
- 测试文件不创建：--spec=false
- 创建的组件的样式文件为scss：—style=scss
- --inline-style(ng generate component hero-app --inline-style) 创建组件时，cli会自动顶一个空的styles数组 
- 不安装依赖：--skip-install 简写：-si
- 把文件放进了src/app中，而不是单独的目录中：--flat
- 告诉CLI把它注册到AppModule的imports数组中 --module=app
