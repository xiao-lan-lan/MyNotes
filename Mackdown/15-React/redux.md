# Redux

### Part1

#### 01-基础-[redux](https://www.redux.org.cn/)-介绍

[redux官网](https://redux.js.org/introduction/getting-started)

[redux中文网](https://www.redux.org.cn/) 

> 回忆vue->vuex

1. vuex是专门用于管理复杂vue项目的共享数据
2. vuex是vue的插件
3. 全局`单例`->全局的对象store
4. vuex的管理流程->**图示**
5. vuex插件->基于Flux架构模式->vuex是Flux这种思想在vue中的体现

> react->组件通信

1. props
2. props.children
3. 子传父 
4. 子组件B->**提升**到父组件->子组件A

> redux

1. **redux是一个用于js应用的状态管理容器**
   1. redux和react没有任何直接关系
   2. redux可以在jq|vue等项目中使用,也可以在react项目中使用
   3. redux基于Flux架构模式的具体产物
   4. redux可以用来做统一的数据管理
2. react是用于构建用户页面的js库->从MVC的角度思考->react->View层
3. redux,从MVC的角度思考->Model层->数据层

#### 02-基础-redux-action

1. 是 store 数据的唯一来源
2. action的使用位置:通过 store.dispatch() 将 action 传到 store -> 建立store和action的联系
3. Action 本质上是 JavaScript 普通对象
4. action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作
5. 除了 type 字段外，action 对象的结构完全由你自己决定
6. action 创建函数是返回一个 action的方法
7. 把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程。
8. 将来action创建函数的使用位置是  dispatch(acfn2())  ->  待测试->讲完store之后!!!!

```js
 const acfn1 = (num) => {
            return {
                type: 'add',
                num
            }
        }
        const acfn2 = (num) => {
            return {
                type: 'sub',
                num
            }
        }

```

`注意` : 将来action创建函数的使用位置是  dispatch(acfn2())  ->  待测试->讲完store之后!!!!



> 1. npm 安包-> 报错->安装失败-> 产生错误缓存->npmcache文件夹
> 2. npm i xxoo -> 直接找错误缓存 -> 导致安装失败
> 3. 清除缓存
>    1. C盘->路径删除(干净)
>    2. npm cache clean --force(大多数情况好用)

#### 03-基础-redux-reducer

```js
// 1. actions只是提供了方法的列表(相当于书籍的目录)
// 2. state 都被保存在一个单一对象中
// 3. reducer 就是一个纯函数
// 4. 接收旧的 state 和 action，返回新的 state
// 5. reducer建议(不这样写也不会报错,但是会产生副作用)
// 5.1 不要直接修改形参!!!!!!!!!!!
// 5.2 不要写API 请求和路由跳转；
// 5.3 不要调用非纯函数，如 Date.now() 或 Math.random()。
// 6. reducer函数里面根据不同的action类型对state进行不同的处理,并且返回

```

```js
 function reducerfn(state = 10, action) {

      switch (action.type) {
        case 'add':
          let state1 = state
          state1++
          return state1

        case 'sub':
          let state2 = state
          state2--
          return state2

        default:
          return state
      }
    }
```



#### 04-基础-redux-store-dispatch

> store->仓库

1. 提供 getState() 方法获取 state；
2.  提供 dispatch(action) 方法更新 state；
3. 通过 subscribe(listener) 注册监听器;
4. 通过 subscribe(listener) 返回的函数注销监听器。
5. Redux 应用只有一个单一的 store
6. 利用reducer创建store

```js
 const {
      createStore
    } = Redux
    let store = createStore(reducerfn)
    // let store = createStore(reducerfn, {
    //   num: 100
    // })

    console.log(store.getState()); // 10

    // state改变时,触发下面的cb
    const unsubscribe = store.subscribe(() =>
      console.log(store.getState())
    )

    // 发起action->修改state
    // store.dispatch(行为action)
    store.dispatch(createAction1())
    // store.dispatch(createAction1())
    // store.dispatch(createAction1())
    // store.dispatch(createAction1())
    // store.dispatch(createAction1())
    store.dispatch(createAction2())
```

`注意`: 此时redux和react没任何关系

#### 05-基础-redux-结合react使用redux

1. 之前的store/action/reducer的代码和react没关
2. 在react项目中如何使用redux

> 步骤

1. 脚手架搭建react项目create-react-app demos
2. 简化模板
3. 启动项目
4. npm i redux
5. index.js -> import {createStore} from 'redux'

> 回顾

1. action-> 提供了很多的描述->描述了有事儿发生
2. reducer->函数->接收旧state和action,返回新state
3. store->连接了action和reducer,同时提供的state的初始值

`index.js`

```js
import { createStore } from 'redux'

function reducerfn(state = 10, action) {
  switch (action.type) {
    case 'add':
      let state1 = state
      state1++
      return state1

    case 'sub':
      let state2 = state
      state2--
      return state2

    default:
      return state
  }
}

let store = createStore(reducerfn)

ReactDOM.render(<App store={store} />, document.getElementById('root'))

```

`App.js`

```js
render() {
    const { store } = this.props
    // console.log(store)
    return (
      <div>
        App
        <p>{store.getState()}</p>
        <button onClick={() => {}}>点我+1</button>
      </div>
    )
  }
```

`App.js`

```js
  <button
          onClick={() => {
            store.dispatch(createAction1())
          }}
        >
          点我+1
        </button>
        <button
          onClick={() => {
            store.dispatch(createAction2())
          }}
        >
          点我-1
        </button>
```

`index.js`

```js

// 每次dispatch后 就会执行下面的cb -> 获取最新state
store.subscribe(() => {
  console.log(store.getState())
  ReactDOM.render(<App store={store} />, document.getElementById('root'))
})

ReactDOM.render(<App store={store} />, document.getElementById('root'))
```

`总结`

1. 入口文件index.js -> 
   1. 配置store和reducer
   2. 把store传递给App组件
2. App.js中
   1. 接收props中的store
   2. 导入actions.js
   3. 调用store.dispatch(action)
   4. 使用store.getState()

`注意`

1. react->构建用户页面的js库 -> 写视图的-> 处理View视图
2. redux->专门处理共享数据的->处理state的->处理Model数据模型



#### 06-基础-redux-结合react-异步action-中间件

> 同步action->完成
>
> 异步action->写法完全不同

`actions.js`

```js
export const createAction3 = num => {
  return dispatch => {
    // dispatch(createAction1())

    setTimeout(() => {
      dispatch(createAction1())
    }, 1000)
  }
}

```

> 上述代码没问题->但是需要中间件帮你处理代码

1. npm i redux-thunk
2. index.js

```js
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'

let store = createStore(reducerfn, applyMiddleware(thunk))
```



1. react->处理视图View
2. redux->处理数据Model
3. 直接在react中使用redux 很麻烦->所以=>
4. 使用react插件react-redux来帮我们简化react中使用redux的操作!



#### 07-案例-[react-redux](https://react-redux.js.org/)-介绍-安装-案例准备

> 刚才的课程重点

1. redux和react没关系
2. redux:action+reducer+store

> redux在react中使用很麻烦->可以**使用react-redux**可以很大的简化在react使用redux

1. 搭建项目
2. 简化模板
3. npm i react-redux redux
4. 把02-其他资源的src中的文件和模板中的src文件替换
   1. App
      1. Cart->购物车
         1. Item->商品组件
         2. Total->计算总数的组件

> 注意: 之前的所有讲解的代码都不需要掌握!->接下来才是需要掌握的

#### 08-案例-react-redux-配置Provider-store-reducer

> 文件结构

1. src
   1. store
      1. index.js->返回store
      2. reducer.js->返回reducer函数

`store/index.js`

```js
// 返回仓库

// action|store|reducer写法和之前完全一样->和redux的写法一样
// react-redux->简化使用方式

import { createStore } from 'redux'

import Reducer from './reducer.js'
// console.log(Reducer)

// createStore(reducer,可选参数2)
// 可选参数2就是项目中的所使用的数据state
let state = {
  num1: 10,
  num2: 20,
  num3: 30
}

let store = createStore(Reducer, state)
export default store

```

`store/reducer.js`

```js
// 返回reducer

const Reducer = (state, action) => {
  console.log(state) // {num1:10,num2:20,num3:30}->接收改之前的state

  switch (action.type) {
    case 'add':
      return state

    case 'sub':
      return state

    default:
      return state
  }
}

export default Reducer

```

`项目的入口文件index.js`

```js
import React from 'react'
import ReactDOM from 'react-dom'

import { Provider } from 'react-redux'
// 其实react-redux就是简化了redux的使用方式
// Provider简化了store的用法,包裹在App组件的外层

import store from './store/index.js'

import App from './App'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)

```



#### 09-案例-react-redux-配置connect 显示总数

`显示Total.js的总数`

```js
import { connect } from 'react-redux'


class Total extends React.Component {	
  render() {
    const { sum } = this.props // {}

    return <div>商品总数：【{sum}】</div>
  }
}

const mapStateToProps = (state, ownProps) => {

      let sum = eval(Object.values(state).join('+'))

    return {
        sum:sum
    }
    
}

export default connect(mapStateToProps)(Total)

```

> mapStateToProps()方法说明

1. 形参1: store的state
2. 形参2: 当前组件的props=>{}
3. return {要添加的新属性:值}

> connect()方法说明

```js
// 1. connect是方法,实参1要mapStateToProps
// connect(mapStateToProps)
// 2. connect() return一个新方法
// connect(mapStateToProps)()
// 3. connect()()是一个高阶组件
// let Total1 = connect(mapStateToProps)(Total)
// export  default Total1
```

> 补充

1. Object.values()->提取对象的所有value放在数组中
2. [].join->把数组转为string
3. eval()->可以识别字符串中的变量和运算符号  (这是一个js的神奇的方法!)

#### 10-案例-react-redux-配置connect-显示名称和数量

`Item.js`

```js
import { connect } from 'react-redux'

const mapStateToProps = (state, ownProps) => {
  // console.log(ownProps) // {pname:num123}
  // console.log(state) // {num1:10,num2:20,num3:30}
  const num = state[ownProps.pname]

  return {
    num,
    pname: ownProps.pname
  }
}

export default connect(mapStateToProps)(Item)

```



#### 11-案例-react-redux-修改数量-connect-dispatch-action

`actions.js`

```js
export const createAction1 = pname => {
  return {
    type: 'add',
    pname: pname
  }
}

export const createAction2 = pname => {
  return {
    type: 'sub',
    pname: pname
  }
}

```

`reducer.js`

```js
// 返回reducer

const Reducer = (state, action) => {
  switch (action.type) {
    case 'add':
      let state1 = Object.assign({}, state)
      state1[action.pname]++

      return state1

    case 'sub':
      let state2 = Object.assign({}, state)
      state2[action.pname]--
      return state2

    default:
      return state
  }
}

export default Reducer

```

`Item.js`

```js
import React from 'react'

// 把store的数据映射到当前组件中
import { connect } from 'react-redux'

import { createAction1, createAction2 } from '../store/actions.js'

class Item extends React.Component {
  render() {
    const { num, pname, add, sub } = this.props
    // console.log(add)

    return (
      <div>
        <button
          onClick={() => {
            // 修改数据->store->state->dispatch(action)
            add()
          }}
        >
          +
        </button>
        <button
          onClick={() => {
            sub()
          }}
        >
          -
        </button>
        <span>
          【{pname}】商品的数量【
          {num}】
        </span>
      </div>
    )
  }
}

// 映射state到props->把store的state改为当前组件的props属性

const mapStateToProps = (state, ownProps) => {
  // console.log(ownProps) // {pname:num123}
  // console.log(state) // {num1:10,num2:20,num3:30}
  const num = state[ownProps.pname]

  return {
    num,
    pname: ownProps.pname
  }
}

// 把dispath映射为组件的props
const mapDispatchToProps = (dispatch, ownProps) => {
  // console.log(dispatch, ownProps)
  // console.log(ownProps)

  return {
    add: () => {
      dispatch(createAction1(ownProps.pname))
    },
    sub: () => {
      dispatch(createAction2(ownProps.pname))
    }
  }
}
// connect
// 1. 是方法
// 2. 形参1是某种结构的函数
// 3. connect()返回的是HOC函数
// 4. 新组件 =connect()(旧组件)

// let ItemNew = connect(mapStateToProps)(Item)

// export default ItemNew

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Item)

```

#### 12-案例-总结

重要的总结` !!!!

1. react->UI->视图View
2. redux->数据store|action|reducer->数据模型Modal
3. react-redux->连接react视图和redux数据的桥梁-> Provider&connect

```js
import { Provider } from 'react-redux'
import { connect } from 'react-redux'

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Item)
```



---



#### 13-todos案例-演示-准备

`技术栈`

1. react->视图->组件(JSX)
2. redux->状态管理->reducer + action + store
3. react-redux->使用redux->Provider&connect

`业务`

1. 列表展示
2. 添加任务
3. 删除任务
4. 切换任务状态

`准备`

1. 搭建react项目
2. 安装redux和react-redux
3. 简化模板
4. 开始编写redux代码
   1. actionType/index.js
   2. actions/index.js
   3. reducer/index.js



#### 14-todos案例--创建actionTypes

```js
// actionTypes

/**
 * 1. 添加
 * 2. 删除
 * 3. 切换任务状态
 */

const ADD_TODO = 'ADD_TODO'
const DELETE_TODO = 'DELETE_TODO'
const TOGGLE_TODO = 'TOGGLE_TODO'

export { ADD_TODO, DELETE_TODO, TOGGLE_TODO }

```



#### 15-todos案例--创建action

```js
// action

import { ADD_TODO, DELETE_TODO, TOGGLE_TODO } from '../actionTypes'

// addTodo
// deleteTodo
// toggleTodo

const addTodo = text => ({ type: ADD_TODO, text })
const deleteTodo = id => ({ type: DELETE_TODO, id })
const toggleTodo = id => ({ type: TOGGLE_TODO, id })

export { addTodo, deleteTodo, toggleTodo }

```



#### 16-todos案例--创建reducer-添加任务

```js
// reducer

import * as actionTypes from '../actionTypes'
const reducer = (state = [], action) => {
  switch (action.type) {
    case actionTypes.ADD_TODO:
      const id = state.length === 0 ? 1 : state[state.length - 1].id + 1
      return [...state, { id, text: action.text, done: false }]
    default:
      return state
  }
}

export default reducer

```

> 此时可以测试效果

#### 17-todos案例--创建reducer-删除任务和切换状态

```js
case actionTypes.DELETE_TODO:
      return state.filter(item => item.id !== action.id)

    case actionTypes.TOGGLE_TODO:
      return state.map(item => {
        const tempItem = { ...item }
        if (tempItem.id === action.id) {
          tempItem.done = !tempItem.done
        }
        return tempItem
      })
```

`测试` -> index.js

```js
import { createStore } from 'redux'
import reducer from './reducer'
import { addTodo, deleteTodo, toggleTodo } from './actions'

const store = createStore(reducer)
store.subscribe(() => {
  console.log(store.getState())
})
store.dispatch(addTodo('吃饭'))
store.dispatch(addTodo('睡觉'))
store.dispatch(deleteTodo(1))
store.dispatch(toggleTodo(2))

```

> 如何确定所用方法

1. 明确需求
2. 常用API的使用(作用/形参/返回值)记住

### Part2

#### 01-todos案例-组件结构和样式

```html
 <div>
        {/* 添加任务 */}
        <input type="text" />
        <button>ADD</button>
        {/* 任务列表 */}
        <ul>
          <li>
            <span style={{ textDecoration: 'line-through' }}>吃饭</span>
            <button>X</button>
          </li>
          <li>
            <span>睡觉</span>
            <button>X</button>
          </li>
        </ul>
      </div>
```

```css
li {
    line-height: 30px;
}

span {
    display: inline-block;
    width: 150px;
}
```



#### 02-todos案例-组件结构调整

1. App
   1. components/AddToDo
   2. components/ToDoList
2. index.js
3. index.css



#### 03-todos案例-配置store和Provider

`index.js`

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import configStore from './configStore'
import { Provider } from 'react-redux'
import * as serviceWorker from './serviceWorker'

const store = configStore()
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)

serviceWorker.unregister()
```

`configStore`

```js
import { createStore } from 'redux'

import reducer from './reducer'

const configStore = () => {
  const store = createStore(reducer)
  store.subscribe(() => {
    console.log(store.getState())
  })
  return store
}

export default configStore

```



#### 04-todos案例-列表展示

```js
import React from 'react'
import { connect } from 'react-redux'
const ToDoList = props => {
  const { list } = props
  if (list.length === 0) {
    return <p>请添加任务</p>
  }

  return (
    <>
      <ul>
        {list.map(item => (
          <li key={item.id}>
            <span
              style={{ textDecoration: item.done ? 'line-through' : 'none' }}
            >
              {item.text}
            </span>
            <button>X</button>
          </li>
        ))}
       
      </ul>
    </>
  )
}

const mapStateToProps = (state, ownProps) => {
  return {
    list: state
  }
}
export default connect(mapStateToProps)(ToDoList)
// export default ToDoList

```



#### 05-todos案例-添加任务

> 添加任务、事件绑定、清空和非空校验

```js
import React, { createRef } from 'react'
import { connect } from 'react-redux'
import { addTodo } from '../actions'
const AddToDo = props => {
  const textRef = createRef()
  const handleAdd = () => {
    // console.log(textRef.current.value, props)
    if (textRef.current.value.trim() === '') return
    props.dispatch(addTodo(textRef.current.value))
    textRef.current.value = ''
  }
  return (
    <>
      <input type="text" ref={textRef} />
      <button onClick={handleAdd}>ADD</button>
    </>
  )
}

const mapStateToProps = state => {
  return {}
}
export default connect(mapStateToProps)(AddToDo)
```



#### 06-todos案例-切换任务

```js
 const handleToggle = ID => {
    props.dispatch(toggleTodo(ID))
  }
```



#### 07-todos案例-删除任务

```js
 const handleDele = ID => {
    props.dispatch(deleteTodo(ID))
  }
```



#### 08-todos案例-展示组件和容器组件-说明

[文档](https://www.redux.org.cn/docs/basics/UsageWithReact.html) 

1. 展示组件:ToDoList,不要和redux进行关联
2. 容器组件:数据获取、状态更新,和redux直接关联,没有JSX结构



`具体含义`

- 展示组件：普通的 react 组件，与 redux 不直接关联
  - 展示组件，只负责 组件结构和样式（ React 的内容 ）
  - 分离为独立的展示组件后，那么，展示组件就不会与 redux 密切关联在一起，或者说只能配合redux使用。因为分离了，所以，展示组件与 redux 就没有任何关联了，**所以，即便是将 redux 替换为其他的状态管理工具，但是，展示组件 是不需要做任何改动的！！！**
- 容器组件：与 redux 关联的组件（使用 connect 高阶组件包装的组件）
  - 容器组件，负责与 redux 交互，负责获取 state，负责 dispatch 分发 action 来修改状态
  - 容器组件没有 JSX 结构

#### 09-todos案例-展示组件和容器组件-拆分

`容器组件` -> 管理redux

```js
// 对应的是ToDoList.js中的容器组件代码
import { connect } from 'react-redux'

import ToDoList from '../components/ToDoList'
import { toggletodo, deletetodo } from '../actions'

const mapStateToProps = state => {
  return {
    list: state
  }
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    toggle: ID => {
      dispatch(toggletodo(ID))
    },
    dele: ID => {
      dispatch(deletetodo(ID))
    }
  }
}

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(ToDoList)

```

`展示组件`-> JSX

```js
const handleToggle = ID => {
    // props.dispatch(toggleTodo(ID))
    props.toggle(ID)
  }

  const handleDele = ID => {
    // props.dispatch(deleteTodo(ID))
    props.dele(ID)
  }
```

> 目的: 让视图层V和数据层M完全分离 耦合性低->
>
> 如果使用其他的状态管理工具,比如mobx,这里的展示组件(React的JSX代码)完全不用修改



#### 10-todos案例-不同状态任务-功能分析

- 1 all         展示所有任务
- 2 active      展示未完成的任务
- 3 completed   展示已完成的任务

在这三种任务状态之间来回切换，所以，每一次切换显示不同状态的任务时，没有直接
修改 redux 中状态（ 任务列表数组 ）。

而仅仅是根据：状态（任务列表数组）+ 要展示任务的状态 ==> 新的要展示的任务列表数据

```js
// 第一个参数：表示 redux 中所有的任务状态
// 第二个参数：表示 要展示的任务状态，可选值为：all/active/completed
const getTodosByType = (todos, filter) => {
  if (filter === 'all') {
    return todos
  } else if (filter === 'active') {
    return todos.filter(item => !item.done)
  } else if (filter === 'completed') {
    return todos.filter(item => item.done)
  }
}

// 调用：
getTodosByType([], 'all')           // 所有数据： []
getTodosByType([], 'active')        // 所有未完成数据数据： []
getTodosByType([], 'completed')     // 所有已完成数据数据： []
```



#### 11-todos案例-Footer展示组件

目的: 创建Footer组件,里面添加三个按钮

文件: components/Footer.js

```js
import React from 'react'

export default props => {
  return (
    <>
      <p>
        <button>All</button>
        <button>Active</button>
        <button>Competed</button>
      </p>
    </>
  )
}

```

#### 12-todos案例-创建三个状态的action

`actionType.js`

```js
const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

```

`action`

```js
// 1. all
// 2. active
// 3. completed
const setVisibilityFilter = filter => ({type: ActionType.SET_VISIBILITY_FILTER, filter})
```

#### 13-todos案例-combineReducers合并reducer

`目的`: setVisibilityFilter的目的不是修改state,所以需要另外增加一个reducer

```js
import { combineReducers } from 'redux'

import * as ActionType from '../actionType'

const todos = (state = [], action) => {
  switch (action.type) {
    case ActionType.ADD_TODO:
      let state1 = [...state]
      if (state.length === 0) {
        return [{ id: 1, text: action.text, done: false }]
      }
      state1.push({
        id: state1[state1.length - 1].id + 1,
        text: action.text,
        done: false
      })

      return state1
    case ActionType.DELETE_TODO:
      // 分析:这里要做的事儿->filter()
      // 1. 遍历state
      // 2. 筛选符合条件的元素
      // 3. 返回新数组
      return state.filter(item => action.id !== item.id)

    case ActionType.TOGGLE_TODO:
      // 已有数据:action.id=>11
      // 1. 遍历原始数组
      // 2. 返回新数组
      // 3. 条件+执行语句
      let temp = [...state]
      return temp.map(item => {
        if (action.id === item.id) item.done = !item.done
        return item
      })

    default:
      return state
  }
}
const visibilityFilter = (state = 'all', action) => {
  return state
}

const rootReducer = combineReducers({
  todos,
  visibilityFilter
})

export default rootReducer

```

> 此时 state的默认值发生变化, 为一个对象 {todos:[],?}

#### 14-todos案例-合并reducer执行流程

- 1 调用 dispatch 来分发一个动作
- 2 redux 内部将 action 传递给 reducer
  - 2.1 redux 将 action 传递给 rootReducer（根reducer）
  - 2.2 根reducer，分别调用每一个 reducer，来执行相同的状态处理
  - 注意：每个 reducer 中，获取到的仅仅是当前 reducer 处理的那一部分数据（比如：todos reducer 只处理 state.todos 数据，visibilityFilter reducer 只处理 state.visibilityFilter 数据）
- 注意：只要分发了任何一个 action，redux中所有的 reducer 全部都会被执行一次！！！
  - 能处理当前 action ，就返回新状态
  - 不能处理当前 action，就返回默认

#### 15-todos案例-Footer容器组件

```js
/**
 * 容器组件 Footer.Container.js
 */

import * as ACTIONS from '../actions'
import { connect } from 'react-redux'
import Footer from '../components/Footer.js'

const mapStateToProps = state => {
  return {
    filter: state.visibilityFilter
  }
}

const mapDispatchToProps = dispatch => {
  return {
    setFilter(filter) {
      dispatch(ACTIONS.setVisibilityFilter(filter))
    }
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(Footer)

```



#### 16-todos案例-展示不同状态任务



#### 17-todos案例-小结

- 如何在react中合理的使用redux?



#### 18-todos案例-完善切换状态

1. 代码位置: 列表变化<-ToDoList.js<-容器**ToDoList.Container.js**<-处理了todos数组<-getTodosByType
2. reducer(第二个)->state的默认值="all"

```js
// 对应的是ToDoList.js中的容器组件代码
import { connect } from 'react-redux'
import { toggletodo, deletetodo } from '../actions'
import ToDoList from '../components/ToDoList'

// 第一个参数：表示 redux 中所有的任务状态
// 第二个参数：表示 要展示的任务状态，可选值为：all/active/completed
const getTodosByType = (todos, filter) => {
  // console.log('getTodosByType：', todos, filter)
  if (filter === 'all') {
    return todos
  } else if (filter === 'active') {
    return todos.filter(item => !item.done)
  } else if (filter === 'completed') {
    return todos.filter(item => item.done)
  }
}

// 给组件提供状态
const mapStateToProps = state => {
  // console.log('TodoList state：', state.todos)
  // 因为使用 combineReducers 以后，redux中的状态变为一个对象了，所以，需要将 对象 中的
  // todos 数据，单独传递给组件，那么，组件中才能正确获取到 todos 任务列表数据
  return {
    list: getTodosByType(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    toggle: ID => dispatch(toggletodo(ID)),
    dele: ID => dispatch(deletetodo(ID))
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(ToDoList)

```



#### 19-todos案例-本地存储

##### 回顾

1. setItem
2. getITem
3. JSON.parse()
4. JSON.stringfy()

##### 问题

1. 应该保存什么数据啊?->  todos数组  +  visibilityFilter 字符串  ->  {todos:[],visibilityFilter:''}
   1. reducer1->state=[]
   2. reducer2->state="all"
   3. 合二为一的大state对象->
2. 在哪set保存啊?->在有最新数据的位置存->store/index.js->configStore 方法中
3. 在哪get取值啊?->

#### 20-todos案例-配合路由

##### 路由

1. npm i react-router-dom
2. Router -> App -> (Link+Switch>(Route+Redirect))

`路由的标准写法`

```js
// 路由标识使用结构
// Provider>Router->App
<Provider store={store}>
<Router>
    <Link/>

    <Switch>
        <Route/>
        <Route/>
        <Redirect/>
    </Switch>
</Router>

</Provider>
```

`提示`

1. 用redux和react-redux之前的路由写在在这里不变
2. 



#### 21-React-基础-小结

1. shouldComponentUpdate ->性能优化的->减少无意义的组件render

2. JSX高级语法-> 

   ```js
   <Modal.Left/>
    <Modal.Right/>
   ```

3. WebComponents-> 标准/规范-> js组件的写法

4. JSX语法(渲染数据|列表渲染|条件渲染)

5. 组件(无状态和有状态)-> state|钩子函数

6. 表单(受控|非受控)

7. 组件复用(高阶组件HOC | renderProps)

8. diff算法对比的过程(补丁)  key={最好不要使用index}

9. npx create-react-app demos

10. dev | eject | build

    

#### 22-React-路由-小结

> 开发SPA时,通过路由控制组件的显示/切换

1. 结构

2. 动态路由 /:id

3. 编程式导航

4. 重定向

5. withRouter-> 路由库提供的高阶组件->加工组件

   ```js
   
   // Login组件的props中就会增加路由相关信息(history/match等)
   export default withRouter(Login)
   
   ```

6. 注意: 

   1. react路由-> 两种模式
   2. vue-router->三种模式





