![](https://user-gold-cdn.xitu.io/2020/3/29/171268f6b5fd4d0a?w=1050&h=591&f=jpeg&s=10834)



## Vue Router

#### 路由初体验

##### 安装

```javascript
npm install --save  vue-router
```

##### 使用 

`router.js  路由配置`  

```js
@ 如果你的Vue-Cli 是 3 的版本，可以在创建项目的同时，可以选择安装router 插件
vue ui 

1. 创建好项目，自带的router文件就是路由配置文件



import Vue from 'vue'
//  引入  vue-router
import VueRouter from 'vue-router'
import Father from '../views/Father.vue'

2. 在全局使用这个vue-router
Vue.use(VueRouter)


3. 配置路由选项
const routes = [
  {
    path: '/',
    name: 'Father',
    component: Father,
    meta: {
      keepAlive: true // 需要缓存
    }
  }
]

3. 创建Vue-router实例，挂载router配置项
const router = new VueRouter({
  routes
})

4.最后导出  Vue-router实例， 提供给 Vue 挂载使用
export default router


```

`index.js   Vue入口文件配置`

```javascript
import Vue from 'vue'
import App from './App.vue'
1. 引入 路由配置文件
import router from './router'
import store from './store'

Vue.config.productionTip = false

2. 将路由挂载到 Vue实例中使用。
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```



#### 动态路由传递参数

```javascript
const routes = [
  {
    1. 通过在url后：参数即可
    path: '/home/:name',
    name: 'home',
    component: Father,
    meta: {
      keepAlive: true // 需要缓存
    }
  }
]



2.接收路由参数：  this.$route.params.路由参数
```

#### 捕获404页面

```javascript
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}


当使用一个通配符时，$route.params 内会自动添加一个名为 pathMatch 参数。它包含了 URL 通过通配符被匹配的部分：

1. this.$route.params.pathMatch  来获取通配符后的url
```

#### 嵌套路由

```javascript
1. 通过在父路由中添加 children 数组，直接引入嵌套组件和配置path
2. 页面中使用 通过 <router-link to="/父路由path/嵌套path">小红</router-link>
3. 配置显示入口   <router-view></router-view>

{
    path: '/teacher',
    name: 'teacher',
    component: () => import('../views/Teacher/Teacher.vue'),
    children: [
      {
        path: 'xiaohong',
        component: () => import('../views/Teacher/XiaoHong.vue')
      },
      {
        path: 'xiaoming',
        component: () => import('../views/Teacher/XiaoMing.vue')
      }
    ]
  }
```

<img src='https://ae01.alicdn.com/kf/H506b468b3bad4624bc69cd9c795c7406n.gif'>

#### 编程式导航

> 导航分为：  
>
> `router.push `来实现 编程式导航 
>
> `<router-link>` 声明式导航    创建 a 标签来定义导航链接
>
> `router.push`会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

##### ` router.push`

```javascript
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
```

##### `router.replace`

> 它不会向 history 添加新记录，它会替换当前path

```javascript
1.声明式写法
<router-link :to="..." replace>
    
2.编程式写法
router.replace(...)
```

##### `router.go(n)`

> 路由的前进和后退，  前进传入整数，  后退传入负数

#### 命名路由

> 所谓命名路由，给路由配置name属性， 然后在页面中也可以使用  `this.$router.push({ name: 'home'，{params:name='Test'}})`进行路由跳转传递参数， 很方便。

```javascript
const routes = [
  {
    path: '/home:name',
    name: 'home',
    component: Father,
  }
]



1 通过命名路由来实现 路由跳转和传递参数
this.$router.push({ name: 'home'，{params:name='Test'}})
```

#### 重定向

```javascript
1. redirect: '/b'
2. 重定向的目标也可以是一个命名的路由：  redirect: { name: 'foo' }
```

#### 路由守卫

> ​        当你访问Web具体某个页面时，例如个人主页，虽然你记住个人主页url，但是通过路由守卫功能就会判别你是否有权限进入该页面，在没权限的时候，做相应处理，重定向到登陆页。

```javascript

router.beforeEach((to, from, next) => {
    //我在这里模仿了一个获取用户信息的方法
  let isLogin = window.sessionStorage.getItem('userInfo');
    if (isLogin) {
        //如果用户信息存在则往下执行。
        next()
    } else {
        //如果用户token不存在则跳转到login页面
        if (to.path === '/login') {
            next()
        } else {
            next('/login')
        }
    } 
})
```

#### 路由过渡特效

```javascript
1.给路由入口加入 transition


<transition>
  <router-view></router-view>
</transition>



2. 实现不同的过渡特效 ，组件内使用 <transition> 并设置不同的 name。
 <transition name="long">
      <div class="main">...</div>
  </transition>
```

#### 路由懒加载

> 官方： 当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

```javascript
1. 所有路由都放置在一个同步块中 .routerPath.js 中

const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')


2. 在 router 文件中正常使用就可，使用 chunk name  来赋值 component
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }，
    { path: '/bar', component: Bar }，
    { path: '/baz', component: Baz }
  ]
})

```





<center><h4>✨觉得不错的点赞，帮忙转发分享以下，原创不易！
<center><h3>🌟关注微信公众号'前端自学社区' 获取更多资料✨<h3/><center/>


![](https://user-gold-cdn.xitu.io/2020/3/29/17126937821820ec?w=344&h=344&f=jpeg&s=8820)

<center><h3> 💥回复加群 可以加入 自学前端群💥

