## 前言
每次想要更新下node.js都不记得该如何去处理，特此笔记下

## 步骤
1、查看本机`node.js`版本
```
node -v
```

2、清除`node.js`的`cache`
```
sudo npm cache clean -f
```

3、安装n工具(已安装则可以忽略)，这个工具是专门用来管理`node.js`版本的
```
sudo npm install -g n
```
4、安装最新版本的`node.js`
```
sudo n stable
```

5、更新 npm到最新版本
```
sudo npm install npm@latest -g
```

6、验证
```
node -v
npm -v
```

## 小结
经历上诉步骤一般都是可以安装成功的
