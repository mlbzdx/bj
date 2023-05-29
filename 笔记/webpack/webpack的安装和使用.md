# webpack简介

> webpack官网地址：https://www.webpackjs.com/

webpack是一个构建工具，它将开发时态（devtime）的所有文件均视为模块，然后将这些模块的文件打包成可以在运行时态（runtime）中使用的体量减小，依赖减少的文件。

# webpack安装

webpack提供了两个包，核心包和脚手架框架

* webpack,核心包，包含了webpack构建过程中所需要使用的api。
* cli框架，它提供了一些简单的命令，通过调用webpack核心包中的api来完成构建过程。

为了适用于不同webpack版本的工程，因此在使用npm进行安装时推荐采用本地安装的方式。

默认情况下，webpack会以`./src/indexjs`作为入口文件分析依赖关系，打包到`./dist/main.js`文件中。

本地安装后，在工程根目录下新建src目录以及其目录下的index.js。这些准备好后就可以书写代码了。之后通过`npx webpack`即可进行打包。

通过`--mode`选项可以控制webpack的打包结果的运行环境。

`--mode-development`开发环境的打包 通常配置在npm的dev脚本中

`--mode-production`生产环境的打包 通常配置在npm的build脚本中