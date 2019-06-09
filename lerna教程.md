## 前言
在最初开开发package的时候，还属于一种刀耕火种的阶段。没有什么自动化的工具。发布package的时候，都是手动修改**版本号**。如果packages数量不多还可以接受。但是当数量逐渐增多的时候，且这些packages之间还有依赖关系的时候，对开发人员来说，就很痛苦了。工作不仅繁琐，而且需要用掉不少时间。

举个例子，如果你要维护两个package。分别为module-A,module-B。 下面是这两个package的依赖关系。

module-A
```
{
  "name": "module-A",
  "version": "1.0.0",
  "author": "",
  "license": "",
  "description": "",
  "scripts": {
  },
  "dependencies": {
    "module-B": "^1.0.0"
  },
  "private": true,
}
```
  
module-B
```
{
  "name": "module-B",
  "version": "1.0.0",
  "author": "",
  "license": "",
  "description": "",
  "scripts": {
  },
  "dependencies": {
  },
  "private": true,
}
```
在这样的环境下，module-A是依赖module-B的。如果module-B有修改，需要发布。那么你的工作有这些。

1.  修改module-B版本号，发布。
2.  修改module-A的依赖关系，修改module-A的版本号，发布。

这还仅仅只有两个package,如果依赖关系更复杂，大家可以想想发布的工作量有多大。

## 什么是lerna?为什么要使用lerna?
lerna到底是什么呢？lerna官网上是这样描述的。
> A tool for managing JavaScript projects with multiple packages.

引入lerna后，上面提到的问题不仅迎刃而解，更为开发人员提供了一种管理多packages javascript项目的方式。

1.  自动解决packages之间的依赖关系
2.  通过`git`检测文件改动，自动发布
3.  根据`git`提交记录，自动生成CHANGELOG

一个基础lerna仓库结构如下：
```
my-lerna-repo/
    ┣━ packages/
    ┃     ┣━ package-1/
    ┃     ┃      ┣━ ...
    ┃     ┃      ┗━ package.json
    ┃     ┗━ package-2/
    ┃            ┣━ ...
    ┃            ┗━ package.json
    ┣━ ...
    ┣━ lerna.json
    ┗━ package.json
```

## 使用lerna的基本工作流

### 准备工作

1. git仓库(自行安装)
2. npm仓库注册开通(参考文章>>>[npm发线上包(npm publish)](npm发线上包npmpublish.md))
3. 全局安装lerna
```
npm install lerna -g
```

### 初始化一个lerna项目
1、创建文件夹
```
mkdir lerna-demo && cd $_
```

2、初始化lerna工程
```
lerna init
```

执行成功后，目录下将会生成这样的目录结构
```
lerna-demo/
    ┣━ packages/
    ┣━ lerna.json
    ┗━ package.json
```

## 创建模块
> Lerna 提供了两种创建或导入模块的方式，分别是 create，import。

### create
> 创建一个 lerna 管理的模块。基本命令格式如下：

```
lerna create packageName [loc]
```

[注意]packageName必须是唯一的(npm仓库中无重名已发布包)

```
lerna create @lion/package-a
```
命令执行完后，lerna 会帮我们在指定位置创建模块的文件夹，同时会默认在该文件夹下执行 npm init 的命令，在终端上根据根据提示填写所有信息后会帮我们创建对应的 package.json 文件，大致的结构如下
```
lerna-demo/
    ┣━ packages/
    ┃     ┗━ package-a/
    ┃            ┣━ ...
    ┃            ┗━ package.json
    ┣━ lerna.json
    ┗━ package.json
```

### import
导入一个已存在的模块，同时保留之前的提交记录，方便将其他正在维护的项目合并到一起。基本命令格式如下：`lerna import dir`

dir 是本项目外的包含 npm 包的 git 仓库路径（相对于本项目根路径的相对路径）


### 查看模块列表
创建完毕之后，我们可以通过 list 命令来查看和确认现在管理的包是否符合我们的预期，执行如下命令：
```
lerna list
```

### 添加依赖包
现在我们来添加依赖包，在 lerna 项目里，你可以分别给每个模块单独添加依赖包，也可以同时给部分或全部模块添加依赖包，还可以把管理的某些模块作为依赖添加给其他模块。
```
lerna add [@version] [--dev] [--exact]
```
[注意]--dev 和 --exact 等同于 npm install 里的 --dev 和 --exact

当我们执行此命令后，将会执行下面那2个动作：

1. 在每一个符合要求的模块里安装指明的依赖包，类似于在指定模块文件夹中执行 `npm install <package>`。
2. 更新每个安装了该依赖包的模块中的 `package.json` 中的依赖包信息

