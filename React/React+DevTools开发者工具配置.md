# React DevTools开发者工具配置

![https://static.xuyanshe.club/img/1648391772898-4afcad18-f0be-4d6d-879b-1a0cd707e223.png](React+DevTools开发者工具配置\1648391772898-4afcad18-f0be-4d6d-879b-1a0cd707e223.png)

在chrome插件商店中添加

在store下的index.js或者是store.js中的`createStore()`中的**第二个参数**要填写

```JavaScript
window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
```

填写好的如下

```JavaScript
export default createStore(allReducer,
window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());
```

