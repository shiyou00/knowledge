## 前言
`CSS Modules` 它不是将 CSS 改造成编程语言，而是功能很单纯，只加入了局部作用域和模块依赖，这恰恰是网页组件最急需的功能。

因此，CSS Modules 很容易学，因为它的规则少，同时又非常有用，可以保证某个组件的样式，不会影响到其他组件。

![](./image/44.png)

本文所有代码示例：
[ruanyf](https://github.com/ruanyf/css-modules-demos)

## 局部作用域
CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。

产生局部作用域的唯一方法，就是使用一个独一无二的class的名字，不会与其他选择器重名。这就是 CSS Modules 的做法。

下面是一个React组件App.js。

```
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
上面代码中，我们将样式文件App.css输入到style对象，然后引用style.title代表一个class。
```
.title {
  color: red;
}
```

构建工具会将类名style.title编译成一个哈希字符串。
```
<h1 class="_3zyde4l1yATCOkgn-DBWEL">
  Hello World
</h1>

._3zyde4l1yATCOkgn-DBWEL {
  color: red;
}
```

这样一来，这个类名就变成独一无二了，只对App组件有效。

CSS Modules 提供各种插件，支持不同的构建工具。本文使用的是 Webpack 的css-loader插件，因为它对 CSS Modules 的支持最好，而且很容易使用

`webpack.config.js`配置清单
```
module.exports = {
  entry: __dirname + '/index.js',
  output: {
    publicPath: '/',
    filename: './bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'stage-0', 'react']
        }
      },
      {
        test: /\.css$/,
        loader: "style-loader!css-loader?modules"
      },
    ]
  }
};
```
上面代码中，关键的一行是style-loader!css-loader?modules，它在css-loader后面加了一个查询参数modules，表示打开 CSS Modules 功能。

## 全局作用域

CSS Modules 允许使用`:global(.className)`的语法，声明一个全局规则。凡是这样声明的class，都不会被编译成哈希字符串。

App.css加入一个全局class。
```
.title {
  color: red;
}

:global(.title) {
  color: green;
}
```

App.js使用普通的class的写法，就会引用全局class。
```
import React from 'react';
import styles from './App.css';

export default () => {
  return (
    <h1 className="title">
      Hello World
    </h1>
  );
};
```
CSS Modules 还提供一种显式的局部作用域语法:local(.className)，等同于.className，所以上面的App.css也可以写成下面这样。
```
:local(.title) {
  color: red;
}

:global(.title) {
  color: green;
}
```

## 定制哈希类名
css-loader默认的哈希算法是`[hash:base64]`，这会将`.title`编译成`._3zyde4l1yATCOkgn-DBWEL`这样的字符串。

webpack.config.js里面可以定制哈希字符串的格式。
```
module: {
  loaders: [
    // ...
    {
      test: /\.css$/,
      loader: "style-loader!css-loader?modules&localIdentName=[path][name]---[local]---[hash:base64:5]"
    },
  ]
}
```
你会发现.title被编译成了demo03-components-App---title---GpMto。

## Class 的组合
在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为"组合"（"composition"）。

在App.css中，让`.title`继承`.className` 。

```
.className {
  background-color: blue;
}

.title {
  composes: className;
  color: red;
}
```
App.js不用修改。
```
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
App.css编译成下面的代码。
```
._2DHwuiHWMnKTOYG45T0x34 {
  color: red;
}

._10B-buq6_BEOTOl9urIjf8 {
  background-color: blue;
}
```

相应地， h1的class也会编译成`<h1 class="_2DHwuiHWMnKTOYG45T0x34 _10B-buq6_BEOTOl9urIjf8">`


## 输入变量
CSS Modules 支持使用变量，不过需要安装 PostCSS 和 postcss-modules-values。
```
npm install --save postcss-loader postcss-modules-values 
```

把`postcss-loader`加入`webpack.config.js`
```
var values = require('postcss-modules-values');

module.exports = {
  entry: __dirname + '/index.js',
  output: {
    publicPath: '/',
    filename: './bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'stage-0', 'react']
        }
      },
      {
        test: /\.css$/,
        loader: "style-loader!css-loader?modules!postcss-loader"
      },
    ]
  },
  postcss: [
    values
  ]
};
```

接着，在colors.css里面定义变量。
```
@value blue: #0c77f8;
@value red: #ff0000;
@value green: #aaf200;
```

App.css可以引用这些变量。
```
@value colors: "./colors.css";
@value blue, red, green from colors;

.title {
  color: red;
  background-color: blue;
}
```

## 如何配合less或sass使用

[详细介绍了配合css-modules配合sass的使用](https://github.com/camsong/blog/issues/5)

