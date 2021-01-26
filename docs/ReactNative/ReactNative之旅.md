## React Native 之旅

### React Native 踩坑开始

> 5.1 假期 就这样短暂的结束了，你都干啥了？ 
>
> 😂，我嘛加了3天班，你们呢？
>
> 
>
> ​       最近公司有个React Native 项目，不得不学习下RN了。由于之前学过React，学React Native 过程还算可以，不太艰难。
>
> 学习React Native 踩了好多坑，总结文章，以便别的小伙伴开发RN,可以轻松上手，减少踩坑。😂

<hr>

#### React Native  环境 安装(必须按照以下3个要求配置安装，否则会环境错误)

- ##### Python 2.x ， 必须安装2.x 的版本。 

- ##### Node， 版本必须在12或者大于12

- ##### Java JDK 环境 必须为 1.8 版本

<img src="https://s1.ax1x.com/2020/05/05/Ykm5nI.png">

<center><h3>缺少Python 环境</h3></center>

#### 项目依赖安装工具

- ##### 首选 yarn ，安装国外资源依赖快

- ##### Npm， 下载速度会很慢，可以设置淘宝源，加快速度

#### 手机模拟器 调试

- ##### 安装 Android Studio  （需要配置AS 的环境，这里就不介绍了）

- ##### 夜深模拟器  

- ##### 其它模拟器

<hr/>

#### 安装 React Native 脚手架

> 我之前是按照官方提供的脚手架安装的，出现各种坑，创建好了项目，启动项目，各种报错，最后各种百度，没果。
>
> 后来选择了EXPO，可真香。



##### 官方提供 脚手架

``````javascript
1. 全局安装脚手架
npm uninstall -g react-native-cli
2. 创建新项目
react-native init  ProjectName
3. yarn start
``````

#### 安装 EXPO 脚手架

##### 什么是EXPO

