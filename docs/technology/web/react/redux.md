# redux相关知识

首先介绍一下redux

redux也是一个状态管理库，对于我来讲，就是另外的一个vuex。核心由三部分组成：**Store**、**Action** 和 **Reducer**。

- Store：Store 是 Redux 的核心，用于存储整个应用的状态（State）
- Action：Action 是一个普通的 JavaScript 对象，用于描述发生了什么事件
- Reducer：Reducer 是一个纯函数，接收当前状态和 Action，返回新的状态

在使用时也是通过dispatch进行调用相应的action修改state的值。可以通过subscribe来监听state的变化，并通过useState来更新页面

一个简单的使用redux的过程：

```js
import {configureStore} from '@reduxjs/toolkit'

// 暂时先用两个state，一个是comList，用来表示当前画布区的组件列表。一个是nowCom，用来表示从左侧拖拽组件的type（就是从左侧组件列表拖拽的组件）
const initialState  = { comList: [], dragCom: '', selectCom:''}

// 通过dispatch调用action然后通过reducer生成最新状态的state

// comReducer会返回一个state，即使是进行某些操作，也是会返回更新后的state
const comReducer = (state: any = initialState, action: any) => {
    // eslint-disable-next-line no-debugger
    console.log(action.value,'action.value')
    // 根据行为来进行相应的操作，直接返回修改后的state的值就是对state进行修改
    switch (action.type) {
        case 'changeNowCom': {
            return {...state, dragCom: action.value}
        }
        case 'changeComList': {
            return {...state, comList: action.value}
        }
        // 增加selectCom用来表示选中的节点
        case 'changeSelectCom': {
            return {...state, selectCom: action.value}
        }
        default: {
            return state
        }
    }
}


// 创建 store
const store = configureStore({
    reducer: comReducer,
    middleware: (getDefaultMiddleware) => getDefaultMiddleware({
        serializableCheck: false // 关闭序列化检查
    })
});
export default store;

```

包括一些多状态管理等操作，就不一一详解了，写这篇文章主要是想记录下redux中的异步操作，刚看到的时候我有点疑惑，一个状态管理库为什么需要异步操作。

**主要还是使用redux中applyMiddleware** 

**使用 Redux Thunk：**

看了这段代码之后才发现，原本的redux中createStore时是传入一个对象，异步的情况就是createStore时可以传入一个函数，依据函数内部处理逻辑可执行多个不同的action

```js
// 安装 redux-thunk
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const store = createStore(reducer, applyMiddleware(thunk));

// 异步 action creator
function fetchUser() {
  return function (dispatch) {
    dispatch({ type: 'FETCH_USER_REQUEST' }); // 发起请求前的 action

    return fetch('https://api.example.com/user')
      .then(response => response.json())
      .then(data => {
        dispatch({ type: 'FETCH_USER_SUCCESS', payload: data }); // 成功后的 action
      })
      .catch(error => {
        dispatch({ type: 'FETCH_USER_FAILURE', payload: error }); // 失败后的 action
      });
  };
}

// 调用异步 action
store.dispatch(fetchUser());
```

其它的一些中间件，比如Redux Saga等，使用方法类似就不一一介绍了

