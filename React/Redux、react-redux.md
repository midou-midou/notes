# Redux、react-redux

### redux

数据层的框架，解决react的数据传递问题。react的数据传递仅能在两个有关联的组件之间传递，如果第一层的组件要传递数据到第n层，就会很麻烦，所以需要一个数据层框架。 Redux = Reducer + Flux（最初的数据层框架）

图解

![https://static.xuyanshe.club/img/1648352194842-9fea66b6-ea14-423e-9174-2417cc1404be.png](Redux、react-redux\1648352194842-9fea66b6-ea14-423e-9174-2417cc1404be.png)



redux的工作流程：

![https://static.xuyanshe.club/img/1648352274817-13e6786a-023a-4d93-97f9-774cd5a20ea8.png](Redux、react-redux\1648352274817-13e6786a-023a-4d93-97f9-774cd5a20ea8.png)

根据上图的工作流程，可以得到

**redux的三个核心部分**

：

- action

- reducer

- store

用一个简单的例子来描述这三个核心部分：

- React Component：借书的人（使用redux的组件）

- Action Creators：借书的话（动作对象）

- Store：图书馆（将state，action，reducer联系到一起的方式）

- Reducers：记录书的本（用于初始化状态，加工状态的）

redux的工作流程：一个“借书的人” 说了一句“我要借书”的话，之后会在图书馆的“图书记录本”中查找书，找到了书就直接告诉借书的人

![https://static.xuyanshe.club/img/1648352974706-c6c76186-bda5-4435-84c6-b50a157e160c.png](Redux、react-redux\1648352974706-c6c76186-bda5-4435-84c6-b50a157e160c.png)

项目中的基本结构如上

- actionCreate：创建动作，动作要提供给组件和reducer使用，组件使用对应的action，会发送到reducer中加工之后返回一个新的state

- actionType：定义action的类型

- reducer：定义初始化store中的state，接收不同的action对state进行加工处理，之后返回一个新的state给store对象

```JavaScript
// actionCreate.js
import {
    PLAYINGVOICE
} from '../constant'

export const createPlayingAction = data => ({type: PLAYINGVOICE, data})
```

```JavaScript
// actionType.js
export const PLAYINGVOICE = 'playingvoice'
```

```JavaScript
// reducer.js
import {
    PLAYINGVOICE
} from '../constant'

var playingVoice = {voice: {path: '', desc: ''}}

export default function playerReducer(preState = playingVoice, action){
    const { data, type } = action
    if(type === PLAYINGVOICE){
        preState = Object.assign(preState, {voice: data.onevoice})
    }
    return newState;
}
```

```JavaScript
// store.js
import reducer from './Reducer.jsx'

const store = createStore(reducer);

export default store
```

```JavaScript
import React, { Component } from 'react'
import { deleteitem, } from './store/actionCreate'
import store from './store'

class ListItem extends Component{
    constructor(props){
        super(props);
        this.state={isFinished: false}
        this.deleteItem=this.deleteItem.bind(this);
    }
    componentDidMount(){
        store.subscribe(() => {
            if(this.state.isFinished){
                // do something
            }else{
                // do something
            }
        })
    }
    render(){
        return (
          <button id="deleteBtn" onClick={this.deleteItem}>删除</button>
        );
    }
    deleteItem(){
        const action = deleteitem(this.props.data.id);
        store.dispatch(action);
    }
export default ListItem;
```

上面演示了，在组件中redux的简单使用

### redux中的一些API

`combineReducers({})` 将多个Reducer拼接成一个大的reducer

### react-redux

- UI组件

- 容器组件

