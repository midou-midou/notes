# Context包

在goroutine之间进行上下文的传递，相同的context可以传递给不同goroutine中的函数，goroutine可以携带一些参数，传递给需要使用的goroutine，也可以对goroutine进行控制，也就是所谓的并发控制

### 创建context

```Go
context.Background()    // 上下文默认值，其他的context都应该从context中派生出来context.TODO()          // 不确定context类型时使用
```

### 使用context提供的with函数

```Go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
func WithValue(parent Context, key, val interface{}) Context
```

### 携带控制、超时控制、取消控制

- 携带控制：`withValue`

- 超时控制：`withTimeout`和`withDeadline`

- 取消控制：`withCancel`

