# React中配置代理（防止CORS）

### 方法一：

在**package.json**中追加如下的配置`"proxy":"http://localhost:5000"`

缺点：只能在react项目中配置这一个代理 原理：请求了3000端口不存在的资源时，将请求发送给5000。但是如果3000端口已经存在的资源就会把请求发送给3000

### 方法二：

- 在src目录下新建配置文件（命名必须一致）`setupProxy.js`

- 在配置文件中写入

```JavaScript
const proxy = require('http-proxy-middleware')
module.exports = function(app) {
  app.use(
    proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
      target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
      changeOrigin: true, //控制服务器接收到的请求头中host字段的值
      /*
          changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
          changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
          changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
      pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
    }),
    proxy('/api2', { 
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {'^/api2': ''}
    })
  )
}
```

优点：可以配置多个代理，可以灵活的控制请求是否走代理。 缺点：配置繁琐，前端请求资源时必须加前缀