```
# 在 package-a 这个模块里安装 lodash 依赖
$ lerna add lodash --scope @lion/package-a

# 在 package-a 这个模块里安装 lodash 依赖，并作为 devDependencies
$ lerna add lodash --scope @lion/package-a --dev

# 在所有模块中安装 @lion/package-a 这个依赖除了 package-a 自己
$ lerna add @lion/package-a

# 在所有模块里安装 lodash 依赖
$ lerna add lodash
```

### 安装依赖包
lerna 通过 bootstrap 命令来快速安装所有模块所需的依赖包。基本命令如下
```
lerna bootstrap
```
当执行完上面的命令后，会发生以下的行为：

1. 在各个模块中执行 npm install 安装所有依赖
2. 将所有相互依赖的 Lerna 模块 链接在一起
3. 在安装好依赖的所有模块中执行 npm run prepublish
4. 在安装好依赖的所有模块中执行 npm run prepare

### 清理依赖包
可以通过 clean 命令来快速删除所有模块中的 node_modules 文件夹。基本命令如下：
```
lerna clean
```

### 版本迭代
lerna 通过 version 命令来为各个模块进行版本迭代。基本命令如下：

```
 lerna version [major | minor | patch | premajor | preminor | prepatch | prerelease]
```

如果不选择此次迭代类型，则会进入交互式的提示流程来确定此次迭代类型
```
$ lerna version 1.0.1 # 按照指定版本进行迭代
$ lerna version patch # 根据 semver 迭代版本号最后一位
$ lerna version       # 进入交互流程选择迭代类型 
```

当执行此命令时，会发生如下行为：

1. 标记每一个从上次打过 tag 发布后产生更新的包
2. 提示选择此次迭代的新版本号
3. 修改 package.json 中的 version 值来反映此次更新
4. 提交记录此次更新并打 tag
5. 推送到远端仓库

> 你可以在执行此命令的时候加上 ——no-push 来阻止默认的推送行为，在你检查确认没有错误后再执行 git push 推送

### –conventional-changelog
```
$ lerna version --conventional-commits
```

version 支持根据符合规范的提交记录在每个模块中自动创建和更新 CHANGELOG.md 文件，同时还会根据提交记录来确定此次迭代的类型。只需要在执行命令的时候带上 --conventional-changelog 参数即可

### –changelog-preset
```
$ lerna version --conventional-commits --changelog-preset angular-bitbucket
```
changelog 默认的预设是 angular，你可以通过这个参数来选择你想要的预设创建和更新 CHANGELOG.md

> 上述 2 个参数也可以直接写在 lerna.json 文件中，这样每次执行 lerna version 命令的时候就会默认采用上面的 2 个参数

```
"command": {
  "version": {
    "conventionalCommits": true,
    "changelogPreset": "angular"
  }
}
```

## 发版
在一切准备就绪后，我们可以通过 publish 命令实现一键发布多个模块。基本命令如下：
```
$ lerna publish
```
当执行此命令时，会发生如下行为：

1. 发布自上次发布以来更新的包(在底层执行了 lerna version，2.x 版本遗留的行为)
2. 发布当前提交中打了 tag 的包
3. 发布在之前的提交中更新的未经版本化的 “canary” 版本的软件包（及其依赖项）

注意： Lerna 不会发布在 package.json 中将 private 属性设置为 true 的模块，如果要发布带域的包，你还需要在 ‘package.json’ 中设置如下内容：
```
"publishConfig": {
    "access": "public"
  }
```

由于我们之前已执行过 lerna version 命令，这里如果直接执行 lerna publish 会提示没有发现有更新的包需要更新，我们可以通过从远端的 git 仓库来发布：
```
$ lerna publish from-git
```

在确认后 lerna 就会帮我们把所有更新后的模块都发布在 npm 仓库里，当然在这之前你要做好发布 npm 包的一些准备，比如在 npm 注册账号，并在本地 npm adduser 等(查看>>>[npm发包](npm发线上包npmpublish.md))

此时我们去 npm 上就能看到新发布上去的模块了


## lerna的两种模式

lerna有两种工作模式,Independent mode和Fixed/Locked mode。  
lerna的默认模式是Fixed/Locked mode，在这种模式下，实际上lerna是把工程当作一个整体来对待。每次发布packages，都是全量发布，无论是否修改。但是在Independent mode下，lerna会配合Git，检查文件变动，只发布有改动的package。

lerna.json
```
{
  "lerna": "3.5.1",
  "packages": ["packages/*"],
  "version": "independent", <-- 这里设置
  "stream": true,
  "command": {
    "publish": {
      "npmClient": "npm",
      "allowBranch": "develop",
      "verifyAccess": false,
      "message": "chore(release): publish",
      "registry": "https://registry.npm.local"
    }
  },
  "conventionalCommits": true
}
```
