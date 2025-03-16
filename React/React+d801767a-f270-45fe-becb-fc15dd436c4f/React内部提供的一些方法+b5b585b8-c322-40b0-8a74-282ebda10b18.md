# React内部提供的一些方法

### lazyload

配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包

```JavaScript
const Login = lazy(()=>import('@/pages/Login'))
```

**Suspense组件** 在加载制定的组件前可以先显示一个其他组件，用户交互良好的组件，给予用户正在加载中等的提示

```JavaScript
<Suspense fallback={<h1>loading.....</h1>}>
  <Switch>
    <Route path="/xxx" component={Xxxx}/>
    <Redirect to="/login"/>
  </Switch>
</Suspense>
```