> [Expo](http://expo.io/)是通用React应用程序的框架和平台。它是围绕React Native和本机平台构建的一组工具和服务，可帮助您从同一JavaScript / TypeScript代码库在iOS，Android和Web应用程序上开发，构建，部署和快速迭代。

##### 安装脚手架

```javascript
1. 全局安装 EXPO
 推荐用 yarn 安装//  
 npm install --global expo-cli
（当时用npm，安装了半个小时，也没安装完......😭）

2. 创建项目
expo init my-project

```

#### EXPO 提供了 很方便开发便捷

> 从项目的开发 到 最终的上线， 都很轻松。
>
> 当你想打包你的App 成APK 文件：
>
> ​	你可以是使用EXPO 提供的 指令： `expo build:android`   
>
> 打包的时候，会需要EXPO的账户， 因为它会发布到你EXPO账户下，生成APK 文件 ，发布到应用商城，需要证书或者资料， 它会给你生成一个， 完全不用我们操心，只关注编码。

#### 光说不练  纯耍流氓，下面为EXPO 操作演示

##### 创建项目

<img src="https://s1.ax1x.com/2020/05/05/Yki6DU.png" >

##### 下载依赖会需要一段时间

<img src="https://s1.ax1x.com/2020/05/05/YkF7zq.png">

#### EXPO 项目介绍

<img src="https://s1.ax1x.com/2020/05/05/YkEexI.png">



#### 启动项目 `yarn start` 

> 启动成功后，它会开启一个服务，会自动打开一个网页，在这个网页中，你只需要把你的 手机模拟器 或者 真机 连着电脑， 然后 点击 `Run on Android device` 就可以运行在手机上了。
>
> 是不是很轻松哈，使用官方提供的，你的自己配置，查找模拟器。😝

​	

<img src="https://s1.ax1x.com/2020/05/05/Yke9rq.png">

#### React Native 支持热更新

> 这样很方便我们开发APP 中调试， 数据改变， 视图同时改变。

#### 打包项目

> 由于我是Windows 环境，在这里就只介绍 如何打包 Android  APK 文件了。
>
> IOS 打包 去官方读文档也可以，文档可能对国人不太友好，纯英文，翻译工具可以帮到我们。

<img src="https://s1.ax1x.com/2020/05/05/YkuXmn.png">

<center><h3>打包成功，它会提供一个链接，去这个链接你就可以下载打包后的APK 文件</h3></center>



#### 下载APK 

<img src="https://s1.ax1x.com/2020/05/05/YkK80I.jpg">

##### 显示效果

<img src="https://s1.ax1x.com/2020/05/05/YkKrBn.jpg">





### 到此该结束了

> 本章介绍了，如何配置React Native 环境， 以及EXPO 神器如何使用，从 0 到  打包成 APK 文件流程。
>
> 中间我踩了很多坑，写文章记录下来，别的朋友就可以减少踩坑的时间，专注业务开发方面，从而开发出优质的APP 应用。

<img src="https://s1.ax1x.com/2020/05/05/Yk1CU1.jpg">



<center><h3>祝大家，5.1 快乐</h3></center>



#### 动画入门使用

> 1.  导入 动画库， 不需要安装。
>
>    `import { Animated} from 'reac-native';`
>
> 2.  设置动画状态
>
>    ```react
>    state = {
>    
>    ​	top:  new  Animated.Value(900)
>    
>    }
>    ```
>
>    
>
> 3. 创建动画应用于视图中
>
>    ```javascript
>    const AnimatedContainer =  Animated.createAnimatedComponent(Container)`
>    
>    `Container`  为 自定义的样式组件
>    ```
>
>    
>
> 4. 将动画状态应用于样式组件中
>
>    ```react
>      <AnimatedContainer style={{ top: this.state.top}}>
>                 
>       </AnimatedContainer>
>    ```
>
> 5. 给动画添加执行事件
>
>    ```react
>    componentDidMount(){
>            Animated.spring(this.state.top,{
>                toValue:0
>            }).start()
>        }
>    ```
>
>    

##### 完整代码

```react
import React from 'react';
import styled from 'styled-components';
import { Animated } from 'react-native';

class Menu extends React.Component {

    state = {
        top: new Animated.Value(900)
    }

    componentDidMount() {
        Animated.spring(this.state.top, {
            toValue: 0
        }).start()
    }

    render() {
        return (
            <AnimatedContainer style={{ top: this.state.top }}>
                <Cover></Cover>

            </AnimatedContainer>
        );
    }
}

export default Menu;


const Container = styled.View`
position:absolute;
background:white;
width:100%;
height:100%;
z-index:100;
`

const AnimatedContainer = Animated.createAnimatedComponent(Container)

const Cover = styled.View`
width:100%;
height:200px;
background:deepskyblue;


`
```

##### 效果如下

<img src="https://s1.ax1x.com/2020/05/03/JzRz1s.gif">

##### 弹起Menu的高度，自设配机型高度

```javascript
1. 从 react-native 中 引入 Dimensions，
import { Animated, Dimensions} from 'react-native';

2. 动态获取屏幕高度
const screenHeight = Dimensions.get("window").height;

3.改变动画视图高度
    state = {
        top: new Animated.Value(screenHeight)
    }

4.改变事件中高度
    toggleMenu = () => {
        Animated.spring(this.state.top, {
            toValue: screenHeight
        }).start()
    }
5. 动态修改样式组件中的 视图的高度， 通过 $ {} ，来修改
        const Container = styled.View`
        position:absolute;
        background:white;
        width:100%;
        height:${screenHeight};
        z-index:100;
      `
        
        
6.大功告成， 不管是什么机型都适配        
```

#### Redux

##### 1. 安装依赖

```javascript
yarn add  redux -s

yarn add react-redux -s
```

##### 2.  在入口文件 app.js 使用

```react
import React from 'react'
import { createStore } from 'redux'
//使用Provider达到数据共享
import { Provider } from 'react-redux'
import HomeScreen from './screen/HomeScreen'


// 存储组件状态
const initialState = {
  action: "",
  test:"测试"
};


// reducer 处理状态,将状态返回到组件中使用
// reducer 传递两个参数 一个为 state 状态,  第二个参数为 action 改变state
const reducer = (state = initialState, action) => {
  // 通过接收action.type 来 处理相应的组件需要的state状态
  switch (action.type) {
    case 'CLOSE_MENU':
      return { action: 'closeMenu' }
      
    case 'OPEN_MENU':
      return { aciton: 'openMenu' }
    case 'CHANGE_TEXT':
      return { test : '修改成功'} 
    default:
      return state
  }

}

// 创建store 仓库
const store = createStore(reducer);

const App = () => (
  //  使用 Provider 包裹组件， 传递store， 所有组件可使用 store中参数和reducer
  <Provider store={store}>
    <HomeScreen />
  </Provider>
)

export default App;
```

##### 3. 组件获取state

```react
// 1.第一步必须 引入 import { connect } from 'react-redux';
import { connect } from 'react-redux';


//2. 通过  mapStateToProps(state) 来获取store中的state


//  通过React-redux 的 mapStateToProps 来传递store中的 state 来 给组件使用
function mapStateToProps(state){
    // 通过 state。来获取组件需要的 state，最后返回，
    //  因为 使用connnect 连接了 mapStateToProps,并且也绑定了 组件， 所以组件和 store 是共享数据 使用的
    return {action: state.action}
}




//3. 组件中使用state
this.props. [mapStateToProps返回的对象]
```

##### 4. 改变state， dispatch

```react
//1. 通过react-reudx 的 mapDispatchToProps 函数来 出发 reducer 来改变state状态，通过指定 action 的type，
//  然后在 store 中判断action.type 来处理 相应的业务
function mapDispatchToProps(dispatch) {
    return {
        closeMenu: () => 
            dispatch({
                type: "CLOSE_MENU"
            })
    } 
}



//2，通过在 控件上绑定事件，触发 mapDispatchToProps 函数中返回的函数
```

##### 5.  使用connect 连接 state和action，绑定组件

```react
//  重点:
//      要想给组件传递state 或者 改变 state, 必须通过react-redux 的 connect 连接  mapStateToProps , mapDispatchToProps 函数, 并且绑定组件
export default connect(mapStateToProps,mapDispatchToProps)(Menu);
```





<hr/>

#### Fetch 请求

> ReactNative 中请求接口的库。 它提供了一个 JavaScript 接口，用于访问和操纵 HTTP 管道的一些具体部分，例如请求和响应。它还提供了一个全局 [`fetch()`](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch) 方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

##### 使用

```javascript
fetch('接口url')
    .then(
    response => {
    // 1.获取JSON内容， 第一步需要转换成 json数据
      response = response.json()
})
  .then(
    response => {
        //切记，由于这块要将数据赋值给state，RN的执行顺序是：
        //由于请求是异步加载，而render渲染在异步前，数据赋值是不能的，直接报错，没有赋值上去。
        // 2. ✔ 正确的解决办法，React中提供了，在给异步赋值时， 传递一个空对象即可
        
            this.setState(() => {
                return { info: response.article }
            })
    }
			

);
```

<img src="https://s1.ax1x.com/2020/05/04/YCYRAI.png">

##### 渲染结果

<img src="https://s1.ax1x.com/2020/05/04/YCtPb9.png">





> 本文就不过多讲有关Fetch内容，主要以可以在RN 中快速可以请求，拿到数据，渲染为例。
>
> 更多有关Fetch使用在下面链接
>
> https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

#### 导航Navigation



> 安装 Navigation 
>
> `npm install -s react-navigation`