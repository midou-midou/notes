# React基础

## 基础

### 一、JSX

```JavaScript
const element = <h1>Hello, {name}</h1>
```

JavaScript和UI代码的结合 用`{}`包裹表达式（js表达式，变量）

```JavaScript
const element = <img className={imgClass} src={user.avatarUrl} />
```

- class属性写为className

- 属性用小驼峰语法

```JavaScript
const element = <h1>Hello, world!</h1>
ReactDOM.render(element,document.getElementById('root'))
```

一个可运行的React Hello World例子 在React中，碰到`标签`会以html解析，碰到`{}`会以JavaScript解析 渲染一个React组件使用`ReactDOM.render(element, container[, callback])`

### 二、组件

1. 类式组件

2. 函数式组件

```JavaScript
function MyComponent(){
  return <h2>我是用函数定义的组件(适用于【简单组件】的定义)</h2>
}
export defualt class MyComponent extends React.Component {
  render(){
    return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>;
  }
}
```

插入组件使用import，使用组件用 开头大写 的标签，组件可嵌套

```JavaScript
import Hello from './components/Hello'
var MyComponent = function(){
  return <Hello />
}
```

渲染组件，ReactDOM的使用

```JavaScript
ReactDOM.render(
  MyComponent,
  document.getElementById('root')
)
```

### 三、组件上的props和state

组件接收的入参为props，组件自身的状态为state

### props

组件身上的标签属性（也就是这个组件接收的参数）都会存放到组件的props属性上

