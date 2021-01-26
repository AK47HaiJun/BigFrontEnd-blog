## React

## 搭建React  脚手架

- ```
  npm install -g create-react-app
  ```

## 创建Rect 项目

- ```
  create-react-app demo01   //用脚手架创建React项目
  ```


## 父组件向子组件传值

```React
父组件

<div>
      <Son
          key={item+index}  
          content = {item}   传递属性
          index = {index}	 
          deleItem = {this.deleItem.bind(this)}   传递方法
      />
</div>
    
# 通过在子组件身上 绑定  变量名 = {要传递的值}  进行父向子通信


子组件

render(){
        return(
            <li onClick={this.onListen}>
                {this.props.content}   通过this.props.变量名来接收使用父组件传递过来的值和方法
            </li>
        )
    }

# 子组件通过 this.props.变量名 来接收使用

```

## 父组件向子组件传值， 校验传递值得属性

```react
#第一步 导入  import PropTypes from 'prop-types';

#第二部 在 class 外部定义

类名.propTypes = {
    //以下为父组件向子组件传递的属性，通过   :PropTypes。类型  来校验 
        content:PropTypes.string.isRequired,  //必须传递
        index:PropTypes.number, 
        deleItem:PropTypes.func          //传递为方法
}



指定默认传递值
// 传递默认值， 不要在 父组件的子组件上绑定变量名
Son.defaultProps = {
    name:'张三'
}

```



## React中 ref的使用

```react
//ref的作用： 用来获取元素的一些属性值

//使用方法：

eg： 获取表单赋值
<input type="text"
                 value={this.state.value} 
                 onChange={this.handleChange} 
                 ref = {input => {
                    this.input = input
 }}
 
handleChange(event) {
        this.setState({value: this.input.value});
 }


eg: 获取ul的 li 的个数

 <ul
    ref = {ul => {
       this.ul = ul
           }
>
</ul>
addList(){
        this.setState({
            menu:[...this.state.menu,this.state.value]
        },()=>{
            // 由于是虚拟Dom 是异步执行，在你添加内容后，原有长度不会改变
            // React中 diff 算法提供了回调方法， 虚拟DOM加载完成，会执行接下来的步骤
            console.log(this.ul.querySelectorAll('li').length)
        })    
    }
```

## Npm 一些小知识

```javascript
npm  install  库        # 安装到项目的 node——modules中了，不会安装任何依赖

npm install  -g xx      # 全局安装到本地磁盘上，通过 prefix可以来改变安装伪装

npm install -save  库     #安装到项目 node_modules中，并安装依赖， 依赖会安装 dependencies生产环境中

npm  install  --seve-dev  库   # 安装到开发环境的依赖下
```

## Redux 

```react
# 1. 安装
 yarn add redux --save
 
 
 #2. 使用 
 创建一个store文件来管理状态
 #index.js  内如如下
 
import {createStore} from 'redux'
// 引入reducer
import reducer from './reducer'
// 创建store仓库, 注入reducer ，可以这样理解仓库添加管理成员
const store = createStore(reducer，
 window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()  加入此串代码可以调试，前提google浏览器安装redux              
                         );
// 导出store
export default store


# reducer 文件内容如下

// 定义一个Components中需要的对象状态
const todoObj = {
    list:[
        'Racing car sprays burning fuel into crowd.',
        'Japanese princess to wed commoner.',
        'Australian walks 100km after outback crash.',
        'Man charged over missing wedding girl.',
        'Los Angeles battles huge wildfires.',
    ]
}

export default (state = {todoObj，多个组件时，依次添加即可} ,action) => {
    return state
}






# 3. 在组件中 使用 store中的状态数据

首页引入 import store from './store/index'  


使用store中的数据：   //store.getState()




```

## Redux 实现流程

```react
// 1. Components 
在 Components 的 constructor 中，添加：
									this.state = store.getState()
//通过 this.state = store.getState()   获取到 store中的state，方便控件赋值使用
									this.storeChange = this.storeChange.bind(this)
									store.subscribe(this.storeChange)
//store 订阅模式来 更新视图state， store中数据状态改变， 会自动执行 store.subscribe() 来更新状态


//完整代码如下：
constructor(props){
        super(props)
        this.state = store.getState()
        this.storeChange = this.storeChange.bind(this)
        store.subscribe(this.storeChange)
    }



# 控件赋值   
this.state.属性/对象



// 2. 通过监听事件， 状态值改变，  通过 store.dispatch(对象)  传递给 store接收来使用
    changeValue(e){
        const action = {
            type:"inputValues",   //type为 自定义名字，用来store来判别接收的action为哪个
            value:e.target.value
        }
        // 将action 发送给 store接收
        store.dispatch(action)
    }

    storeChange(){
        // 通过来获取最新的store中的state
        this.setState(store.getState())
    }






// 3. Reducers 


// 定义一个组件中需要的对象
const todoObj = {
    list:[
        'Racing car sprays burning fuel into crowd.'
    ],
    inputValue:'测试'
}
export default (state = {todoObj} ,action) => {
    console.log(state,action)
    // Reducer 只能接收state，不能改变state
    // 通过来判断action的type 来判断更新哪个state
    if (action.type === 'inputValues') {
        // 通过深拷贝 来获取 components 传过来的action对象
        let newState = JSON.parse(JSON.stringify(state))
        // 改变state值
        newState.todoObj.inputValue = action.value
        // 返回给store，来更新
        return newState
    }
    return state
}




//4.  store

import {createStore} from 'redux'
import reducer from './reducer'
// 创建store仓库, 注入reducer ，可以这样理解仓库添加管理成员
const store = createStore(reducer,window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()) // 创建数据存储仓库);
// 导出store
export default store







```



