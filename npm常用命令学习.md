## 什么是npm
npm有两层含义。一层含义是Node的开放式模块登记和管理系统，网址为npmjs.org。另一层含义是Node默认的模块管理器，是一个命令行下的软件，用来安装和管理Node模块。

npm不需要单独安装。在安装Node的时候，会连带一起安装npm。但是，Node附带的npm可能不是最新版本，最好用下面的命令，更新到最新版本。

```
npm install npm@latest -g
```
`@latest`表示最新版本
`-g`表示全局安装

## 常用的npm命令
```
npm help 查看命令列表
npm -l 查看各个命令的简单用法
npm -v 查看npm的版本
npm config list -l 查看 npm 的配置

npm init 初始化一个package.json文件，但是有很多问答需要手动输入
npm init -y 默认直接帮我们输入好，这是一个快捷方式

npm install 下载package.json中的依赖包
npm install --save-dev === npm install webpack -D 
npm install --save === npm install webpack -S
npm info webapck 查看软件有哪些版本号
npm install webpack@4.2.2 安装软件的具体版本
```

## npm set
npm set 用来设置环境变量

上面命令等于为npm init设置了默认值，以后执行npm init的时候，package.json的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的 `~/.npmrc`文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行npm config。
```
npm set init-author-name 'Your name'
npm set init-author-email 'Your email'
npm set init-author-url 'http://yourdomain.com'
npm set init-license 'MIT'
```

## npm config
```
npm config get globalconfig 输出全局配置文件npmrc的路径
npm config set prefix $dir 上面的命令将指定的$dir目录，设为模块的全局安装目录。如果当前有这个目录的写权限，那么运行npm install的时候，就不再需要sudo命令授权了。
```


## npm info
> npm info命令可以查看每个模块的具体信息。比如，查看underscore模块的信息。

## npm list
> npm list命令以树型结构列出当前项目安装的所有模块，以及它们依赖的模块。

```
npm list 当前目录的依赖树状结构
npm list -global 全局目录的依赖树状结构
npm list webpack 列出单个模块例如webpack
```

## npm install
> Node模块采用npm install命令安装。每个模块可以“全局安装”，也可以“本地安装”。“全局安装”指的是将一个模块安装到系统目录中，各个项目都可以调用。一般来说，全局安装只适用于工具模块，比如eslint和gulp。“本地安装”指的是将一个模块下载到当前项目的node_modules子目录，然后只有在项目目录之中，才能调用这个模块。

```
# 本地安装
npm install <package name>

# 全局安装
sudo npm install -global <package name>
sudo npm install -g <package name>
```
npm install也支持直接输入Github代码库地址。
```
npm install git://github.com/package/path.git
npm install git://github.com/package/path.git#0.1.0
```

安装之前，npm install会先检查，node_modules目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。

如果你希望，一个模块不管是否安装过，npm 都要强制重新安装，可以使用-f或--force参数。
```
npm install <packageName> --force
```

如果你希望，所有模块都要强制重新安装，那就删除node_modules目录，重新执行npm install。
```
rm -rf node_modules
npm install
```

【安装不同版本】  
install命令总是安装模块的最新版本，如果要安装模块的特定版本，可以在模块名后面加上@和版本号。
```
npm install sax@latest
npm install sax@0.1.1
npm install sax@">=0.1.0 <0.2.0"
npm install readable-stream --save --save-exact // 在package.json文件指定安装模块的确切版本
npm install <module-name>@1.3.1-beta.3 // 安装指定的beta版本
```

install命令可以使用不同参数，指定所安装的模块属于哪一种性质的依赖关系，即出现在packages.json文件的哪一项中。
> –save：模块名将被添加到dependencies，可以简化为参数-S。
> –save-dev: 模块名将被添加到devDependencies，可以简化为参数-D


npm install默认会安装dependencies字段和devDependencies字段中的所有模块，如果使用--production参数，可以只安装dependencies字段的模块。
```
npm install --production
```

## npm update，npm uninstall
```
# 升级当前项目的指定模块
npm update [package name]

# 升级全局安装的模块
npm update -global [package name]
```

npm uninstall命令，卸载已安装的模块。
```
npm uninstall [package name]

# 卸载全局模块
npm uninstall [package name] -global
```

## npm run
npm不仅可以用于模块管理，还可以用于执行脚本。package.json文件有一个scripts字段，可以用于指定脚本命令，供npm直接调用。

npm run命令会自动在环境变量$PATH添加node_modules/.bin目录，所以scripts字段里面调用命令时不用加上路径，这就避免了全局安装NPM模块。

npm run如果不加任何参数，直接运行，会列出package.json里面所有可以执行的脚本命令。

npm内置了两个命令简写，npm test等同于执行npm run test，npm start等同于执行npm run start。
```
{
  "name": "myproject",
  "devDependencies": {
    "jshint": "latest",
    "browserify": "latest",
    "mocha": "latest"
  },
  "scripts": {
    "lint": "jshint **.js", // 执行命令 npm run lint 则会执行冒号后面的命令
    "test": "mocha test/"
  }
}
```

关于这部分的内容具体可以看另外一篇文章[npm scripts 脚本基础](npmscripts脚本基础指南.md)

## npm bin
> npm bin命令显示相对于当前目录的，Node模块的可执行脚本所在的目录（即.bin目录）。

```
# 项目根目录下执行
npm bin
./node_modules/.bin
```

## npm adduser
npm adduser用于在npmjs.com注册一个用户。

```
npm adduser
Username: YOUR_USER_NAME
Password: YOUR_PASSWORD
Email: YOUR_EMAIL@domain.com
```

