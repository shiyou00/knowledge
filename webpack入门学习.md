## 前言
前端构建工具经历了很多了，npm scripts -> grunt -> gulp -> Fis3 -> webpack -> rollup.js , 但是所有构建工具都是为了解决我们实际项目中的静态资源打包的问题，各有优缺点。今天我们主要是介绍webpack的使用方式。

## 什么是webpack
> 本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(static module bundler)。在 webpack 处理应用程序时，它会在内部创建一个依赖图(dependency graph)，用于映射到项目需要的每个模块，然后将所有这些依赖生成到一个或多个bundle。

## webpack的优点
- 专注于处理模块化的项目，能做到开箱即用、一步到位；
- 可通过 Plugin 扩展，完整好用又不失灵活性；
- 使用场景不局限于Web开发；
- 社区庞大活跃，经常引入紧跟时代发展的新特性，能为大多数场景找到已有的开源扩展；
- 良好的开发体验；

## webpack的缺点
- 只能用于采用模块化开发的项目。

## 核心概念

【入口(entry)】
> 入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，webpack 会找出有哪些模块和 library 是入口起点（直接和间接）依赖的。

默认值：
```
module.exports = {
    entry : './src/index.js' 
}
```
当然也可以多入口配置，在后面的实战中会一一讲到。

【输出(output)】
> output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，主输出文件默认为 ./dist/main.js，其他生成文件的默认输出目录是 ./dist。

简单配置output
```
const path = require('path');
module.exports = {
    entry : './src/index.js'
    output:{
        path : path.resolve(__dirname,'dist'),     // 输入文件的路径。path.resolve(__dirname,'') 这个是node语法 找到当前跟目录
        filename: 'my-first-webpack.bundle.js' // 输出文件的名字，具体有很多用法。实战中会讲到
    }
}
```

【loader】
> 作为开箱即用的自带特性，webpack 自身只支持 JavaScript。而 loader 能够让 webpack 处理那些非 JavaScript 文件，并且先将它们转换为有效 模块，然后添加到依赖图中，这样就可以提供给应用程序使用。

loader代码展示
```
const path = require('path');
module.exports = {
    entry : './src/index.js'
    output:{
        path : path.resolve(__dirname,'dist'), 
        filename: 'my-first-webpack.bundle.js'
    },
    module: {
        rules : [
            {
                test : /\.txt$/,         // 通过正则去匹配相应的文件
                use : 'raw-loader'    // 使用哪个loader来处理匹配的文件
            }
        ]
    }
}
```

【plugins】
> loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务，插件的范围包括：打包优化、资源管理和注入环境变量。

插件代码简单展示
```
const path = require('path');
module.exports = {
    entry : './src/index.js'
    output:{
        path : path.resolve(__dirname,'dist'), 
        filename: 'my-first-webpack.bundle.js'
    },
    module: {
        rules : [
            {
                test : /\.txt$/,         // 通过正则去匹配相应的文件
                use : 'raw-loader'    // 使用哪个loader来处理匹配的文件
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ]
}
```
在上面的示例中，html-webpack-plugin 会为你的应用程序生成一个 html 文件，然后自动注入所有生成的 bundle。

webpack 4.x之前的核心概念就以上几个点，后续在实战内容中会补充一些其他概念和实战用法

## 小结
webpack入门的核心的四个概念：entry 入口文件的配置， output 输出文件的配置，loader打包css,ES6,typeScript等变为浏览器可识别的模块。plugins则是在整体打包过程中解决loader无法实现的其他事，例如压缩文件之类的。

## 参考文献
- [官方文档](https://webpack.docschina.org/concepts/)