## 视图UI  于 逻辑 分离

#### UI 

```React
// UI 只负责写页面布局，通过this.props接收使用 逻辑数据和方法



import React, { Component, Fragment } from 'react';
import { Input, Button, List } from 'antd';
import "./todoList.css"
class TodoListUI extends Component {

    render() { 
        return (  
            <Fragment>
            <div className="head">
                <Input placeholder="please in content" style={{ width: '300px' }} onChange={this.props.changeValue} value={this.props.value}></Input>
                <Button type="primary" onClick={this.props.addContent}>添加 </Button>
            </div>
            <div className="content">
                <List
                    size="large"
                    bordered
                    dataSource={this.props.list}
                    renderItem={(item,index) => (<List.Item onClick={() => {this.props.deleteItem(index)}}>{item}</List.Item>)}
                />
            </div>
        </Fragment>
        );
    }
}
 
export default TodoListUI;
```

#### 逻辑

```react
// 逻辑 负责写UI 中所需要的数据和方法， 通过引入UI 组件， 给 组件上 绑定数据和方法
//eg：
<TodoListUI
           value = {this.state.todoObj.inputValue}
           changeValue = {this.changeValue}
           addContent = {this.addContent}
           list = {store.getState().todoObj.list}
           deleteItem = {this.deleteItem}
           />




//完整代码如下：
import React, { Component} from 'react';
import TodoListUI from './todoListUI'
import "./todoList.css"
import store from './store/index'

class TodoList extends Component {
    constructor(props){
        super(props)
        this.state = store.getState()
        console.log(store.getState().todoObj)
        this.changeValue = this.changeValue.bind(this)
        this.addContent = this.addContent.bind(this)
        this.storeChange = this.storeChange.bind(this)
        this.deleteItem = this.deleteItem.bind(this)
        // store 订阅模式来 更新视图state， store中数据状态改变， 会自动执行 store.subscribe() 来更新状态
        store.subscribe(this.storeChange)
    }
    render() {
        return (
           <TodoListUI
           value = {this.state.todoObj.inputValue}
           changeValue = {this.changeValue}
           addContent = {this.addContent}
           list = {store.getState().todoObj.list}
           deleteItem = {this.deleteItem}
           />
        );
    }
    changeValue(e){
        const action = {
            type:"inputValues",
            value:e.target.value
        }
        // 将action 发送给 store接收
        store.dispatch(action)
    }
    storeChange(){
        // 通过来获取最新的store中的state
        this.setState(store.getState())
    }
    addContent(){
        const action = {
            type:'add',
            inValue:this.state.todoObj.inputValue
        }
        store.dispatch(action)
    }
    deleteItem(index){
        console.log(index+"--------------")
        const action = {
            type:'delete',
            indexs:index
        }
        store.dispatch(action)
    }
}

export default TodoList;
```



## 无状态组件 

```react
// 无状态组件 就是一个函数，它创建时始终保持了一个实例，避免了不必要的检查和内存分配，做到了内部优化。
// 好处：  1. 省去考虑this 问题，  2， 代码更少了，提高了性能  3， 没有class一说了

//如何使用：
//定义一个变量， 接收 render 中 return的内容，最后导出去



import React, { Fragment } from 'react';
import { Input, Button, List } from 'antd';
import "./todoList.css"

const TodoListUI = (props) => {
        return (  
            <Fragment>
            <div className="head">
                <Input placeholder="please in content" style={{ width: '300px' }} onChange={props.changeValue} value={props.value}></Input>
                <Button type="primary" onClick={props.addContent}>添加 </Button>
            </div>
            <div className="content">
                <List
                    size="large"
                    bordered
                    dataSource={props.list}
                    renderItem={(item,index) => (<List.Item onClick={() => {props.deleteItem(index)}}>{item}</List.Item>)}
                />
            </div>
        </Fragment>
        );
    }
 
export default TodoListUI;

```

## Redux   axios 获取接口数据

