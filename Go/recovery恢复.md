# recovery恢复

概念：定义在 `defer`关键字后的延迟函数中。使用内置函数 `recover()`来执行恢复操作。执行的流程如下

![https://cdn.nlark.com/yuque/0/2022/jpeg/22573238/1664421337675-ecdd2fb3-2b62-4d00-bce6-48d3a7b701f7.jpeg](recovery恢复/1664421337675-ecdd2fb3-2b62-4d00-bce6-48d3a7b701f7.jpeg)

具体的例子如下

```Go
failTime := make(chan int)
go func(failTime chan int) {
    defer func() {
        if p := recover(); p != nil {
            fmt.Println("compress is recover", p)
        }
    }()
    fmt.Println("start compress")
    for i := 0; i < 10; i++ {
        // 假设第七次发生了错误
        if i == 7 {
            failTime <- 1
            panic("compress is error")
        }
    }
}(failTime)
count := <-failTime
fmt.Printf("compress is done, fail:%d, success:%d", count, (10 - count))
```

上面函数模拟对一个文件压缩多次，记录其中失败的次数（或者编号）从而重试压缩程序运行的结果如下

```Go
start compress
compress is recover compress is error
compress is done, fail:1, success:9
```

`recover()`函数的使用非常的简单，并且在没有错误的情况下调用函数会返回一个`nil`

