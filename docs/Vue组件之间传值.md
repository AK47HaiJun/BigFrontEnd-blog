<img src="https://s3.ax1x.com/2021/01/24/sq9Vm9.jpg">







### 前言
> &nbsp;&nbsp;&nbsp;&nbsp;做了半年的公司系统，终于就在前天上线了。后期改BUG时间拖得太长了，出现的大部分BUG 是 前端  与后端 信息不对称导致的，逻辑性错误很不多，用户体验上稍微差点，毕竟第一次做这么大的系统(100w+)，通过这次系统的开发，总结了不少经验，如何更好的跟后端人员协作开发以及如何设计来提高用户体验上，之前自己做开发没关注这方面，只注重功能实现，后期的这块多补补。 <br/><br/>&nbsp;&nbsp;&nbsp;项目上线后，接下来就是后期的维护更新了，最近时间终于不是之前那么忙碌了，简单的对系统做了下复盘。 由于项目采用的技术栈是`Vue`， 平常开发只注重功能实现了，接下来陆续会对 ` Vue` 深入分析,来封装常用业务组件，以及`Vue源码解析`<br/><br/> &nbsp;&nbsp;&nbsp;本章将是对 ` Vue `组件通信的8方法总结，日常开发组件通信密切，熟悉组件通信可以更好的开发业务。





![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3245a3c0598b4b79b32f60649ded9cca~tplv-k3u1fbpfcp-watermark.image)





##   `Vue` 组件之间传值



### 1. 父组件 向 子组件 传递值

> - 1. 在父组件中引入子组件
> - 2. 注册子组件
> - 3. 在页面中使用，子组件标签上 动态绑定传入动态值  /    静态值
> - 4. 在子组件中，使用 `  props`  来接受 `  父组件 ` 传递过了的值
>
> **子组件接收的父组件的值分为引用类型和普通类型两种：**
>
> - **普通类型：字符串（String）、数字（Number）、布尔值（Boolean）、空（Null）**
> - **引用类型：数组（Array）、对象（Object）**

```html
#父组件

<template>
  <div>
    <!-- 传递值 -->
    <Test
     :obj="obj" 
     info="测试"/>
  </div>
</template>

<script>
// 引入子组件
import Test from "../components/Test.vue";
export default {
  name: "about",
  // 注册子组件
  components: {
    Test,
  },
  data() {
    return {
      obj: {
        code: 200,
        title: "前端自学社区",
      },
    };
  },
};
</script>

```



````html
<template>
    <div>
        <h1>{{obj.code}}</h1><br>
        <h2>{{obj.title}}</h2>
        <h3>{{info}}</h3>
    </div>
</template>

<script>
    export default {
        name:'test',
        props:{
            obj:Object,
            info: [String,Number]  //info值为其中一种类型即可，其他类型报警告
        }
    }
</script>
````



> 由于 ` Vue` 是 单向数据流， `  子组件`  是不能直接 修改 ` 父组件` 的 值。



### 2. 子组件 向父组件传递值

>  子组件通过绑定事件，通过 `  this.$emit('函数名'，传递参数)`

```html
#父组件

<Test
     :obj="obj" 
     info="测试"
     @modify="modifyFatherValue"/>

<script>
// 引入子组件
import Test from "../components/Test.vue";
export default {
  name: "about",
  // 注册子组件
  components: {
    Test,
  },
  data() {
    return {
      msg:'我是父组件'
    };
  },
  methods:{
    // 接受子组件传递来的值，赋值给data中的属性
    modifyFatherValue(e){
      this.msg = e
    }
  }
};
</script>
```



```html
# 子组件

<button @click="modifyValue">修改父组件的值</button>


<script>
    export default {
        name:'test',
        methods:{
            modifyValue(){
                this.$emit('modify','子组件传递过来的值')
            }
        }
    }
</script>
```

### 3. 父组件 通过  `  $refs`  /   `$children ` 来获取子组件值

> ` $refs` : 
>
> -  获取DOM 元素 和 组件实例来获取组件的属性和方法。
> - 通过在 子组件 上绑定 ` ref` ，使用 ` this.$refs.refName.子组件属性 / 子组件方法`
>
> 
>
> ` $children`  :  
>
> - 当前实例的子组件，它返回的是一个子组件的集合。如果想获取哪个组件属性和方法，可以通过  `  this.$children[index].子组件属性/f方法`



#### 示例 Text 组件

