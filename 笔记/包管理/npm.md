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

## 包的配置

npm 将每个使用npm的工程本身都看作一个包，包的信息都需要通过一个名称固定的配置文件来描述

**配置文件的名称固定为: package.json**

### 初始化配置

在经过初始化配置之后，当前包的目录下都会多出package.json文件。

可以手动建立该文件，更多时候也可以通过命令`npm init`命令来创建。

配置文件中包含大量信息，包括：

* package name 包的名字，默认为当前文件目录名，但不能出现中文和特殊字符，必须为英文单词字符，支持连接符。
* version  当前版本，默认为1.0.0。版本有通用的版本规范，通常由主版本号，此版本号和补丁号组成。
  * 主版本号：仅当程序发生重大变化时才会增长，如新增了重要功能，新添加了大量API，技术架构发生了变化。
  * 次版本号：仅当程序发生了一些小变化后才会增长，如新增了一些小功能，新添加了一些辅助API。
  * 补丁版本号：仅当解决了一些Bug，或者进行了一些局部优化后才会增长。如解决了某个函数的bug或者提高了某个函数的运行效率。
* description 包的描述
* entry point 包的入口模块 默认为index.js
* test command 测试命令
* git repository git或者svn的仓库地址
* keywords 搜索关键词。包发布在npm官网后，搜索该关键词可以找到该包。
* author 包的作者必须是有效的npm账户名，书写规范是 `acount <mail>`，例如：`mlbzdx <mlbzdx@outlook.com>`，不正确的账号会导致包发布失败
* license 协议

