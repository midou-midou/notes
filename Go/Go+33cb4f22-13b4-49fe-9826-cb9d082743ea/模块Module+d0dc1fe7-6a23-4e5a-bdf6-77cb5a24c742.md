# 模块Module

### 概念：

是语义化版本管理的依赖项的包管理工具 #### 解决的问题： GOPATH下不能做包的版本管理，比如A项目依赖于v1版本，B项目依赖v2版本。pkg目录下不能放两个版本的包 #### 更新模块：

```Plain Text
go get package-path@vX.X.X #更新到指定版本go get package-path #默认更新到最新的版本
```

### 依赖本地包：

在`.mod`文件中`require`引入时，可以使用replace替换路径为本地路径

```Go
require (
    3rd/module/testmod v0.0.0
)

replace 3rd/module/testmod => /usr/local/go/testmod
```

### 私有化部署（CI）无网络环境部署：

```Go
go mod vendor
```

上面的命令可以将go项目依赖的包下载到项目的`vendor`目录下

```Go
go build -mod vendor
```

让go编译的时候从当前项目的vendor目录开始构建