```React
 componentDidMount(){     
     
 // axios 发起请求，将请求到的数据， 派发到 store中    
 axios.get('http://rap2api.taobao.org/app/mock/243473/example/1580477042091').then(res => {
     		// 创建action ，绑定type，和请求到的内容，派发到store中，
            const action = {
                type:'getList',
                data:res.data.data
            }
            store.dispatch(action)

        })
    }




//store中 通过action.type 来判断，执行相应的 组件state数据赋值
export default (state = {todoObj} ,action) => {
     // 首先必须： 通过深拷贝 来获取 components 传过来的action对象
    let newState = JSON.parse(JSON.stringify(state))
    //通过action.type 来判断
    switch (action.type) {
        case 'inputValues': 
        // 改变state值
        newState.todoObj.inputValue = action.value
        // 返回给store，来更新
        break
        case 'add':
            newState.todoObj.list.push(action.inValue)
            break
        case 'delete': 
        // 改变state值
        newState.todoObj.list.splice(action.indexs,1)
        // 返回给store，来更新
        break      
    }
    return newState

}

```

















## React Router

- #### 安装

  ```
  yarn add react-router-dom --save
  ```

### Router 组成

> - 路由器：  `<BrowserRouter>`  和 `<HashRouter>`
> - 路由匹配器：`<Route>` 和  `<Switch>`
> - 导航：  `<Link>`   `<NavLink>`  和 ` <Redirect>`
>
> ```javascript
> import { BrowserRouter, Route, Link } from "react-router-dom";
> 
> ```

#### 路由器

> 每个React Router应用程序的核心应该是路由器组件。
>
> `<BrowserRouter>` 常规的URL路径，但需要配置服务器相应配置。
>
> `<HashRouter>`位置存储在URL 的`hash`一部分中， `#` 
>
> 例如： `http://example.com/#/your/pag`
>
> 由于哈希永远不会发送到服务器，因此这意味着不需要特殊的服务器配置

#### 路由匹配器

> 当 `switch` 被渲染，它会搜索`<Route> ` 内容找到一个`path` 的`url` 渲染。

#### 导航

> `<Link>`  会创建链接的组件，无论`<Link> `正确与否，都会渲染成HTML 中的 `<a>`
>
> `<NavLink>`  它是`<Link>`的一种特殊类型，它是主动 to 到当前位置相匹配。
>
> `<Redirect>` 强制导航到目标`url`, 重定向

### 钩子

> React Router带有一些挂钩，可让您访问路由器的状态并从组件内部执行导航。
>
> - `useHistory`
> - `useLocation`
> - `useParams`
> - `useRouteMatch`

#### `useHistory`

> 使用这个钩子函数可以访问 `history` 实例。

```javascript
import { useHistory } from "react-router-dom";



let history = useHistory();

history.push("/home");

```

#### `useLoaction`

> 该钩子函数可以返回 `loaction`  表示当前`URL`对象。

```javascript
import { useLocation } from "react-router-dom";



let location = useLocation();
//当前url path name
console.log(location.pathname) 
```

#### `useParams`

> 获取当前`URL`参数的键值对对象，

```javascript
import { useParams } from "react-router-dom";


let { slug } = useParams();

console.log(slug) 
```

#### `useRouterMatch`

### `<BrowserRouter>`

#### `basename`

> 所有位置的基本URL。

```html
<BrowserRouter basename="/calendar">
    <Link to="/today"/> // renders <a href="/calendar/today">
    <Link to="/tomorrow"/> // renders <a href="/calendar/tomorrow">
</BrowserRouter>
```

#### `getUserConfirmation`

> 用于确认导航功能，默认使用`window.confirm`

```html
<BrowserRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>
```

#### `forceRefresh` 

> 它为true 时，它会刷新整页导航。

```html
<BrowserRouter forceRefresh={true} />
```

### `<HashRouter>`

> 哈希历史记录不支持`location.key`或`location.state`
>
> ### 配置服务器以供使用`<BrowserHistory>`

#### `basename`

> 所有位置的基本URL。

```html
<BrowserRouter basename="/calendar">
    <Link to="/today"/> // renders <a href="/calendar/today">
    <Link to="/tomorrow"/> // renders <a href="/calendar/tomorrow">
</BrowserRouter>
```

#### `getUserConfirmation`

> 用于确认导航功能，默认使用`window.confirm`

```html
<BrowserRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>
```

###  `<Link>`

####  `to : string`

```html
<Link to="/about">About</Link>


<Link to="/courses?sort=name" />

```

#### `to: object`

> - pathname:   url name
> - search:    查询参数字符串
> - hash：输入网站的hash值
> - state

```html
<Link
  to={{
    pathname: "/courses",
    search: "?sort=name",
    hash: "#the-hash",
    state: { fromDashboard: true }
  }}
/>
```

























