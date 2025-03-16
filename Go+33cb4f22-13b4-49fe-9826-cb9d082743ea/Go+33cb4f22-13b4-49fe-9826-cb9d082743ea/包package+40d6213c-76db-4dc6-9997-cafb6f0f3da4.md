# 包package

### 概念：

包-类似于命名空间，类库相关概念，但是不等同于这两个概念

**main包**： 每一个Go应用程序只能有一个main包，main包下可以有多个文件，但是所有的这些.go文件中只能有一个文件带有main函数的，其他的都不可以带main函数，main函数就是这个包的入口。当然，也不是说开发的包必须有main入口，下面看fmt包的结构

![https://cdn.nlark.com/yuque/0/2022/png/22573238/1649857882771-0e603218-a9b8-4d47-9af0-c88be2a9ce88.png#clientId=ua864ae96-c647-4&from=paste&height=310&id=u7ca18215&name=image.png&originHeight=620&originWidth=552&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129739&status=done&style=none&taskId=u20ea2388-9de9-414c-a2e3-8cf11608589&title=&width=276](https://cdn.nlark.com/yuque/0/2022/png/22573238/1649857882771-0e603218-a9b8-4d47-9af0-c88be2a9ce88.png#clientId=ua864ae96-c647-4&from=paste&height=310&id=u7ca18215&name=image.png&originHeight=620&originWidth=552&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129739&status=done&style=none&taskId=u20ea2388-9de9-414c-a2e3-8cf11608589&title=&width=276)

每一个.go文件开头都是指定了属于fmt包

![https://cdn.nlark.com/yuque/0/2022/png/22573238/1649858063482-6abeab4f-a893-488a-958c-9075ba89766f.png#clientId=ua864ae96-c647-4&from=paste&height=186&id=u4e945eaf&name=image.png&originHeight=452&originWidth=1082&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57110&status=done&style=none&taskId=u8faf8048-9fe0-401f-98c7-d7ca77feb23&title=&width=445](https://cdn.nlark.com/yuque/0/2022/png/22573238/1649858063482-6abeab4f-a893-488a-958c-9075ba89766f.png#clientId=ua864ae96-c647-4&from=paste&height=186&id=u4e945eaf&name=image.png&originHeight=452&originWidth=1082&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57110&status=done&style=none&taskId=u8faf8048-9fe0-401f-98c7-d7ca77feb23&title=&width=445)

**包的导入**：import导入即可，但是一般不导入当前目录下的包。默认导入的包会去GOPATH去找，找src文件夹下的包

因为不好版本管理，所以现在都是用module模块来管理包 #### 包的导出： 默认导出的，必须是大写字母开头的函数，变量等

