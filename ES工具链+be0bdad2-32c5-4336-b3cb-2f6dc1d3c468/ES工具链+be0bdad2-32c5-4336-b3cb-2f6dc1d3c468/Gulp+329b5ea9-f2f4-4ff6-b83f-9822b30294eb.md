# Gulp

## 前言

首先，`Gulp`应该是一个构建工具。就包括jetkins、现在又有新的cicd工具（GitHub的cicd、gitlab的cicd）。同时，Gulp又可以处理js文件，预处理sass等css拓展文件，也就像webpack、vite一样。也可以配置热替换、热更新，启动本地服务器，模块化的支持（**但是没有专业的好**）等等

`Gulp`更应该是上面这一套流程的一个提供工具。但是，对于前端来说，`webpack`、`vite`提供了模块化的支持，且现在主流都是单页面的应用，都是通过`script module` 模块化引入每一个js文件。且这两个工具都提供了压缩，预处理文件等常用的功能，用这些专门针对性的文件也足够了

但是，不代表就不需要`Gulp`了，比如要定义一个`workflow` 工作流，上面俩工具就不行了。又一个但是，工作流都是用其他的cicd工具替代了（Github cicd、gitlab cicd）

项目中要按照需要来选择工具，有可能是两者都要，也有可能只要其中之一就足够了

## 概念

Gulp是一个工具，一个用于构建js工程的工具。可以打包js、转换sass、压缩js等

## 文档

[官方文档](https://gulpjs.com/docs/en/getting-started/quick-start)