```JavaScript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

传值

```JavaScript
const el = <Welcome name="嘉然" />
```

传入值的类型检查，PropTypes的使用

```JavaScript
static propTypes = {
  name:PropTypes.string.isRequired, //限制name必传，且为字符串
  sex:PropTypes.string,//限制sex为字符串
  age:PropTypes.number,//限制age为数值
}
```

默认值，defaulyProps的使用

```JavaScript
static defaultProps = {
	sex: "male",
	age: 20
}
```

props不可修改

### State

函数式组件需借助Hooks使用state，类式组件可以直接使用state。 state是一个对象的形式

```JavaScript
class Weather extends React.Component{
  constructor(props){
    super(props)
    //初始化状态
    this.state = {isHot:false,wind:'微风'}
}
```

不可以直接修改state，使用setState()修改 `this.setState({isHot:!isHot})`

使用`setState(stateChange||updater, [callback])`需要注意：

- 不要在一些生命周期函数中使用，会引起死循环

- 更新是异步的，如果要实现同步更新，setState要接收一个函数

- callback会在组件状态和界面都更新后才会被调用

- 如果新状态不依赖于原状态 ===> 使用对象方式

- 如果新状态依赖于原状态 ===> 使用函数方式

- 如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取

### 四、事件处理

1. 事件处理回调

2. `Ref`

类式组件里，在回调函数中要注意this指向的问题

```JavaScript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

上面的写法还有其他的形式，可以不用`bind`来绑定，bind涉及到作用域，参考javascript部分

```JavaScript
class LoggingButton extends React.Component {
  handleClick = () => {
    console.log('this is:', this);
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

上面到写法就是不需要使用bind绑定到写法

函数式组件的事件绑定如下

```JavaScript
function ActionLink() {
  handleClick = (e) => {
    console.log('The link was clicked.');
  }
  return (
    <button href="#" onClick={handleClick}>Click me</button>
  );
}
```

**Ref** 访问DOM的方法，即为一个DOM的引用 创建一个Ref

- `React.createRef()`创建的是ref对象

- 回调形式的Ref

- String类型的Ref（已经过时，不再使用）

从上到下依次是三种使用Ref的例子

```JavaScript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

1. 当在html元素上使用ref，ref对象接收的是底层DOM元素，放到其current属性上

2. 当ref用于class组件上时，ref对象接收的是这个组件的实例，放到其current属性上

3. 函数没有实例，不能将其放到ref上

回调形式的ref

```JavaScript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = null;
    this.setTextInputRef = element => {
      this.textInput = element;
    };
    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if (this.textInput) this.textInput.focus();
    };
  }
  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }
  render() {
    // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

1. 在组件挂载的时候，会执行ref的回调函数

2. 在组件卸载的时候，也会调用ref的回调函数

### 五、虚拟DOM、真实DOM

### 虚拟DOM

开销极小的普通对象

```JavaScript
const Velement = <div>Hello, world</div>;
```

### 真实DOM

```JavaScript
const Relement = document.getElementById("root");
```

### 两者关系

- 调用ReactDOM.render将虚拟DOM渲染成真实DOM，组件state改变时，React会创建新VDOM并生成RDOM

- 虚拟DOM没有真实DOM过多的属性

### 六、Diff算法

对象差异比对调和（Reconciliation）算法

- 虚拟DOM的比对

- 三种比对策略

Tree Diff：DOM树的同层比对，跨层时，直接创建新节点及节点下的树 Element Diff：节点在同一层级，会使用 删除、插入、移动 来更新插入

![https://static.xuyanshe.club/img/1648260914571-62e0ab6e-8899-437b-8984-0421b57ea84d.png](https://static.xuyanshe.club/img/1648260914571-62e0ab6e-8899-437b-8984-0421b57ea84d.png)

Component Diff：

- 不同类型组件：使用Tree Diff

- 同类型组件，比对 属性 - 子节点 子节点如根节点比对形式 属性 - 子节点 比对。碰到不同类型节点便使用Tree Diff规则。也可以使用 生命周期函数`shouldComponentUpdate()`来手动控制组件更新不同类型组件

不同类型组件

```HTML
<div>
  <Counter />
</div>
<span>
  <Counter />
</span>
```

同类型组件

```HTML
<div className="before" title="stuff" />
<div className="after" title="stuff" />
```

节点在同一层级，会使用 **删除、插入、移动** 来更新 插入

```HTML
<ul>
  <li>first</li>
</ul>
<ul>
  <li>first</li>
  <li>second</li>
</ul>
```

每个节点带有key，避免 一些破坏渲染顺序的操作引发的性能问题

```HTML
<ul>
  <li>first</li>
</ul>
<ul>
  {/* React不会做移动操作更新ul下面的两个li，而是直接删除原来ul的li，生成新的两个li */}
  <li>second</li>
  <li>first</li>
</ul>
```

根据key判断原节点是否在原VDOM中存在

### 七、生命周期函数

生命周期钩子，特定时刻执行

- React16.4 旧生命周期

![https://static.xuyanshe.club/img/1648260914566-29493971-1264-4585-8b29-a8892b6fea6e.png](https://static.xuyanshe.club/img/1648260914566-29493971-1264-4585-8b29-a8892b6fea6e.png)

初始化阶段: 由ReactDOM.render()触发—初次渲染

1. `constructor()`

2. `componentWillMount()`

3. `render()`

4. `componentDidMount()`

更新阶段: 由组件内部this.setSate()或父组件render触发

1. `shouldComponentUpdate()`

2. `componentWillUpdate()`

3. `render()`

4. `componentDidUpdate()`

卸载组件: 由ReactDOM.unmountComponentAtNode()触发 `componentWillUnmount()`

- 新生命周期

![https://static.xuyanshe.club/img/1648260914567-71926746-14eb-4a2b-abab-b9be2e79546d.png](https://static.xuyanshe.club/img/1648260914567-71926746-14eb-4a2b-abab-b9be2e79546d.png)

image.png

1. 初始化阶段: 由ReactDOM.render()触发—初次渲染

    2. `constructor()`

    3. `getDerivedStateFromProps()`

    4. `render()`

    5. `componentDidMount()`

6. 更新阶段: 由组件内部this.setSate()或父组件重新render触发

    7. `getDerivedStateFromProps()`

    8. `shouldComponentUpdate()`

    9. `render()`

    10. `getSnapshotBeforeUpdate()`

    11. `componentDidUpdate()`

12. 卸载组件: 由`ReactDOM.unmountComponentAtNode()`触发`componentWillUnmount()`

PS.手动控制组件更新

```JavaScript
shouldComponentUpdate(){    
	//do something    
	reture true // 或者填写false
}
```

### 八、官方脚手架

[Create a New React App – React (reactjs.org)](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app) 安装 `npm install -s create-react-app` 使用 `npx create-react-app myapp`

### 九、网络请求

- axios

- fetch

### axios

[axios中文网](http://www.axios-js.com/) 简单使用实例

```JavaScript
axios.get(`/api1/users?q=${keyWord}`).then(
  response => {
    // 成功请求后返回的响应
    error => {
      //请求失败后的处理
    }
)
```

### fetch

[MDN:WorkerOrGlobalScope.fetch()](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch) 实例

```JavaScript
fetch.get(`/api1/users?q=${keyWord}`)
  .then( resp => {} err => {})
  .then( resp(data) => {} err => {})
  async await语法
  async() => {
      const response= await fetch(`/api1/search/users2?q=${keyWord}`)
      const data = await response.json()
}
```

