![](https://user-gold-cdn.xitu.io/2020/3/30/1712bff3b02bc9a3?w=950&h=500&f=png&s=9258)

## Vuex

#### 安装

```javascript
npm install vuex --save

yarn add vuex


// Vuex  依赖 Promise，所有需要安装 es6-promise

npm install es6-promise --save 
yarn add es6-promise 
```

#### Vuex 介绍

> Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

#### store.js 

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 0,
    message: '测试信息'
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})

```



#### State   ------>驱动应用的数据源

##### 1.在组件中 获取store 中的state 数据

```javascript
1. 在组件中 获取store 中的state 数据

	// 一般是 通过计算属性中声明属性，然后使用  this.$store.state.数据源   来获取数据的
  computed: {
    count () {
      return store.state.count
    }
  }

```

##### 如何获取多个state呢？

> 如果想获取多个state数据呢，其实可以在computed 中写多个属性 来获取state，但是当state变化时，会重新求取计算属性，并且触发更新相关 联的 DOM，非常影响性能。
>
> 好在Vuex  提供了 `mapState`  辅助函数，减少不必要的开销

```javascript
1. 首先第一步 必须引入 mapState  
import { mapState } from 'vuex'

2. 在computed 使用即可
computed: mapState({
    counts: state => state.count,
    //counts为自定义的属性： state参数 是stote中的state，然后直接通过state。数据源获取即可
    addNumber (state) {
      return this.numbers + state.count
    }
  })


3. 在页面 直接使用 自定义的属性
```

#### Getter

> 作用类似 computed，但有缓存功能。
>
> 使用场景：  当一个组件需要过滤后的state值，你可以在组件中通过filter进行过滤，那么其它组件也需要过滤后的值，你就的再		      次过滤state。
>
> ​		     当使用getter 处理后，一次处理，多次使用，提高效率

```javascript
export default new Vuex.Store({
  state: {
    count: 0,
    message: '测试信息',
    arr: [1, 2, 3, 4, 5, 6]
  },
  getters: {
    filterArr: state => {
      return state.arr.filter(items => items % 2 === 0)
    }
  }
})
```

##### `mapGetters` 辅助函数

> ​	`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性，它也在 `computed` 中定义使用

```javascript
import { mapGetters } from 'vuex'


export default {
  // ...
  computed: {
  //1. 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}


//2.或者将一个 getter 属性另取一个名字，使用对象形式：

    ...mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
	})

```



##### 在组件中访问getter 值

```javascript
this.$store.getters.自定义的属性

在 其它 组件访问该getters 时，值仍然时  [ 2, 4, 6 ]  过滤后的
```

#### Mutation

> Mutation 是用来更改Store的状态的唯一方法。
>
> 它由 事件类型 和 回调函数构成。回调函数是用来更改state状态的，参数为state
>
> ##### 使用mutation：
>
> 1. 必须在 mutation中注册事件，
> 2. 然后在组件中通过   `store.commit(事件名)`  来 触发改变state的状态
>
> ##### 注意：
>
> ​	1. `mutations` 只能使用在同步的情况下，不能用在异步情况下。

<img src='https://ae01.alicdn.com/kf/Hf5cfcf93bfe04a18b9275df37e7916d2U.gif'>

##### `store.commit  参数`

> store.commit   接收两个参数
>
> ​	第一个参数为： store.mutation中的事件名
>
> ​	第二个参数为： 要传递的  **载荷**

```javascript
mutations: {
  increment (state, n) {
    state.count += n
  }
}


store.commit('increment', 10)


```

##### `mapMutations`辅助函数

> `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。

##### 对象的提交方式

```javascript
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}


mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}store.commit({
  type: 'increment',
  amount: 10
})
```

##### Mutation 使用技巧

> 在多人协作开发时，随着 mutation 的 type 增多，会维护困难。
>
> 官方推荐采用 使用 事件类型，来处理,开发效率提高。

```javascript
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'

// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})


