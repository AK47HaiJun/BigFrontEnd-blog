![](https://user-gold-cdn.xitu.io/2020/3/27/1711c71edd31b4b8?w=800&h=600&f=jpeg&s=151223)

## Vue

### 样式绑定

#### class 使用

> 1.通过数组方式添加样式 
>
> *通过数组方式添加样式   【‘样式名’】 -->* 这里的样式名是提前在CSS中定义好的， 使用 :class绑定使用

```vue
 <h1 :class="['size', 'color',boo?'big':'weight']">添加样式</h1>
```

> 2.*通过添加对象的方式进行判断*   *{‘样式名’,布尔值变量}*

```vue
<h1 :class="[ 'color',{'big':boo}]">添加样式</h1>
```

> 3.通过使用 计算属性来控制样式的显示

```vue
<div v-bind:class="classObject"></div>

data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```



#### style 内联样式使用

> CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名

> 1.*通过往style中传入对象改变样式*
>
> *缺点不能传多个对象*

```vue
<h3 :style="ss">测试</h3>

<script>
       new Vue({
            el: '#fade',
            data:{
                ss:{color:'red','font-size':"50px"},
            }
        })
    </script>

```

> 2. *通过往style中传入数组，在数组中可以放多个对象*

```vue
 <h3 :style="[yy , dd]">测试</h3>

<script>
       new Vue({
            el: '#fade',
            data:{
                dd:{"font-weight":900},
                yy:{color:'blue','font-size':"30px"},
            }
        })
    </script>
```

### 条件渲染

#### v-if  与  v-show

```js
// 两个指令都可以做控制元素渲染。
//区别是： 
1. v-if  是用来控制元素的创建和销毁
2. v-show  是用来控制元素的 display 变化
        
//选择使用：
如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
```

### 列表渲染

#### v-for

> `v-for` 指令基于一个数组来渲染一个列表。
>
> 格式：1. 遍历数组
>
> ​		（ item，index）  in items       
>
> ​		 item 为每一项 ， index为索引，  
>
> 2. 遍历对象
>
>   ​	(value, name, index) in object
>
>   ​         value 为值   name为key    index 为索引
>
> ##### 注意
>
> 1  根据JavaScript 机制 ， in 可以改为 of， 更接近于JavaScript
>
> 2. 遍历项必须绑定key，来确定每个节点的身份

#### 变异方法

```js
//官方含义：会改变调用了这些方法的原始数组。
简单说： 就是改变了原始数组，在原始数组上做一些操作，例如：增加，删除..

// 变异方法包括：
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
```

#### 非变异方法

```js
//所谓非变异方法：不改变原始数组，生成新的数组
// 非变异方法包括：
filter()
concat() 
slice()
....
```

#### 注意

```js
//vm 为Vue实例

var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})

//当你利用索引直接设置一个数组项时
vm.items[indexOfItem] = newValue    ❌错误操作

//官方提供了两种解决办法   
1.  Vue.set
Vue.set(vm.items, indexOfItem, newValue)

2. Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)



//当你修改数组的长度时
vm.items.length = newLength 		❌错误操作

//官方提供了一种解决办法  
1.vm.items.splice(newLength) 
```

#### 对象变更注意

> 有时可能遇到这种需求，在原有data对象属性中，想实现动态添加属性，
>
> 直接添加是，不是响应式的，官方提供了解决办法。

```js
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})

// 添加一个属性 可以使用
Vue.set(vm.userProfile, 'age', 27)

// 为已有对象赋值多个新属性，可以使用 Object.assign()
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

#### 显示过滤 / 排序的结果

> ​	有时，我们想要显示一个数组经过过滤或排序后的版本，而不实际改变或重置原始数据。在这种情况下，可以创建一个计算属性，来返回过滤或排序后的数组。

```js
    <div>
        <div id="main">

            <template v-for='(item,index) in filters' :key=index>
                <li>{{item}}</li>
            </template>
        </div>
    </div>

    <script>
        var vm = new Vue({
            el: '#main',
            data: {
                list:[1,2,3,4,5,6]
            },
            computed: {
                filters: function(){
                    return this.list.filter(item => 
                        item%2 == 0
                    )
                }
            },
        })
    </script>
```

### 事件处理

#### 事件修饰符

> Vue.js 为 `v-on` 提供了**事件修饰符**。对事件的一些操作限制， 修饰符是由点开头的指令后缀来表示的。
>
> - `.stop`
> - `.prevent`
> - `.capture`
> - `.self`
> - `.once`
> - `.passive`
> - `.once`

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

#### 按键修饰符

> 由于Vue 废除了 keyCode 事件， 在开发中，想要获取用户输入的按键，可以自己通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：   具体设置为 https://cn.vuejs.org/v2/api/#keyCodes

```js
// 虽然Vue 废除了 keyCode事件，但是Vue 提供了绝大多数常用的按键码的别名：
 .enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right


<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="自定义事件">
```

### 表单输入绑定

#### 复选框checkbox

> 单个复选框 绑定到布尔值
>
> 多个复选框，绑定到同一个数组

#### 单选按钮radio

> 直接绑定到data中自定义属性中

#### 选择框 select

> v-model 绑定到 select 元素上。
>
> <select v-model="selected">
>  <option disabled value="">请选择</option>
>  <option>A</option>
>  <option>B</option>
>  <option>C</option>
> </select>
>
> 多选时： 绑定到一个数组上

#### 值绑定

> 对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：
>
> <!-- 当选中时，`picked` 为字符串 "a" -->
> <input type="radio" v-model="picked" value="a">
> <!-- `toggle` 为 true 或 false -->
> <input type="checkbox" v-model="toggle">
> <!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
>
> <select v-model="selected">
> <option value="abc">ABC</option>
> </select>

> 把值绑定到 Vue 实例的一个动态属性上，这时可以用 `v-bind` 实现，并且这个属性的值可以不是字符串。

##### 复选框

```js
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>

// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```

##### 单选按钮

```js
<input type="radio" v-model="pick" v-bind:value="a">
// 当选中时
vm.pick === vm.a
```

##### 选择框选项

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>

// 当选中时
vm.selected.number // => 123
```

#### 修饰符

- ##### .lazy

- ##### .number

- ##### .trim

### 组件基础

##### 父组件 向 子组件 传递值  通过在子组件 身上动态绑定传值

```javascript
三部曲：“
1. 先引入 子组件
2. 注册 子组件
3. 使用子组件 传值


例如：

    <template>
        <div>
            <h1>Father  组件</h1>
            <hr>
               // 这块动态传递了一个数组对象
            <Son :list = list  @receiveToParent="receiveToSon"></Son>
        </div>
    </template>
    <script>
    import Son from './Son'
    export default {
      name: 'Father',
      components: {
        Son
      },
      data () {
        return {
          list: [
            { title: '小张中彩票了', author: 'Mr.Liu' },
            { title: '房价跌倒谷底', author: 'Mr.Liu' },
            { title: '阿里云便宜了', author: 'Mr.Liu' }
          ]
        }
      },
      methods: {
        receiveToSon (e) {
          console.log(e)
        }
      }
    }
    </script>

```

#### 子组件向父亲传递值  $emit

```javascript
1. 子组件 通过this.$emit('事件名称'，传递参数)
2. 在父子组件中 通过 在子组件身上 @事件名称 = 自定义的事件  来接收参数

<template>
    <div>
        <h1>Son  组件</h1>
        <template>
            <ul>
                <li v-for="(item,index) of list" :key="index"> <h2>{{item.title}}</h2> <br><h3>{{item.author}}</h3></li>
            </ul>
        </template>
        <Button @click="toParent">子向父 传递值</Button>
    </div>
</template>
<script>
export default {
  name: 'Son',
  //  通过prop 来接收 父组件传递过来的数据
  props: ['list'],
  data () {
    return {
      name: '测试'
    }
  },
  methods: {
    toParent () {
    // 子组件 通过 this.$emit(事件名，要传递的参数)
      this.$emit('receiveToParent', this.name)
    }
  }
}
</script>

```



### 组件探索

##### 组件命名两种格式

```js
1. kebab-case
my-component-name

2. PascalCase
MyComponentName
```

#### Prop

##### Prop 命名格式

> ​	命名使用kebab-case (短横线分隔命名) 命名：
>
> ​	如果 使用 驼峰命名  postTitle ==》 post-title

##### Prop  类型约束

> 默认我们传递时是Prop 为 字符串数组形式；为了更好的管理Prop， 我们可以以对象的形式进行管理。

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}


Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})	
```



##### Prop 传递值

> ​	*任何*类型的值都可以传给一个 prop。





#### 单向数据流

> ​	父级组件状态发生变化，子组件会随着父级组件变化为最新的状态。反过来不可以，子级组件发生变化，父级组件跟真变化，这样Vue会发出警告





#### 禁用Attribute继承

> ​    默认可以给子组件传递任意的  Attribute ，然后子组件接收使用 Attribute。
>
> ​    但有时，我们想限制给子组件传递  Attribute， 那么该怎么做呢？
>
> 1. 首先， 我们应确认子组件要接收哪些Props 
> 2. 然后， 在子组件中 定义这个 属性，  inheritAttrs: false,  则 就禁止Attribute继承
>
> 这样做的目的是为了什么呢？
>
> ​	别人在接手开发时，他只能传递只定的Props，不能传递其它的Props

```js
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```

#### 自定义事件 使用

##### 事件名

> 1.事件名不存在任何自动化的大小写转换。
>
> 2.触发的事件名需要完全匹配监听这个事件所用的名称。
>
> 
>
> Vue 官方推荐 事件命名采用 **kebab-case**

```js
this.$emit('to-A','参数')


<A    @to-A='父级的自定义事件'></A>

methods:{
    receiveSon:(e){
        接收传递过来的参数
        console.log(e)
    }
}
```

#### 插槽

> ​	所谓插槽：就是 在Vue中， 子组件定义了 `<slot></slot>` ， 它就相当于在父组件占据了位置，你可以往里面插入任何数据，可以定义多个slot。
>
> slot 又分为： 具名slot   默认slot  作用域slot 

##### 具名插槽

> ​	所谓具名插槽， 就是 插槽有自己的name， 在子组件中定义好，可以在父组件中通过指定来渲染
>
> ​	格式：  <slot name="自定义"></slot>
>
> ​	
>
> ​	使用:
>
> 1. 先定义好插槽在子组件中
> 2. 在父组件中引入组件，然后在子组件中插入即可
> 3. 3
>
>   注意：
>
>   在使用具名插槽和作用域插槽时，Vue 官方 已经废弃了 slot  和  slot-scope 的使用，  可以 使用 `v-slot` 指令





```js
// 子组件

<template>
  <div>
    //具名插槽
    <slot name="head"></slot>
    <h1>Son 组件</h1>
	// 默认插槽
    <slot></slot> 
    <Button @click="toParent">子向父 传递值</Button>
    <slot name="foot"></slot>
  </div>
</template>




//父组件

<template>
    <div>
        <h1>Father  组件</h1>
        <hr>
        <Son  @receiveToParent="receiveToSon">
            <template v-slot>
                <h1>我是默认插槽</h1>
            </template>
            <template v-slot:head>
                <hr>
                <h1>我是具名插槽  head</h1>
                <hr>
            </template>
            <template v-slot:foot>
                <hr>
                <h1>我是具名插槽  foot</h1>
                <hr>
            </template>
        </Son>

    </div>
</template>
```

##### 作用域插槽

> 有时，我们可能遇到这种需求：
>
> 在插槽内容中能够访问子组件数据，那么就用到作用域插槽。
>
> 简单的说：  父组件在子组件中使用子组件提供的prop数据
>
> 
>
> 如何使用呢？
>
> ​     1.通过 在子组件 slot 中动态绑定数据   `<slot :obj='obj'></slot>`  
>
> 2. 在父组件中使用的方法两种
>
> （1）`v-slot:default = "son"`     使用prop中数据 `{{son.obj.name}}`
>
> （2）`v-slot= "{obj}"`     使用prop中数据 `{{obj.name}}`
>
> ###### 优先采用第二种，这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。
>
> 
>
> 

###### 注意

> `默认插槽的缩写语法不能和具名插槽混用会导致作用域不明确`
>
> `出现多个插槽，可以要使用多个template`

```js
// 子组件
<template>
  <div>
    <slot :obj='obj'></slot>
    <slot :obj='games'></slot>
  </div>
</template>
<script>
export default {
  name: 'Son',
  data () {
    return {
      name: '测试',
      obj: {
        name: '张三',
        age: 22
      },
      games: {
        version: '版本：0.1',
        name: '王者农药'
      }
    }
  },

}
</script>




//父组件
<template>
    <div>
        <h1>Father  组件</h1>
        <hr>
        <Son  @receiveToParent="receiveToSon">
            // 通过v-slot:自定义 = “{}” 使用prop数据
            <template v-slot:default= "{obj}">
                {{obj.name}}
            </template>
             <templatev-slot:other= "{games}">
                {{games.name}}
            </template>
            <template v-slot:head>
                <hr>
                <h1>我是具名插槽  head</h1>
                <hr>
            </template>

        </Son>
    </div>
</template>
```

##### 插槽简写

```js
 <template #head>
                <hr>
                <h1>我是具名插槽  head</h1>
                <hr>
 </template>

```



#### 动态组件

##### 组件上使用 `keep-alive` 来达到缓存效果

> 可能你遇到这种需求，第一次点击了首页，然后点击个人主页，返回的时候，首页组件会被重新加载，浪费必要的性能问题，想解决此类为题，Vue中 提供了 在 组件上 动态绑定 `keep-alive` 来处理

###### 需要在router中设置router的元信息meta

```js
  {
    path: '/',
    name: 'Father',
    component: Father,
    meta: {
      keepAlive: true // 需要缓存
    }
  }
```

###### 使用$route.meta的keepAlive属性进行判断

```js
<el-col :span="24" class="content-wrapper">
  <transition name="fade" mode="out-in">
    //这里需要缓存，使用keep-alive标签包裹
    <keep-alive>
      <router-view v-if="$route.meta.keepAlive"></router-view>
    </keep-alive>
    //这里不需要缓存
    <router-view v-if="!$route.meta.keepAlive"></router-view>
  </transition>
</el-col>

```



##### 


<center><h3>关注微信公众号'前端自学社区' 获取更多资料<h3/><center/>

![](https://user-gold-cdn.xitu.io/2020/3/27/1711c7d8954c9017?w=344&h=344&f=jpeg&s=8820)

<center><h3>回复加群 可以加入 自学前端群<h3/><center/>