![image-20230519155612720](https://s1.vika.cn/space/2023/05/19/7f7d3444602249a29f219241a95d7e76)

```json
{
  "name": "practice",
  "version": "1.0.0",
  "description": "'尝试初始化 创建一个包'",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+ssh://git@github.com/mlbzdx/practice.git"
  },
  "author": "mlbzdx",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/mlbzdx/practice/issues"
  },
  "homepage": "https://github.com/mlbzdx/practice#readme"
}

```

如果想简化手动配置的流程，可以执行`npm init --yes`或其简写`npm init -y`。

![image-20230519160441004](https://s1.vika.cn/space/2023/05/19/dd528904c0ca4fcbbe852145a4fe8698)

```json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

### 依赖关系的配置

##### 手动配置依赖关系

package.json文件除了提供关于包的信息之外，最重要的作用的就是**记录当前工程包的依赖**

* dependencies 生成环境的依赖包

  ```json
  "dependencies" : {
  //	"依赖包名" ： "依赖包的版本",
  	"jquery" : "latest", //latest表示最新版本，不建议使用这样的写法
  	"lodash" : "4.17.15"
  }
  ```

  

* devDependencies 开发环境的依赖包

  ```json
  "devDependencies" : {
  	"mocha" : "6.2.2"
  }
  ```

配置好依赖关系后，就可以使用命令来安装相关的依赖包了。

* `npm install` 或者 `npm i` 同时安装生产环境和开发环境的依赖。
* `npm install --production` 只安装生产环境的依赖

这样在多人开发中，只需要通过package.json配置文件即可完成相关依赖包的安装，不需要上传复杂庞大的node_modules文件目录。

##### 安装时自动配置依赖关系

现在使用`npm i 包名`安装相关包时，会自动将包的依赖关系加入到package.json文件的生产环境配置中，而npm以往的旧版本执行该命令时不会添加，在安装时需要执行`npm i --save 包名 `或`npm i -S 包名`才会将依赖关系到package.json文件的生产环境配置中。

如果需要在安装时自动配置到开发环境中，那么在安装时需要执行`npm i --save-dev 包名`或`npm i -D 包名`。

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

### 安装包后命令的使用

一些包安装好后可以提供一些命令工具。常见为带有CLI包名的安装包，这类型的包会提供一些命令工具。但这些命令工具不能直接使用。

* 提供命令工具的包为全局安装，则需要将其配置到环境变量，重启系统后才能使用该命令。
* 提供命令工具的包为本地安装，如带有CLI包名的安装包，npm 会将它的CLI脚本文件放在node_modules/.bin下，使用命令`npx  命令名`即可执行该命令。(npx 命令是安装npm安装完成后配置的命令)

## 包的使用

在相关的包安装好后，package.json文件中依赖配置内容会发生变化。同时，文件根目录下会多出node_modules目录（用于存放本地安装的包）和package-lock.json的文件。这里包的使用，就是针对node_modules目录下存放的包的使用。

首先，要使用包，就必须正确的导入包。而要导入相应的包，则需要了解包的入口文件以及包的导入方式和逻辑。

### 包的入口文件

安装的包，会以该包名的文件目录形式，存放在node_modules文件目录下。

如，下图中package.json文件的生产环境依赖配置显示安装了lodash和jquery两个包，在node_modules文件目录下则多出了lodash和jquery两个文件目录。

![image-20230520161705810](https://s1.vika.cn/space/2023/05/20/3110c17ac5ed4c50b673ce1c1797ab60)

但在具体导入时，文件目录不能直接导入，第三方包一般会将该包提供的相关功能封装导入到一个入口文件中，我们在自己的模块需要使用时，导入该入口文件即可。

那么，如何确定第三方包的入口文件以及它的位置？（这里是相对于包根目录的位置，具体在自己模块使用时的路径需要进行相应调整）

1. 查找包名的文件目录下，是否直接存在`包名.js`文件，本例中即`node_modules/lodash/lodash.js`。
2. 如果没有`包名.js`文件，则查看是否存在`包名/入口文件`，即：node_modules/lodash/入口文件
   * 这里入口文件的确认需要查看安装包名目录下的package.json文件，读取main字段后的文件作为入口文件。

![image-20230520170012358](https://s1.vika.cn/space/2023/05/20/c4f740df34224f1e97e477f2e04dcd4d)

由图可知，lodash的入口文件存放在lodash.js，由此可知，该包入口文件的相对路径为`node_modules/lodash/lodash.js`

* 若不包括main字段，则以index.js作为入口文件

  在lodash该包中比较特殊，同时提供了index.js文件，因为该文件导入了上面的lodash.js，也可作为入口文件导入。![image-20230520173239554](https://s1.vika.cn/space/2023/05/20/e0271846b39f496f9568801a1a64838e)

### 包的导入逻辑

在node环境中对模块进行导入时，如果每次都要先确认第三方包的入口文件，再使用相对路径去导入的话增加了工作量。

为此，可以将相对路径省略简写，帮忙确认入口文件并导入。

```
require('包名')
```

导入文件后查找入口文件的流程：

* 会默认在node_modules模块中查询入口文件。

  如果导入的模块是特殊模块（node的内置模块），会在node_modules生成的内置模块文件目录处查询文件。例如fs。

  如果是第三方包，则直接查找node_modules文件对应包名目录下的模块。

  这是自己书写的模块导入时必须使用相对路径的原因，因为默认查找的第三方包存放的node_modules目录中不存在自己当前项目写的模块。

* 没有再返回上一级文件目录中查询入口文件，直到顶级的盘符目录都不存在，抛出错误。这样能既能查找到从本地安装的包，也能查找到全局安装的包，保证安装的包能够使用。

## 卸载包

我们可以使用以下命令来卸载 Node.js 模块。

```
$ npm uninstall <Module Name>   #卸载模块全写
$ npm un <Module Name>          #卸载模块简写
```

## 查看安装包

你可以使用以下命令来查看所有全局安装的包

```
$ npm list -g
```

如果要查看某个包的版本号，可以使用命令如下：

```
$ npm list <Module Name>
```

查看包的所有的版本信息

```
npm view <Module Name> versions
```

查看包安装的位置

```
npm root  [-g]
//[-g] 表示可选，有该指令表示查看包安装的全局路径，没有表示查看本地包安装的指令。
```

## 脚本使用

npm 脚本(npm scripts)
在开发的过程中我们可能会反复使用很多的CLI 命令，例如：

* 启动工程命令（node 或 一些第三方包提供的CLI命令)
* 部署工程命令（一些第三方包提供的CLI命令）
* 测试工程命令（一些第三方包提供的CLI命令)

这些命令纷繁复杂，根据第三方包的不同命令也会不一样，非常难以记忆。于是，npm 非常贴心的支持了脚本，只需要在package.json 中配置scripts 字段，即可配置各种脚本名称。之后，我们就可以运行简单的指令来完成各种操作了，运行方式是npm run 脚本名称
不仅如此，npm 还对某些常用的脚本名称进行了简化： 

* npm start
* npm stop
* npm test

## 更新模块（包）

查看哪些包可以更新

```
npm outedate  <Module Name>
```



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