```

##### `mutations` 提交方式

> - 1. 使用 `this.$store.commit('xxx')`
> - 2. `mapMutations` 辅助函数来提交

```javascript
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```



#### Action

> Action 功能类似 Mutation，
>
> ##### Action  支持 异步操作
>
> ##### Action 提交的 mutation, 不是直接修改state状态
>
> Action 接收参数：   与 store 实例具有相同方法和属性的 context 对象

##### 实例

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

##### 在组件中分发Action

```javascript
this.$store.dispatch('xxx')
```

##### 异步执行Action

```javascript
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

##### `mapActions`辅助函数

##### Action 也支持载荷

```javascript
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

#### Module 模块分割

> 当所有状态对象都集中在个大的对象中，store维护将变得困难。这时，可以使用模块分割成多个对象，最后将对象挂载到store的modeule中,Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```javascript
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```



<hr>



## Vuex 使用小结

### 1. 获取 `state`

> 使用 `mapState` 辅助函数，放在 `computed`使用

```javascript
	1. this.$store.state.属性

	2. 辅助函数  `mapState`
    import { mapState } from "vuex";


	 computed: mapState({
    	name: state => state.person.name,
    	car: state => state.person.car,
    	list: state => state.person.list
  	}),
         
    3.直接在组件中使用即可
```



### 2.  Getter

> 作用类似 computed，但有缓存功能。
>
> 使用辅助函数 ` mapGetters`   放在 `computed` 中使用

```javascript
	1. this.$store.getters.属性


	2. 辅助函数 `mapGetters`
    import {  mapGetters } from "vuex";
	  computed: {
    		...mapState({
      			name: state => state.person.name,
      			car: state => state.person.car,
      			list: state => state.person.list
    			}),
    		...mapGetters(["getCarIndexSecond"])
  	},
        
    3. 在组件中可以直接使用 该值
```

### 3. Mutation

> 唯一用来改变 `state` ，它只能执行`同步操作`。
>
> 它提供了 `事件类型`  和 `回调函数`,  回调函数就是要修改 state 状态，并且它会接受 state 作为第一个参数。
>
> `辅助函数  mapMutations` 在 methods 中使用

```javascript
	1. 创建单独的文件来管理 '事件类型' 
		export const CHANGE_CAR = "CHANGE_CAR";

	2.  创建 mutatios
        const mutations = {
          // 事件类型     state状态   v传递的参数/可对象
          [CHANGE_CAR]: (state, v) => {
            state.car.push(v);
          },
          [GET_LIST]: (state, v) => {
            state.list = v;
          }
        };

	3. 组件中触发 mutations，两种方法
    	3.1 this.$store.commit('increment', 10)
		
		3.2 辅助函数 `mapMutations`
        import { mapMutations  } from "vuex";

		 methods: {
             // 通过在 mapMutations 中注册了 mutations， 然后通过事件执行对应	                 mutations即可，
            ...mapMutations(["CHANGE_CAR", "GET_LIST"]),
            buy() {
              this.CHANGE_CAR(this.inCarContent);
            }
          },
```

### 4.  Action

> 它是用来 触发 `mutation` 来改变 `state` 的，它可以处理异步操作，将处理好的值，通过参数传递 给 mutation 来接收改变 `state`
>
> 辅助函数 `mapActions` 在

```javascript
	1. 创建action
    const actions = {
          // 传递一个  commit 参数，用来触发 mutations
          async getLists({ commit }) {
            const response = await axios.get(
              "http://rap2.taobao.org:38080/app/mock/248210/getList"
            );
            // commit('要出发的mutatios'，要传递的值)
            commit("GET_LIST", response.data.todoList);
          }
        };

	2. 组件使用action
    	2.1 this.$store.dispatch('getLists')
		2.2 使用辅助函数 `mapActions`
        methods: {
            ...mapActions(["getLists"]),
            ...mapMutations(["CHANGE_CAR", "GET_LIST"]),
            buy() {
              this.CHANGE_CAR(this.inCarContent);
            }
          },

```





<center><h3>✨觉得不错的点赞，帮忙转发分享以下，原创不易！<h3/><center/>

<br/>
<center><h3>✨关注微信公众号'前端自学社区' 获取更多资料

![](https://user-gold-cdn.xitu.io/2020/3/30/1712bfe32750fbca?w=344&h=344&f=jpeg&s=8820)

<center><h3>💥回复加群 可以加入 自学前端群💥