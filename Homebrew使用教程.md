## homebrew 
homebrew是osx下的一个包管理工具，可以很方便的管理各类包。官方给出的定义是：macOS缺失的软件包管理器

## 安装
执行如下命令：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
上边的命令行时两个命令，首先下载install文件，然后用系统的ruby工具安装。

【查看版本】
`brew -v`
```
Homebrew 1.4.3
Homebrew/homebrew-core (git revision dba402; last commit 2018-01-09)
```

### 常用命令
1.安装卸载软件
```
brew --version或者brew -v 显示brew版本信息
brew install <formula> 安装指定软件
brew unistall <formula> 卸载指定软件
brew list  显示所有的已安装的软件
brew search text 搜索本地远程仓库的软件，已安装会显示绿色的勾
brew search /text/ 使用正则表达式搜软件
```

2.升级软件相关
```
brew update 自动升级homebrew（从github下载最新版本）
brew outdated 检测已经过时的软件
brew upgrade  升级所有已过时的软件，即列出的以过时软件
brew upgrade <formula>升级指定的软件
brew pin <formula> 禁止指定软件升级
brew unpin <formula> 解锁禁止升级
brew upgrade --all 升级所有的软件包，包括未清理干净的旧版本的包
```

3.清理相关

homebrew再升级软件时候不会清理相关的旧版本，在软件升级后我们可以使用如下命令清理
```
brew cleanup -n 列出需要清理的内容
brew cleanup <formula> 清理指定的软件过时包
brew cleanup 清理所有的过时软件
brew unistall <formula> 卸载指定软件
brew unistall <fromula> --force 彻底卸载指定软件，包括旧版本
```

**[注意]通过brew安装的文件会自动设置环境变量，所以不用担心命令行不能启动的问题。 比如安装好了gradle，即可运行 gradle -v**