```html
<script>
    export default {
        name:'test',
        data() {
            return {
                datas:"我是子组件值"
            }
        },
        props:{
            obj:Object,
            info: [String,Number]
        },
        methods:{
            getValue(){
                console.log('我是Test1')
            }
        }
    }
</script>
```



#### 示例 Text2组件

```html
<template>
    <div>
        <h1>我是Test2</h1>
    </div>
</template>

<script>
    export default {
        name:'test',
        data() {
            return {
                datas:"我是Test2"
            }
        },
        created(){
         console.log( this.$parent.obj ) 
         this.$parent.getQuery()
        },
        methods:{
            getTest2(){
                console.log(this.datas)
            }
            
        }
    }
</script>
```





####  `refs`

```html
<template>
  <div>
   // 给子组件上绑定 ref  
    <Test
      ref="son"
    />
   <Test2/>
  </div>
</template>

// 通过 $refs 示例来获取 子组件的属性和方法

 console.log( this.$refs.son.datas) 

 this.$refs.son.getValue()
```



#### ` $children` 

```javascript
//  通过 $children  来获取 子组件的属性和方法
      this.$children[0].getValue(); // 我是 Test1
      this.$children[1].getTest2();  //我是 Test2
      console.log(`---------${this.$children[1].datas}`); //我是Test2
```



### 4. 子组件 通过  `  $parent`  来获取父组件实例的属性和方法

```html
<script>
    export default {
        name:'test',
        created(){
         console.log( this.$parent.obj ) 
         this.$parent.getQuery()
        },
        
    }
</script>
```





### 5. ` $attrs`  和  `  $listeners`   获取父组件实例属性和方法(组件嵌套情况下使用)

> **`$attrs`**：包含了父作用域中不被认为 (且不预期为) ` props` 的特性绑定 (class 和 style 除外)，并且可以通过 v-bind=”` $attrs`” 传入内部组件。当一个组件没有声明任何 ` props` 时，它包含所有父作用域的绑定 (class 和 style 除外)。
>
> 
>
> 
>
> **`$listeners`**：包含了父作用域中的 (不含 .native 修饰符) v-on 事件监听器。它可以通过 v-on=”`$listeners`” 传入内部组件。它是一个对象，里面包含了作用在这个组件上的所有事件监听器，相当于子组件继承了父组件的事件。
>
> 
>
> #### 使用场景： 多层嵌套组件的情况下使用，可以避免使用Vuex来做数据处理， 使用 `  v-bind="$attrs" v-on="$listeners"`  很方便达到业务数据传递。

#### 父组件

```html
<template>
    <div>
        <Test3  
        :status="status" 
        :title="title"
        @getData="getData" />
    </div>
</template>

<script>
import Test3 from "../components/Test3.vue";
    export default {
        name:'person',
        data(){
            return {
                title:'personal 组件',
                status: false
            }
        },
        methods:{
          getData(){
              console.log(this.title)
          }  
        },
        components:{
            Test3
        }
    }
</script>
```



#### 子组件1

```html
<template>
  <div>
    <h1>Test3 组件</h1>
    <br /><br />
    // 通过 $attrs（属性，除了【props中定义的属性】）  和 $listeners（方法）  来给嵌套子组件传递父组件的属性和方法
    <Test4   v-bind="$attrs" v-on="$listeners"/>
  </div>
</template>

<script>
// 引入子子组件   
import Test4 from "../components/Test4";
export default {
  name: "test3",
  props: ["title"],
  components: {
    Test4,
  },
  created() {
    console.log(this.$attrs);  //{status: false}
    console.log("-----------");
    console.log(this.$listeners); // {getData: ƒ}
  },
};
</script>
```



#### 嵌套子组件

```html
<template>
    <div>
        <h1>Test4 组件</h1>
    </div>
</template>

<script>
    export default {
        name:'test4',
        created(){
            console.log('-----Test4------')
            console.log(this.$attrs) //{status: false}
            console.log(this.$listeners) // {getData: ƒ}
        }
    }
</script>
```

### 6. 跨组件之间传值

> 通过新建一个 ` js` 文件，导入` vue` , 导出` vue`  实例； 然后通过 给导出的实例 上绑定事件` $emit` 事件 , 然后再通过 ` $on` 监听触发的事件，这样就可以达到全局组件数据共享。
>
> 
>
> 
>
> ### 使用场景：
>
> 它可以满足任意场景传递数据， `   父子组件传值` ,  `  子父传值` ,  `  兄弟组件之间传值` ,  `  跨级组件之间传值` . 
>
> #### 通信数据比较简单时，可以采用这种 方案，项目比较庞大，可以采用 `  Vuex` .







