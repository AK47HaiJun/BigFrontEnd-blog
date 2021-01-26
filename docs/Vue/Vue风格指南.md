<!--
 * @Description: 
 * @Author: ZhangXin
 * @Date: 2021-01-26 23:02:09
 * @LastEditTime: 2021-01-26 23:03:07
 * @LastEditors: ZhangXin
-->
<img src="https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1671049535,165011712&fm=26&gp=0.jpg">

## Vue 风格指南

### 组件命名

```javascript
Vue.component('todo-item',{
    
})


export default {
    name: 'TodoItem'
}
```

### Vue data 数据必须是一个函数

> 如果``data` 是一个对象时， 所有Vue 实例都公用一份data， 如只想改动某一块，所有模块都会跟着改变。
>
> 如何阻止呢， 实现每个组件实例都可以管理自己的数据？
>
> ``在 JavaScript 中，在一个函数中返回这个对象就可以了：`

```javascript
Vue.component('some-comp', {
  data: function () {
    return {
      foo: 'bar'
    }
  }
})


// In a .vue file
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```

### Prop 定义详细

> 在组件之间传递数据时：
>
> - 父组件 向 子组件 传递数据时，通过 在子组件上动态绑定传值，然后在子组件中，通过Prop 来接收使用即可。	
>
> 
>
> 一般我们会直接在Prop 直接接收父组件动态绑定的key，没有类型约束，这样父组件传递任何类型数据都可以，这就存在一定的缺陷了。 
>
> eg： 例如，某个key 只能传递number类型， 你传递过来的是 string 类型， 这不就尴尬了。
>
> 
>
> ```javascript
> // 正确做法
> props: {
> status: {
>  type: String,   // 类型
>  required: true  // 必填
> }
> }
> ```
>
> 

### Prop 大小写

> **在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。**

```javascript
props: {
  greetingText: String
}


<WelcomeMessage greeting-text="hi"/>
```

### V-for  设置key的必要性

> 在组件上*总是*必须用 `key` 配合 `v-for`，以便维护内部组件及其子树的状态。

```javascript
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

### 永远不要把 v-if 和 v-for 同时用在同一个元素上。

> - 当 Vue 处理指令时，`v-for` 比 `v-if` 具有更高的优先级
> - 一般要用到 `v-for`  与 `v-if`  连用时， 会将 `v-for` 的数据 进行 `computed` 处理后返回对应的数据 来使用

```javascript
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}


<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```



> - `v-for`  与 `v-if` 分开 使用， 一般将 `v-if` 放在 `v-for` 的最外层

```javascript
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```



### 为组件样式设置作用域

> 在为组件写CSS 样式时， 如果不为你单个组件样式添加样式作用域的话，它会影响全局样式。<br>
>
> 在`App.vue` 顶级组件中，它的样式是全局的.
>
> ##### 如何解决单组件样式影响全局呢？ 官方提供了3中解决方案
>
> - 1. ``scoped    Style 中加入 scoped  ``
> - 2. `使用CSS  Modules`  来设定CSS 作用域`
> - 3. `基于 class 的类似 BEM的策略`

```php+HTML
// 1, scoped

<template>
  <button class="button button-close">X</button>
</template>

<!-- 使用 `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>




//2. CSS Modules

<template>
  <button :class="[$style.button, $style.buttonClose]">X</button>
</template>

<!-- 使用 CSS Modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>


//3.BEM

<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- 使用 BEM 约定 -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```

### 单文组件文件命名大小写 驼峰命名

```javascript
1. 第一种方式
components/
|- MyComponent.vue


2. 第二种方式
components/
|- MyComponent.vue

```

### 基础组件命名

> **应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 Base、App 或 V。**

```javascript
components/
|- BaseButton.vue
|- BaseTable.vue



components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue
|- BaseIcon.vue



components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue

```

### 模板中组件名大小写

> 单文件组件模板 ``PascalCas*`
>
> Dom 模板中 `kebab-case `

```javascript
<!-- 在单文件组件和字符串模板中 -->
<MyComponent/>
    
    
<!-- 在 DOM 模板中 -->
<my-component></my-component>



<!-- 在所有地方 -->
<my-component></my-component>

```

### **多个 attribute 的元素应该分多行撰写，每个 attribute 一行。**

```html
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>



<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>

```

### 让计算属性更简单

> - 当每个计算属性都包含一个非常简单且很少依赖的表达式时，撰写测试以确保其正确工作就会更加容易。
> - 简化计算属性要求你为每一个值都起一个描述性的名称，即便它不可复用。这使得其他开发者 (以及未来的你) 更容易专注在他们关心的代码上并搞清楚发生了什么。

```javascript
让 Computed 更精简，直观

computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}

```

### 指令缩写

> 动态绑定属性 `v-bind`  =======    `：`
>
> 动态绑定事件 `v-on` ======= `@`
>
> 动态绑定插槽 `v-slot` ====== `#`
>
> `官方建议： 要么都用简写，要么都用v- 开头的， 统一规范。`

```javascript
<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
>
      
<input
  @input="onInput"
  @focus="onFocus"
>  
      
      
      
绑定插槽
<template #header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>



<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template v-slot:footer>
  <p>Here's some contact info</p>
</template>
```

### 减少在 `scoped` 中使用 元素选择器

> 在 `scoped` 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

```html
❌不要这样使用
<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>





✔正确使用
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

