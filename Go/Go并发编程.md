---
title: Go并发编程
tags: Go
categories:
  - 学习呦
date: 2025-10-14 21:01:35
---

# Go并发编程

### 前置知识：

### go runtine:

一个简单的`go`关键字就可以开启一个协程

```Go
go test()
go func(str string){    
	// do something
}(abc)
```

协程作为go语言中定义的一个轻量级的线程，

