# Hooks

概念：在函数式组件中可以使用state等一些react特性

### State Hooks

`React.useState()` 让函数组件也可以有state状态, 并进行状态数据的读写操作 写法：`const [xxx, setXxx] = React.useState(initValue)` 说明：

- 参数: 第一次初始化指定的值在内部作缓存

- 返回值：包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数

setXxx()的两种写法：

- setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值

- setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值

### 深入State Hook

引入：当一个function component中使用useState定义了多个state，useEffect是怎么知道哪个state对应哪个state变量？ 首先看一个`useState`错误示范

```JavaScript
function App(){
  const [a, setA] = useState(0)
  if(...){const [b, setB] = useState(0)}
}
```

这时候会报错

![https://static.xuyanshe.club/img/1649777228015-c29f6c03-a691-43f3-bd4b-ab669b846da2.png](Hooks\1649777228015-c29f6c03-a691-43f3-bd4b-ab669b846da2.png)

这是因为`useState`是按照 `state`在内部的数组中的顺序来做数据的存储的，一旦将`useState`

放到了条件语句（上面为if中）就会影响到`useState`

更新数据时react调用的顺序（更新数据就会出现错误） 对于 `useState`的这个问题，官方文档中给的描述如下

> Don’t call Hooks inside loops, conditions, or nested functions. Instead, always use Hooks at the top level of your React function, before any early returns. By following this rule, you ensure that Hooks are called in the same order each time a component renders. That’s what allows React to correctly preserve the state of Hooks between multiple useState and useEffect calls. (If you’re curious, we’ll explain this in depth below.)

所以说，如果要使用`useState`必须放到组件的顶层位置 问题看完了，再来分析一下`useState`内部大致是一个什么样子的，下面为自定义的`useState`

```JavaScript
let x = [];
let index = 0;
const myUseState = initial => {
    let currentIndex = index;
    x[currentIndex] = x[currentIndex] === undefined ? initial : x[currentIndex];
    const setInitial = value=>{
      x[currentIndex] = value;
      render();
    }
    index += 1;
    return [x[currentIndex],setInitial]
}
//模拟的render函数
const render = () => {
  index = 0;  //将 index 重置
  ReactDOM.render(<App/>,document.querySelector('#root'))
}
const App = () => {
  const [n, setN] = myUseState(0)
  const [m, setM] = myUseState(0);
  return (
     <div>
         <p>n：{n}</p>
         <button onClick={()=>setN(n+1)}>+1</button>
         <p>m：{m}</p>
         <button onClick={()=>setM(m+1)}>+1</button>
     </div>
  )
}
```

可以看到要有一个外部的`index`，作为每个函数式组件的存放`state`的数组的索引。 下方line19和20，使用`myUseState`存state进数组时，他们的索引分别为0、1，对应着他们调用myUseState的顺序（line19第一个调用，line20第二个调用） **当line19中的n改变了会发生什么？** myUseState会将n更新的值拿到放到数组的3号位置，m会放到4号位置。 每一次都会提供一个新值放到useState中的内部数组中去，而不是替换原来位置的值。

### Effect Hooks

副作用钩子，可以在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子

React中的副作用操作：

- 可以在钩子中进行异步操作

- 订阅，启动定时器

- 手动更改真实DOM

写法：

```JavaScript
useEffect(() => { 
   // 在此可以执行任何带副作用操作
   return () => { // 在组件卸载前执行 componentWillUnmount() 
     // 在此做一些收尾工作, 比如清除定时器/取消订阅等
   }
 }, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后和卸载执行 
// 如果是为[] react会报警告，不能填写空依赖的useEffect
```

说明： 可以把 useEffect Hook 看做如下三个函数的组合

`componentDidMount()`

`componentDidUpdate()`

`componentWillUnmount()`

官方文档

### Ref Hooks

可以在函数组件中存储/查找组件内的标签或任意其它数据

**写法**： `const refContainer = useRef()`

**作用**： 保存标签对象,功能与 `React.createRef()`一样。根据文档，此类型的数据主要是为了那些不影响页面的数据的存储，比如定时器timer，timer的改变不会让组件重新渲染页面

PS. 不能在组件重新渲染的时候去读取和写入`ref`对象，为了保持纯组件(`pure component`)

#### 获取组件的`ref`

需要将子组件通过`forwardRef()`包装起来

```React
import { forwardRef } from 'react';

const MyInput = forwardRef(({ value, onChange }, ref) => {
  return (
    <input
      value={value}
      onChange={onChange}
      ref={ref}
    />
  );
});

export default MyInput;
```

此API可以将子组件的`ref`暴露出来

**参数**：`render`函数，渲染函数，返回jsx的一个函数，并且会给这个函数传递`prop`和`ref`作为渲染函数的参数

也可以透传`ref`但子级的这些组件都需要接收`ref`，都需要用`forwardRef`包装

PS. `ref`属性和`key`是两个特殊的属性，如果要透传，要用到对应的API，否则就是在绑定了这两个属性的元素上作用了



### `useReducer`

`reducer`的`Hook`API，`reducer`的逻辑查看`redux`那里，概念都是差不多的



