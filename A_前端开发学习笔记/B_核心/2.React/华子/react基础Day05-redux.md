# redux、react-redux
## 一、为什么需要使用redux  

两个组件(非父子关系)之间需要通信的话，可能需要多个中间组件为他们进行消息传递，这样既浪费了资源，代码也会比较复杂。

​	![](https://s2.loli.net/2022/02/26/aVUI9phwFCDQuPo.png)

## 二、redux数据流  

  ​	![](https://s2.loli.net/2022/02/26/mb734PfWJkjDAXn.jpg)

## 三、redux 基本使用

1. 安装redux依赖包 

  ```bash 

    yarn add redux -S  

  ```

2. 新建文件 src/store/index.js 

  ```js 

    import { createStore } from 'redux'
    import reducer from './reducer'

    export default createStore(
      reducer, 
      // 设置调试工具
      window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
    )

  ```

3. 新建文件 src/store/reducer.js  

  ```js 

    let defaultState = {
      count: 1
    }
    export default (state = defaultState, action) => {
      return state
    }
  ```

4. 在App.js中关联state和store的数据，并监听数据的变化同步数据

  ```js

    constructor() {
      super()
      // 通过store.getState 调用store的数据 
      this.state = store.getState()
      store.subscribe(this.handleStoreChange)
    }

    handleStoreChange = () => {
      this.setState(store.getState())
    }
  ```


## 四、react-redux 的基本使用   

1. 安装依赖包  

  ```bash 

    yarn add react-redux -S  

  ```

2. 在index.js 文件中进行修饰

 ```js  

  import { Provider } from 'react-redux'
  import store from './store'

  ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>
  ,
  document.getElementById('root'))

 ```

3. 在需要使用store的组件中通过高阶组件获取state 和 dispatch

  ```js 

    import { connect } from 'react-redux

    const mapStateToProps = (state) => {
      return {
        count: state.count
      }
    }

    const mapDispatchToProps = (dispatch) => {
      return {
        handleAddCount() {
          dispatch(addCount())
        },
        handleSubCount() {
          dispatch(subCount())
        }
      }
    }
    mapStates  

    mapMutations 

    mapGetters  

    mapActions  
    export default connect(mapStateToProps, mapDispatchToProps)(App)
  ```


## 五、redux异步请求 

1. 安装redux-thunk

```bash
  yarn add redux-thunk -S 

```

2. 修改主入口配置

```js

import { createStore, applyMiddleware, compose } from 'redux'

import reducer from './reducer'
import thunk from 'redux-thunk';

// export default createStore(
//   reducer,
//   applyMiddleware(thunk),
//   window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
// )

const composeEnhancers =
  typeof window === 'object' &&
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;

const enhancer = composeEnhancers(
  applyMiddleware(thunk),
);

const store = createStore(
  reducer,//构建初始的数据
  enhancer
)
export default store

```

3. 在actions.js中发送异步请求

```js
import { ADD_COUNT } from './types'
import axios from 'axios'
export const addCountAction = (value) => {
  return {
    type: ADD_COUNT,
    value
  }
}
   
export const getTodos = (number) => dispatch => {
  console.log(number)
  axios
    .get('http://jsonplaceholder.typicode.com/todos')
    .then(todos => {
      // 把actions 发送给 reducer   
      dispatch(addCountAction(todos))
    })
}
```

   