# 声明-类型type-新概念

在go的声明中，有四种声明方式var，const，type，func。分别对应变量，常量，类型和函数的声明

## 声明方法

```Go
type 类型名字 底层类型
```

上面的操作可以自定义一种类型来给其他的变量使用。

```Go
import "fmt"
type Celsius float64 // 摄氏温度
type Fahrenheit float64 // 华氏温度
const (
    AbsoluteZeroC Celsius = -273.15 // 绝对零度
    FreezingC Celsius = 0 			// 结冰点温度
    BoilingC Celsius = 100			// 沸水温度
)
func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }
func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```

上面的实例展示了自定义类型的作用，比如说，虽然华氏度和摄氏度是一样的底层类型`float64`。但是，可以在程序语义化的层面进行区分，不会将华氏度和摄氏度混到一起去做一些计算的工作

