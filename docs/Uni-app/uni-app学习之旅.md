## uni-app 踩坑之旅

###  uni-app 项目目录

> - components             放置可复用组件
> - pages                          放置业务页面
> - static                           存放所有静态文件
> - App.vue                      监听全局的声明周期，全局的样式
> - manifest.json           App  小程序的 具体配置
> - pages.json                 页面路由的关系，导航信息..放置在Pages数组中的`第一个对象`为启 动页       
>
> 
>
> - ┌─components            uni-app组件目录
>   │  └─comp-a.vue         可复用的a组件
>   ├─hybrid                存放本地网页的目录，[详见](https://uniapp.dcloud.io/component/web-view)
>   ├─platforms             存放各平台专用页面的目录，[详见](https://uniapp.dcloud.io/platform?id=%E6%95%B4%E4%BD%93%E7%9B%AE%E5%BD%95%E6%9D%A1%E4%BB%B6%E7%BC%96%E8%AF%91)
>   ├─pages                 业务页面文件存放的目录
>   │  ├─index
>   │  │  └─index.vue       index页面
>   │  └─list
>   │     └─list.vue        list页面
>   ├─static                存放应用引用静态资源（如图片、视频等）的目录，**注意：**静态资源只能存放于此
>   ├─wxcomponents          存放小程序组件的目录，[详见](https://uniapp.dcloud.io/frame?id=%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%BB%84%E4%BB%B6%E6%94%AF%E6%8C%81)
>   ├─main.js               Vue初始化入口文件
>   ├─App.vue               应用配置，用来配置App全局样式以及监听 [应用生命周期](https://uniapp.dcloud.io/frame?id=%E5%BA%94%E7%94%A8%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
>   ├─manifest.json         配置应用名称、appid、logo、版本等打包信息，[详见](https://uniapp.dcloud.io/collocation/manifest)
>   └─pages.json            配置页面路由、导航条、选项卡等页面类信息，[详见](https://uniapp.dcloud.io/collocation/pages)                                                                      

### 资源路径

#### 引入 静态资源

> 在`template`中可以引入静态资源的 `相对定位`和`绝对定位` 

```html
<!-- 绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="@/static/logo.png"></image>
<!-- 相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

#### 引入js文件

```javascript
// 绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// 相对路径
import add from '../../common/add.js'
```

#### 引入CSS 资源

> `css`文件或`style标签`内引入`css`文件时（scss、less文件同理），只能使用相对路径

```javascript
/* 绝对路径 */
@import url('/common/uni.css');
/* 相对路径 */
@import url('../../common/uni.css');
```

> `css`文件或`style标签`内引用的图片路径可以使用相对路径也可以使用绝对路径.   
>
> `在小程序中不可以引入本地图片`

```css
/* 绝对路径 */
background-image: url(@/static/logo.png);
/* 相对路径 */
background-image: url(../../static/logo.png);
```

### 生命周期

> - `onLaunch`     uni-app 初始化时，调用(只调用一次)
> - `onShow`   uni-app 启动时调用， 后到前
> - `onHide`    uni-app    从前到后，调用
> - `onError`  uni-app 报错时触发
> - `onUniNViewMessage`    对 `nvue`  页面发送数据监听

#### 页面生命周期

### 路由

####   路由跳转

> - 1.组件方式跳转
> - 2.API跳转

##### 1.API 跳转

###### 1.uni.navigateTo

> 保留当前页面，跳转到某个页面。
>
> 某个页面接收目标页参数，可以在`onLoad` 生命周期中获取。
>
> 具体配置： https://uniapp.dcloud.io/api/router?id=navigateto

```javascript
1. 跳转到目标页
uni.navigateTo({
    url: 'test?id=1&name=uniapp'
});

//具体详情配置  



2. 在跳转页接收上一个页面传递过来的参数

 onLoad: function (option) {
     //option为object类型，会序列化上个页面传递的参数 
 }
```

###### 2. uni.redirectTo

> 关闭当前页面，跳转到某个页面
>
> 具体配置： <https://uniapp.dcloud.io/api/router?id=redirectto>

``` javascript
uni.redirectTo({
    url: 'test?id=1'
});
```

###### 3. uni.reLaunch

> 关闭所有页面，打开到应用内的某个页面。

```javascript
uni.reLaunch({
    url: 'test?id=1'
});
```

###### 4.uni.switchTab

> 跳转到tabBar 页面，并关闭其他所有非 tabBar 页面。

```javascript
uni.switchTab({
    url: '/pages/index/index'
});
```

###### 5.uni.navigateBack

> 关闭当前页面，返回上一页面或多级页面
>
> 具体配置： <https://uniapp.dcloud.io/api/router?id=navigateback>

```javascript
uni.navigateBack({
    delta: 2
});
```



#### 2.组件方式跳转 `navigator`

###### 使用

```html
<navigator url="navigate/navigate?title=navigate">

</navigator>
```

###### 接收参数

```javascript
 onLoad: function (option) { 
     //option为object类型，会序列化上个页面传递的参数

    }
```

###### 注意

> `url` 有长度限制，传递过长的参数会失败，可以通过 `encodeURIComponent`等方式解决

```html
<navigator :url="'/pages/navigate/navigate?item='+ encodeURIComponent(JSON.stringify(item))">
</navigator>


```

###### 接收太长的参数

```javascript

// navigate.vue页面接受参数
onLoad: function (option) {
    const item = JSON.parse(decodeURIComponent(option.item));
}
```

### 数据共享的四种方法

#### 创建公共文件

> 常用的方法或者变量名可以定义在一个固定文件下，然后导出即可。
>
> 在用的时候，需要每次导入该文件，然后直接通过`对象.属性  /   对象.方法` 来获取文件内容

##### 定义公共文件

```javascript
//    /common/helper.js
const websiteUrl = 'http://uniapp.dcloud.io';  
const now = Date.now || function () {  
    return new Date().getTime();  
};  
const isArray = Array.isArray || function (obj) {  
    return obj instanceof Array;  
};  

export default {  
    websiteUrl,  
    now,  
    isArray  
}

```

##### 组件使用公共文件

```javascript
<script>  
    // 引入公共文件
        import helper from '../../common/helper.js';  

    export default {  
        data() {  
            return {};  
        },  
        onLoad(){  
            //使用该文件
            console.log('now:' + helper.now());  
        },  
        methods: {  
        }  
    }  
</script>
```



#### 挂载Vue.prototype 

> 将属性和方法挂载到`Vue.prototype `，这样Vue 组件都可以访问的到
>
> 访问格式：` this.getName  `
>
> 在`main.js 文件下挂载`

##### `main.js`

```javascript
Vue.prototype.websiteUrl = 'http://uniapp.dcloud.io';  
Vue.prototype.now = Date.now || function () {  
    return new Date().getTime();  
};  
Vue.prototype.isArray = Array.isArray || function (obj) {  
    return obj instanceof Array;  
};
```

##### `componentA`

```javascript
<script>  
    export default {  
        data() {  
            return {};  
        },  
        onLoad(){  
            // 通过this。  调用
            console.log('now:' + this.now());  
        },  
        methods: {  
        }  
    }  
</script>
```

#### `globalData` 

> 小程序中有这个`globalData`概念，它在`App.vue` 中实现挂载。
>
> 它已经在其它平台也已经实现了。

##### 定义 `globalData` 变量 / 方法

```javascript
<script>  
    export default {  
		// 在这块定义变量或者方法
        globalData: {  
            text: 'text'  
        },  
        onLaunch: function() {  
            console.log('App Launch')  
        },  
        onShow: function() {  
            console.log('App Show')  
        },  
        onHide: function() {  
            console.log('App Hide')  
        }  
    }  
</script>  
```

##### 使用 `globalData`  赋值 /  取值

> 赋值：`getApp().globalData.text = 'test'`
>
> 取值：`console.log(getApp().globalData.text) // 'test'`	

#### `Vuex`

> `uni-app`已经内置了`vuex`,用法和Vue 一样。
>
> 创建`store.js`文件，来管理状态。

#####  创建 store.js

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
    state: {},
    mutations: {},
    actions: {}
})
export default store
```

#####  挂载store  到 Vue.prototype 

```javascript
import Vue from 'vue'
import App from './App'
//引入vuex
import store from './store'
//把vuex定义成全局组件
Vue.prototype.$store = store
Vue.config.productionTip = false

App.mpType = 'app'

const app = new Vue({
    ...App,
//挂载
    store
})
app.$mount()
```

##### 在组件中使用vuex

```javascript
this.$store.state
```

### 事件

```javascript
{
    click: 'tap',
    touchstart: 'touchstart',
    touchmove: 'touchmove',
    touchcancel: 'touchcancel',
    touchend: 'touchend',
    tap: 'tap',
    longtap: 'longtap',
    input: 'input',
    change: 'change',
    submit: 'submit',
    blur: 'blur',
    focus: 'focus',
    reset: 'reset',
    confirm: 'confirm',
    columnchange: 'columnchange',
    linechange: 'linechange',
    error: 'error',
    scrolltoupper: 'scrolltoupper',
    scrolltolower: 'scrolltolower',
    scroll: 'scroll'

}
```

### 表单获取值

>通过在元素身上绑定`input`  事件来监听输入值，然后通过 `e.detail.value`  来给 `value` 赋值即可。

```javascript
//完整代码

<input placeholder="请输入密码" :value="username" @input="setUsername"></input>

setUsername(e){
	this.username = e.detail.value
},

```

### 时间选择器

> 插件<https://ext.dcloud.net.cn/plugin?id=2287>