#### ` vue .js`

```js
/*
 * @Description: 
 * @Author: ZhangXin
 * @Date: 2021-01-22 15:48:56
 * @LastEditTime: 2021-01-22 15:51:24
 * @LastEditors: ZhangXin
 */

 import Vue from 'vue'

 export default new Vue()
```



#### 组件A 

```html
<!--
 * @Description: 
 * @Author: ZhangXin
 * @Date: 2021-01-22 14:44:17
 * @LastEditTime: 2021-01-22 16:25:33
 * @LastEditors: ZhangXin
-->

<template>
    <div>  
        <button @click="changeValue">改变</button>
    </div>
</template>

<script>
import zxVue from '../util/newVue.js';
    export default {
        name:'person',
        data(){
            return {
                title:'personal 组件',
                status: false
            }
        },
        methods:{
          changeValue(){
             // 通过给 vue实例绑定事件
             zxVue.$emit("getTitle", this.title)   
          }  
        }
    }
</script>



```





#### 组件C

```html
<!--
 * @Description: 
 * @Author: ZhangXin
 * @Date: 2021-01-22 15:07:30
 * @LastEditTime: 2021-01-22 16:26:38
 * @LastEditors: ZhangXin
-->
<template>
  <div>
    <h1>Test4 组件</h1>
    <h1>{{ title }}</h1>
  </div>
</template>

<script>
import zxVue from "../util/newVue";
export default {
  name: "test4",
  data() {
    return {
      title: "test4",
    };
  },
  mounted(){
    // 通过 vue 实例.$on  监听事件名，来接收跨级组件传递过来的值
    zxVue.$on("getTitle", (item) => {
      this.title = item;
      console.log(item)
    });

  }
};
</script>

```





### 7. Vuex

> 这里就不介绍了，完了单独写一篇文章精讲` Vuex`





### 8.   ` provide `   和  `  inject`  实现父组件向子孙孙组件传值。(层级不限)

> ` provide`  和   ` inject`  这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。
>
>  
>
> `provide` :   
>
> - 是一个对象或返回一个对象的函数
> - 该对象包含可注入其子孙的属性。
>
> 
>
> ` inject` :
>
> - 是一个字符串数组 或者是一个对象
> - 用来在子组件或者子孙组件中注入 ` provide` 提供的父组件属性。
>
> 
>
> #### 使用场景：
>
> ` provide/inject可以轻松实现跨级访问父组件的数据`

```js
# provide
//对象
provide:{
    name:'测试'
}
//返回对象的函数
provide(){
    return {
        name: '测试'
    }
}

#inject
inject:['name']
```



#### 父组件

```html
<!--
 * @Description: 
 * @Author: ZhangXin
 * @Date: 2021-01-22 23:24:16
 * @LastEditTime: 2021-01-22 23:49:50
 * @LastEditors: ZhangXin
-->
<template>
    <div>
        <h1>我是父组件</h1>
        <Son />
    </div>
</template>

<script>
import Son from '../components/son/SonOne'
    export default {
        name:'father',
        provide(){
            return {
                titleFather: '父组件的值'
            }
        },
        components:{
            Son
        },
        data(){
            return{
                title:'我是父组件 '
            }
        },
        
    }
</script>


```



#### 子组件

```html
<template>
    <div>
        <h1>我是子孙组件</h1>
       
    </div>
</template>

<script>
import SonTwo from '../son/SonTwo'
    export default {
        name:'sonone',
        components:{
           SonTwo
        },
        inject:['titleFather'],
        created(){
             console.log(`${this.titleFather}-----------SonTwo`)
        },
        data(){
            return{
                title:'我是子组件 '
            }
        },
        
    }
</script>
```

#### 子孙组件

```html
<template>
    <div>
        <h1>我是子孙组件</h1>
       
    </div>
</template>

<script>
import SonTwo from '../son/SonTwo'
    export default {
        name:'sonone',
        components:{
           SonTwo
        },
        inject:['titleFather'],
        created(){
             console.log(`${this.titleFather}-----------SonTwo`)
        },
        data(){
            return{
                title:'我是子组件 '
            }
        },
        
    }
</script>

```