## npm publish
npm publish用于将当前模块发布到npmjs.com。执行之前，需要向npmjs.com申请用户名。


## npm owner
模块的维护者可以发布新版本。npm owner命令用于管理模块的维护者。

```
# 列出指定模块的维护者
npm owner ls <package name>

# 新增维护者
npm owner add <user> <package name>

# 删除维护者
npm owner rm <user> <package name>
```

## semver规范（语义化版本）
> semver 约定一个包的版本号必须包含3个数字，格式必须为 MAJOR.MINOR.PATCH, 意为 主版本号.小版本号.修订版本号.

MAJOR 对应大的版本号迭代，做了不兼容旧版的修改时要更新 MAJOR 版本号

MINOR 对应小版本迭代，发生兼容旧版API的修改或功能更新时，更新MINOR版本号

PATCH 对应修订版本号，一般针对修复 BUG 的版本号

对于包作者（发布者），npm 要求在 publish 之前，必须更新版本号。npm 提供了 npm version 工具，执行 npm version major|minor|patch 可以简单地将版本号中相应的数字加1

常用命令
```
npm version patch 
```

对于包的引用者来说，我们需要在 dependencies 中使用 semver 约定的 semver range 指定所需依赖包的版本号或版本范围。npm 提供了网站 https://semver.npmjs.com 可方便地计算所输入的表达式的匹配范围。常用的规则示例如下表：

|  rang  |  含义  |  例子  |
| ---- | ---- | ---- |
|  ^2.2.1  |  指定的 MAJOR 版本号下, 所有更新的版本  |  匹配 2.2.3, 2.3.0; 不匹配 1.0.3, 3.0.1  | 
|  ~2.2.1  |  指定 MAJOR.MINOR 版本号下，所有更新的版本  |  匹配 2.2.3, 2.2.9 ; 不匹配 2.3.0, 2.4.5  |
|  >=2.1  |   版本号大于或等于 2.1.0  |  匹配 2.1.2, 3.1  |
|  <=2.2  |   版本号小于或等于 2.2  |  匹配 1.0.0, 2.2.1, 2.2.11  |
|  1.0.0 - 2.0.0  |  版本号从 1.0.0 (含) 到 2.0.0 (含)  |  匹配 1.0.0, 1.3.4, 2.0.0  |

任意两条规则，用空格连接起来，表示“与”逻辑，即两条规则的交集: 如 >=2.3.1 <=2.8.0 可以解读为: >=2.3.1 且 <=2.8.0

任意两条规则，通过 || 连接起来，表示“或”逻辑，即两条规则的并集: 如 ^2 >=2.3.1 || ^3 >3.2

[注] 除了这几种，还有如下更直观的表示版本号范围的写法:

- \* 或 x 匹配所有主版本
- 1 或 1.x 匹配 主版本号为 1 的所有版本
- 1.2 或 1.2.x 匹配 版本号为 1.2 开头的所有版本

[注] 在常规仅包含数字的版本号之外，semver 还允许在 MAJOR.MINOR.PATCH 后追加 - 后跟点号分隔的标签，作为预发布版本标签 - Prerelese Tags，通常被视为不稳定、不建议生产使用的版本。例如：

1.0.0-alpha  
1.0.0-beta.1  
1.0.0-rc.3

## npm5 新增package-lock 文件
> package-lock.json 的作用是锁定依赖安装结构，如果查看这个 json 的结构，会发现与 node_modules 目录的文件层级结构是一一对应的。

## npm5.2新增工具npx
> npx 的使用很简单，就是执行 npx `<command>` 即可，这里的 `<command>` 默认就是 ./node_modules 目录中安装的可执行脚本名。例如上面本地安装好的 webpack 包，我们可以直接使用 npx webpack 执行即可。如果使用npm webpack命令的话，会调用全局的webpack，但当全局的版本和项目的版本不一致的时候，我们还是可以使用npx来只调用项目中的webpack

## 最佳实践
- 使用 npm: >=5.1 版本, 保持 package-lock.json 文件默认开启配置
- 初始化：第一作者初始化项目时使用 `npm install <package>` 安装依赖包, 默认保存 ^X.Y.Z 依赖 range 到 package.json中; 提交 package.json, package-lock.json, 不要提交 node_modules 目录
- 初始化：项目成员首次 checkout/clone 项目代码后，执行一次 npm install 安装依赖包
- 不要手动修改 package-lock.json
- 升级依赖包
    - 升级小版本: 本地执行 npm update 升级到新的小版本
    - 升级大版本: 本地执行 npm install <package-name>@<version> 升级到新的大版本
    - 也可手动修改 package.json 中版本号为要升级的版本(大于现有版本号)并指定所需的 semver, 然后执行 npm install
    - 本地验证升级后新版本无问题后，提交新的 package.json, package-lock.json 文件
- 降级依赖包
    - 正确做法: npm install <package-name>@<old-version> 验证无问题后，提交 package.json 和 package-lock.json 文件
    - 错误做法: 手动修改 package.json 中的版本号为更低版本的 semver, 这样修改并不会生效，因为再次执行 npm install 依然会安装 package-lock.json 中的锁定版本
- 删除依赖包:
    - Plan A: npm uninstall <package> 并提交 package.json 和 package-lock.json
    - Plan B: 把要卸载的包从 package.json 中 dependencies 字段删除, 然后执行 npm install 并提交 package.json 和 package-lock.json

任何时候有人提交了 package.json, package-lock.json 更新后，团队其他成员应在 svn update/git pull 拉取更新后执行 npm install 脚本安装更新后的依赖包


## 小结
通过本文学习下npm的一些常用命令以及规范，semver的版本管理规范，以及npm的一些最佳实践方式
