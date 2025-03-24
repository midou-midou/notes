# Go run / Go build

### Go run：

这是一个Go命令的提供的工具链，包括打包，编译，运行这些步骤

```Plain Text
go run main.go
```

如果是在一个包下直接运行上面的命令，只会对你指定的文件进行打包、编译、运行的操作比如，`main.go`中有使用在main package中的其他.go文件中的方法，运行上面的命令就会提示无法找到某某某函数，所以需要工具链打包整个包，编译整个包才行

```Plain Text
go run .
```

### Go build：

```Plain Text
go build .go build xxx.go
```

上面的命令会将指定的go文件或者整个包打包成一个可以在当前操作系统下执行的文件，Linux为一个脚本，Windows下为exe

