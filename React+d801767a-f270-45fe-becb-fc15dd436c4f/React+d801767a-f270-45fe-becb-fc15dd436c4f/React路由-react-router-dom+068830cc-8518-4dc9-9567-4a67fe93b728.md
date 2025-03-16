# React路由-react-router-dom v5

概念：web应用中提供良好的导航功能

### Hashroute

用法： 包裹在使用路由的组件的最外层，路由方式为Hash route。

说明：

- 是前端路由，前端会匹配URL的变化并做处理，不会给后端发送请求的页面

- 匹配的路径带有一个`#`和后面的部分，作为匹配的路径（匹配的结果）

- 锚点匹配

- 刷新后会导致路由state参数的丢失

原理： `window.location.hash`

```JavaScript
import React from 'react';
import {HashRouter, Route, Switch} from 'react-router-dom';
import Home from '../home';
import Detail from '../detail';
const BasicRoute = () => (
    <HashRouter>
        <Switch>
            <Route exact path="/" component={Home}/>
            <Route exact path="/detail" component={Detail}/>
        </Switch>
    </HashRouter>
);
export default BasicRoute;
```

### BrowerRouter

用法：同上 说明：

- 服务器匹配，会把请求的路径发送给服务器，要等待服务器的返回

- 使用的是H5的history API，不兼容IE9及以下版本

- 路径中不包含’#’

- 刷新后的state会保存在history对象中

代码同上

### 传参数的问题

三种传参数的方法

- params参数

- search参数

- state参数

1.**params参数** 	路由链接(携带参数)：`<Link to='/demo/test/tom/18'}>详情</Link>` 	注册路由(声明接收)：`<Route path="/demo/test/:name/:age" component={Test}/>` 	接收参数：`this.props.match.params` 2.**search参数** 	路由链接(携带参数)：`<Link to='/demo/test?name=tom&age=18'}>详情</Link>` 	注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>` 	接收参数：`this.props.location.search` 	备注：获取到的search是urlencoded编码字符串，需要借助querystring解析 3.**state参数** 	路由链接(携带参数)：`<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>` 	注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>` 	接收参数：`this.props.location.state` 	备注：刷新也可以保留住参数

### Link

```JavaScript
<div className="list-group">
  {/* 原生html中，靠<a>跳转不同的页面 */}
  {/* <a className="list-group-item" href="./about.html">About</a>
                <a className="list-group-item active" href="./home.html">Home</a> */}
  {/* 在React中靠路由链接实现切换组件--编写路由链接 */}
  <Link className="list-group-item" to="/about">About</Link>
  <Link className="list-group-item" to="/home">Home</Link>
</div>
```

路由链接

### Route

注册路由

```JavaScript
<div className="panel-body">
  {/* 注册路由 */}
    <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
</div>
```

### NavLink

较Link相比，可以使用`activeClassName`属性来制定激活的样式

### Switch

负责组件的匹配，且会匹配第一个匹配到的组件，能提高匹配的效率

```JavaScript
{/* 注册路由 */}
<Switch>
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Route path="/home" component={Test}/>
</Switch>
```

### Redirect

默认匹配，默认匹配到指定的组件

```JavaScript
<Switch>
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Redirect to="/about"/>
</Switch>
```

### 严格匹配和模糊匹配

```JavaScript
<Route exec={true} path="/home" component={Home} />
```

开启严格匹配（精确匹配）后，会按照path的顺序严格匹配，即请求的URL必须和path一致才可以匹配成功

### withRouter

可以加工一般组件，使其具有路由组件特有的API。会往一般组件上添加history、location、match到props中去

```JavaScript
import React from 'react'
import withRouter from 'react-router-dom'
 function Navheader() {
    return (
        <div className="navbar-heading">
            <div
            onClick={()=> props.history.push('/')} //点击的时候跳转到首页
            className="navbar-brand">返回首页</div>
        </div>
    )
}
export default withRouter(Navheader)
```



# React路由-react-router-dom v6



