### React项目映入Redux注意事项

#### 1、引入文件

npm i --save redux@3.7.2 react-redux redux-thunk

npm i --save-dev redux-devtools-extension

#### 2、在React项目中创建Redux相关文件

![2019-12-03_205754](E:\笔记\react项目实战\2019-12-03_205754.png)

- store.js

  ```JavaScript
  /*redux最核心管理对象模块*/
  import {createStore,applyMiddleware} from 'redux'
  import thunk from "redux-thunk"; //中间件
  import {composeWithDevTools} from 'redux-devtools-extension'
  
  import reducers from "./reducers";
  
  
  
  //向外暴露store对象
  export default createStore(reducers,composeWithDevTools(applyMiddleware(thunk)))
  ```

- reducers.js

  ```JavaScript
  /*包含多个子reducer：根据老的state和指定的action返回一个新的state*/
  
  import {combineReducers} from 'redux'
  
  function f(state = 0, action) {
      return state;
  }
  function f1(state = 0, action) {
      return state;
  }
  export default combineReducers({
      f,
      f1,
  })
  //向外暴露的对象结构{f:0,f1:0}
  ```

- actions.js

  包含多个action creator 

  异步action：用于与发送请求有关的数据

  同步action

- action-types.js

  包含多个action名称

#### 3、入口文件引入Redux

```JavaScript
import React from 'react';
import ReactDOM from 'react-dom';
import {HashRouter,Route,Switch} from 'react-router-dom'
import {Provider} from 'react-redux'//react-redux

import store from "./redux/store";//store
import Login from "./containers/login/login";
import Register from "./containers/register/register";
import Main from "./containers/main/main";

ReactDOM.render(
    (
        <Provider store={store}>
            <HashRouter>
                <Switch>
                    <Route path='/login' component={Login}/>
                    <Route path='/register' component={Register}/>
                    <Route component={Main}/>  {/*默认组件*/}
                </Switch>
            </HashRouter>
        </Provider>

    ),
    document.getElementById('root'));
```

