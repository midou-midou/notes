# Go对json、xml、yaml等标准格式编码的支持

在go中有对这些标准的编码格式支持的包`encoding/json`、`encoding/xml`、 `encoding/asn1`等结构体成员“ini:”和后面对应于ini文件中的变量要没有空格，其他的json、yaml都要求后面没有空格

json： 定义一个Movie对象的类型，下面是例子。这个对象非常适合json格式，预定义好一个和json中的对象一样的go结构体，`json.unmarshal()`的时候可以直接将json字符串解析成一个预定义好的go对象

```Go
type Movie struct {
     Title  string
     Year   int  `json:"released"`
     Color  bool `json:"color,omitempty"`
     Actors []string
}
```

上面的line3、line4是`tag结构体成员`tag结构体成员可以指明在json序列化的时候将Year、Color替换为指定的变量（上面将Year替换为released，Color替换成为color，并且omitempty选项设置了Go结构体成员为空或者为零值时不进行转换为json格式的过程

编码（转换成json）

```Go
data, err := json.Marshal(movies)
```

**输出**

![https://cdn.nlark.com/yuque/0/2022/png/22573238/1650166769739-60294faf-0f05-4573-8a1d-165c43ee3be4.png#averageHue=%23f1f1f1&clientId=ub0d3d001-d2d2-4&from=paste&height=136&id=u8802d74b&name=image.png&originHeight=272&originWidth=1602&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54864&status=done&style=none&taskId=ua437757f-1bf5-47e7-8d6d-a0c43648ba2&title=&width=801](https://cdn.nlark.com/yuque/0/2022/png/22573238/1650166769739-60294faf-0f05-4573-8a1d-165c43ee3be4.png#averageHue=%23f1f1f1&clientId=ub0d3d001-d2d2-4&from=paste&height=136&id=u8802d74b&name=image.png&originHeight=272&originWidth=1602&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54864&status=done&style=none&taskId=ua437757f-1bf5-47e7-8d6d-a0c43648ba2&title=&width=801)

上面的输出格式不便于观察，所以还有一种转换的方式

```Go
data, err := json.MarshalIndent(movies, "", "    ")
```

- 上面方法的第二个参数是：每行输出前缀

- 第三个参数是：每一个层级的缩进

**输出**

（因为图片太长仅演示了一部分的输出）：

![https://cdn.nlark.com/yuque/0/2022/png/22573238/1650166919942-1265c496-67db-4c22-924a-1d8822770c90.png#averageHue=%23f9f9f9&clientId=ub0d3d001-d2d2-4&from=paste&height=217&id=ubc3cfa8e&name=image.png&originHeight=434&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26251&status=done&style=none&taskId=ue339483e-385b-48ae-8bf5-e270145d6cf&title=&width=307](https://cdn.nlark.com/yuque/0/2022/png/22573238/1650166919942-1265c496-67db-4c22-924a-1d8822770c90.png#averageHue=%23f9f9f9&clientId=ub0d3d001-d2d2-4&from=paste&height=217&id=ubc3cfa8e&name=image.png&originHeight=434&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26251&status=done&style=none&taskId=ue339483e-385b-48ae-8bf5-e270145d6cf&title=&width=307)

#### 解码（json转换成Go对于类型）：

```Go
json.Unmarshal(data, &titles)
```

输出：

```Go
[{Casablanca} {Cool Hand Luke} {Bullitt}]
```

### Yaml：

注意，yaml映射到go结构体。go结构体里面到成员必须开头大写，大写完后面到tag结构体成员必须要对应yaml中定义的变量名称

```YAML
pops:
  - code: NzZhZDFlNWI5NzNhNDQ3MWI3YjJlZTE3ZjIwMmE3Mjk=
    id: 0704f88a-a9b3-4d35-8293-631f291bc5f9
  - code: ODIxOTc5YTk1ZjUyNDA2NWI1Njk1OWY3Njg2YjdlYWU=
    id: 8083d6cf-a84c-4cc4-9753-dc73f9e1ecf4
version: 0
```

对应的结构体成员

```Go
type AuthPop struct {
	Id   string `yaml:"id"`
	Code string `yaml:"code"`
}
type AuthPops struct {
	Pops    []AuthPop `yaml:"pops"`
	Version string    `yaml:"version"`
}
func main(){
    yamlfile, err := ioutil.ReadFile("my.yaml")
    if err := yaml.Unmarshal(yamlfile, &pops); err != nil {
		panic(err.Error())
	}
}
```

### ini:

ini文件解析要注意，tag结构体成员“ini:”和后面对应于ini文件中的变量要没有空格

```Go
type SkyConfigConfigMap struct {
	Global Global `ini:"global"`
	Pop    Pop    `ini:"pop"`
}
// 下面是错误的tag结构体成员的写法
type SkyConfigConfigMap struct {
	Global Global `ini: "global"`
	Pop    Pop    `ini: "pop"`
}
```

