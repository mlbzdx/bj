# npm

> npm的介绍可以参考菜鸟教程中的文档:https://www.runoob.com/nodejs/nodejs-npm.html

## npm概述：

npm是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题

## 配置镜像源

npm的官方服务器registry位于国外，网络响应慢，还有可能连接失败。

因此使用npm安装包之前，需要配置好npm的国内镜像源。

> 国内镜像地址 ：https://registry.npm.taobao.org/

配置命令：

```
$ npm config set registry https://registry.npm.taobao.org/
```

检查配置命令：

```
$ npm config get registry
```

配置成功后返回镜像地址

![image-20230518175822017](https://s1.vika.cn/space/2023/05/18/fedf1dcb9d604bbe8ad734a20b896e66)

使用淘宝镜像的命令：

```
$ npm install -g cnpm --registry=https://registry.npmmirror.com
```

## 包的安装

### 包的安装指令

```
$ npm install <Module Name>  #安装模块全写
$ npm i <Module Name>        #安装模块简写
$ npm i <Module Name>@版本号  #安装指定版本的模块（包）
$ npm i <Module Name> <Module Name> ... #同时安装多个包 
```

### 包的安装方式

#### 全局安装

全局安装的包并非所有工程可用，它仅提供全局的CLI工具，大部分情况下，都不需要全局安装包，除非：

1. 包的版本很稳定，很少有大的更新
2. 提供的CLI工具在各个工程中使用非常频繁
3. CLI 工具仅为开发环境提供支持，而非部署（生产）环境

```
$ npm install --global <Module Name> #全局安装
$ npm i -g <Module Name> #全局安装简写
```

#### 本地安装

##### 安装普通依赖

```
$ npm install <Module Name>           # 本地安装
```

1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。 
2. 可以通过 CommonJS或者Es Moudle来引入本地安装的包。

##### 安装开发依赖

```
$ npm install <Module Name> -D        #安装开发依赖
```

1. 开发依赖仅在开发环境中使用，打包后不会呈现在最终的代码里。

### 安装包的存放与删除

#### 查看安装包的存放目录

* 使用npm进行本地安装命令后，会在当前目录下创建一个node_modules目录存放下载的包，在实际开发时经常将开发根目录作为当前目录后再安装包。
* 使用npm进行全局安装命令后，默认会在本机的node_global目录下建立node_modules目录存放安装包，这个目录的地址可以通过指令`npm config get prefix`来查看。
* 使用npm 安装某个包时，npm会分析该包的依赖树，因此还会安装该包依赖的其他包。

#### 删除安装包存放目录

直接在代码编辑器如vscode中删除node_modules文件目录时如果安装包过多，程序运行缓慢，容易卡死。推荐删除方式：

* 直接在node_modules安装目录下的CMD控制台中运行rmdir /s node_modules命令进行删除
* 或者关闭代码编辑器和命令控制台后，在资源文件目录窗口删除

### 安装包后指令的使用

一些包安装好后可以提供一些命令工具。常见为带有CLI包名的安装包，这表明该包会提供一些命令工具。但这些命令工具不能直接使用。

* 提供命令工具的包为全局安装，则需要将其配置到环境变量，重启系统后才能使用该命令。
* 提供命令工具的包为本地安装，如带有CLI包名的安装包，npm 会将它的CLI脚本文件放在node_modules/.bin下，使用命令npx 命令名即可执行该命令。(npx 命令是安装npm安装完成后配置的命令)

### 查看安装包

你可以使用以下命令来查看所有全局安装的模块

```
$ npm list -g
```

如果要查看某个模块的版本号，可以使用命令如下：

```
$ npm list <Module Name>
```

查看包所有的版本

```
npm view <Module Name> versions
```

## 包的配置

npm 将每个使用npm的工程本身都看作一个包，包的信息都需要通过一个名称固定的配置文件来描述

**配置文件的名称固定为: package.json**

可以手动建立该文件，更多时候也可以通过命令`npm init`命令来创建。

## 卸载模块（包）

我们可以使用以下命令来卸载 Node.js 模块。

```
$ npm uninstall <Module Name>   #卸载模块全写
$ npm un <Module Name>          #卸载模块简写
```

## 更新模块（包）

我们可以使用以下命令更新模块：

```
$ npm update <Module Name>
```

## 搜索模块（包）

使用以下来搜索模块：

```
$ npm search <Module Name>
```

## 添加.gitignore文件

npm 安装的包只在开发的时候使用，在上传到github或gitee等平台进行实际应用时，需要忽略node_modules目录的上传。