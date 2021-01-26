### Axios 封装

#### 定义

> Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

#### 特性

- #####  支持Promise API

- ##### 拦截请求和响应

- ##### 转换请求数据和响应数据

- ##### 自动转换JSON数据

- ##### 客户端支持 XRSF

#### 回归正题

> 在Vue 项目开发中，我们与接口打交道最多了，来通过接收后端接口返回来的数据，最后我将这些接口数据完美的呈现到网页上。
>
> 同时，与接口打交道那么就会用到网络请求，与 `Vue ` 结合的网络请求库有哪些呢？
>
> - `vue-resource`
> - `axios`   官方推荐
> - `fetch`
>
> 本章将使用 ` axios`  来完成接口的请求，以及对`axios` 请求的封装，来满足业务开发。
>
> `一次编写， 终身受用` 😁

#### 开始

##### 安装axios

> yarn add axios 

##### 封装http.js

> 创建单独文件来封装axios，封装的同时，你需要和 `后端`  协商好一些约定，`请求头` ， `状态码`, `请求超时时间`.......
>
> 引入`必要的UI 提示框`, 不同的状态码，提示不同的响应，
>
> `请求头` :  来实现一些具体的业务，必须携带一些参数才可以请求`(例如：会员业务)`
>
> `状态码` :   根据接口返回的不同`status` ， 来执行不同的业务，这块需要和后端约定好。
>
> ` 请求拦截器`:  根据请求的请求头设定，来决定哪些请求可以访问。
>
> `响应拦截器`： 这块就是根据 `后端` 返回来的状态码判定执行不同业务

<img src="https://ae01.alicdn.com/kf/Hbc1a843a703040f5ab31c2870889b1c0H.jpg">

<center><h3>完整代码</h3></center>

#### 配置多域名请求不同URL

> 一般自己写项目时， 一个接口URL 就可以了。但在实际项目开发中，一个项目可能会请求不同的服务器的url，这时，我们简单的配置下访问接口域名，然后不同域名的接口，直接换对象调用即可，这样不管有多少个不同的接口，我们都可以很好的管理使用。

<img src="https://ae01.alicdn.com/kf/H3940a1bcbc524855978b052b6347b44d6.jpg">

<center><h3>完整代码</h3></center>



#### 到现在 axios 基本封装完成，可以满足你基本业务需求了

> `axios` 封装完事了， 接下来就是封装单独的业务模块请求了，这块怎么划分 完全看`个人风格`,， 下面我会列出 ` 两种` 
>
> `业务需求注意:`
>
> - `必须引入 http.js`  axios
> - `必须引入  base.js`  接口url
> - `必须在Vue 入口文件下，引入业务需求 api.js,并且将api挂载到Vue 原型上`
> - `剩下就是写你对应的业务需求了`

 ##### 风格1

> 所有请求都写到一个`api.js`  文件下

<img src="https://ae01.alicdn.com/kf/H91b9b0a415564262976bd595461847d5N.jpg">

<center><h3>完整代码</h3></center>

###### 如何使用呢？

###### 全局挂载`api.js`

<img src="https://ae01.alicdn.com/kf/Hfc9dcb1326d34ce5a1fe1eac64e0b3ddo.jpg">

###### 业务组件调用

<img src="https://ae01.alicdn.com/kf/Hfce7af856b3a4af5b96c71713a43bcaag.jpg">

<hr/>



##### 风格2

> 可以`新建对应组件模块的文件`来管理对应的 业务请求，这样接口出现问题，`定位错误快`，最后将不同的文件 引入到一个 api,js 里， 这样管理起来很方便。

<img src="https://ae01.alicdn.com/kf/H491652a1e9624ba5967f2b415802b72fG.jpg">

<img src="https://ae01.alicdn.com/kf/H758b97644514448697693f4a85af602bU.jpg">

##### 如何使用呢？

<img src="https://ae01.alicdn.com/kf/Ha9b0dc9f4010471fa77955548d5b6dc1x.jpg">

#### 封装 与 不封装对比

<img src="https://ae01.alicdn.com/kf/H5835e6968ff9459299afce8389d4d71fz.jpg">

<center><h2>没有封装， 裸奔的Axios</h2></center>



<img src="https://ae01.alicdn.com/kf/H6ff327b4c61a4388bdd70262b48baf1bZ.jpg">



<center><h2>最后</h2></center>

> 到现在，Axios基本封装完事了，也封装了业务模块的请求，基本上可以满足基本的业务需求了。如果项目还需要其它需求，还可以在原有的上面进行再次封装。
>
> 封装后，如果项目由接口域名有变动，执行调用base.js下的域名对象即可。



