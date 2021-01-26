# 系统刷JavaScripit 构建前端体系

## 基础篇

### 语法

#### var 变量提升

> 关键字声明的变量会自动提升到区块的作用域顶部。

````
function test(){
    console.log(name);
    var name = '小海' ;  
}

test()  // undefined

#等同于

function tests(){
    var name;
    console.log(name);
    name  = '小海' ; 
}
````

#### let  与 var 区别

> #### 区别 一
>
> `let` 的作用域范围为  块，
>
> `var` 的作用域范围为 函数
>
> #### 区别二
>
> `let` 声明的变量**不会在作用域中变量提升**
>
> #### 区别 三
>
> 使用 `var` 关键字声明的变量，可以成为`window`的属性
>
> 使用 `let` 关键字声明的变量，则不可以成为`window`的属性

````javascript

if(true) {
    var arr  = [1,2,3] 
}
console.log(arr)  //[ 1, 2, 3 ]


if(true) {
    let newArr = [1,2,3] 
}
console.log(newArr)  //newArr is not defined



console.log(age)  //Cannot access 'age' before initialization
let age =  24


var data = '今天是个好日子';
console.log(window.data)  //今天是个好日子

let datas =  '今天是个好日子';
console.log(window.datas)  // undefined
````

#### const  声明

> const   与 let  的行为 一样，区别的是： 使用const 声明时，必须同时初始化变量，且不能修改定义后的变量值，否则会报错。
>
> 但是，const 声明限制 只适用与它指向的变量引用。如果 const 声明的变量是对象，改变它内部的属性是不会违反const 的限制。

````javascript
const  phone = 'IPhone 12';
phone = 'IPhone 13' // 报错
console.log(phone)


const obj = {
    name: '张小飞',
    age:22
}

obj.name = '张飞';

console.log(obj) //{ name: '张飞', age: 22 }
````

#### 数据类型

> ECMAscript 
>
> 原始类型： Undefined,   Null,   Boolean,  Number, String , Symbol
>
> 复杂类型： Object

##### Undefined

> Undefined 值 只有一个，undefined. 
>
> 当定义了变量没有初始化，去使用时，就会报 undefined。

##### Null

> Null 类型也只有一个值， null.
>
> null 值表示空对象指针， 所以使用 typeof 时所以它会返回 Object 。
>
> #### 注意：
>
> 在定义变量时，它的值初始化可以用 null 来 赋值初始化，因为这样就可以保留 null 为空指针语义，从而与 undefined 区分开来。

##### Boolean 

> Boolean 是布尔值类型，它有两个值，true  和 false。
>
> - 要将一个变量值转为Boolean 值，可以使用 Boolean() 函数。

|  数据类型 | 转换为true的值         | 转换为false的值 |
| --------: | ---------------------- | :-------------: |
|   Boolean | true                   |      false      |
|    String | 非空字符串             |   空字符串 ''   |
|    Number | 非零数值(包括无穷数值) |      0 NaN      |
|    Object | 任意对象               |      null       |
| Undefined | N/A                    |    undefined    |

##### Number 类型

###### NAN

> 它指 不是数值，用来返回数值的操作失败了。
>
> - 0 ，  +0，  -0   相除都会返回NaN.
>
> - NaN 不等于包括NaN 在内的任何值。

###### 数值转换

> ECMAscript 提供了 3 个函数可以将非数值转换为数值函数：
>
> - Number()
> - parseInt()
> - parseFloat() 

###### Number()

> Number 函数转换规则
>
> - true 为 1，  false  为 0 
> - 数值，直接返回。
> - null，直接返回 0 
> - undefined  字符串 返回 NaN

````javascript
function dy(data){
    console.log(data)
}

dy(Number(""));   0
dy(Number('Hello'));  NaN
dy(Number('000011'));   11
dy(Number(false))    0
````

##### Symbol

> Symbol 是 原始值， 且它的实例是唯一，不可变的。
>
> 用途： 确保对象属性使用的是唯一标识符，避免发生冲突危险。

###### Symbol初始化

> - 使用Symbol( ) 函数来达到初始化。
> - 可以给Symbol函数 传入一个字符串参数。 作用： 可以用这个字符串来调试代码，但这个字符串参数和Symbol 定义没有关系。
>
> #### 注意：
>
> - Symbol 不能用做构造函数，  new  Symbol()  ❌
> - 全局注册Symbol， 共享Symbol  实例， 通过` Symbol.for（）` 来注册即可。
> - 查询全局注册 Symbol ，可以通过 `Symbol.keyFor( )` ，查询到是全局Symbol，则返回，否则undefined

```javascript
// 创建全局 Symbol
let  name  = Symbol.for('小明');
console.log(Symbol.keyFor(name))
let  oldName = Symbol.for('小明');
console.log(name === oldName)


// 初始化 普通Symbol
let newName =  Symbol('小红')
console.log(Symbol.keyFor(newName))   //undefined   没有查询到 全局Symbol
```

##### Object

> 它是一组数据和功能的集合。通过 new  来创建对象，可以给对象添加属性和方法。
>
> Object 有 以下属性和方法
>
> - constructor :  用于创建当前对象的函数，
> - hasOwnProperty( ) :   用来检测对象本身是否包含某个对象
> - isPrototypeof ( ) :   用来判断当前对象是否为另一个对象的原型
> - propertyIsEnumerable ( ) :  用来判断给定的属性是否 可以 使用 for-in
> - toString：返回对象的字符串
> - valueOf ( ) : 返回对象对应的值



### 作用域

#### 函数参数

> 在ECMAscript 中函数的参数就是局部变量
>
> 当在函数内部，重写了参数，它会变成本地对象指针，而本地对象在函数执行结束时就销毁了。
>
> - 当函数参数为对象时，它是以值传递的，不是以引用传递的。
>
> 下面来看你一个例子

````javascript


function getInfo(obj){
    obj.age = 13;
    obj = new Object()
    obj.age = 22 
}
let  xiaoMing = new Object();
getInfo(xiaoMing)
console.log(xiaoMing.age) //13
````

> 你们猜结果会是啥呢？
>
> 我第一开始以为结果为  22 ， 以引用传递的。
>
> 当我刷了高程4变量这章节才知道，它是以值传递的。
>
> - 创建了 一个对象实例 xiaoming
>
> - 将xiaoming 实例传入函数参数中
>
> - xiaoming.age = 13   ,然后又重写了 xiaoming ，将xiaoming.age = 22， 
>
> - 这块是重写了对象参数，它就会变为本地对象指针，函数执行完，本地对象指针也伴随着销毁了，所以 它 最终的值 还是 以重写之前的值。
>
> - #### 这样说明了 函数对象参数是以值传递的。



#### 确定类型

> 通常我们想知道一个变量的类型为什么类型时，可以通过 `typeof` 判断。
>
> 但它对引用类型没有什么作用，当我们想知道一个对象实例它是什么对象类型时，可以通过 `instanceof` 来判断。

````javascript
var  name = '张三'
var  price = 222

console.log(typeof name)  //string 
console.log(typeof price)  //number

class Student{
    constructor(name,age) {
        this.name = name;
        this.age = age
    }
}

class Animal {
    constructor(name,age) {
        this.name = name;
        this.age = age
    }
}

const A = new Student('菲菲',22)
const B = new Animal('大黄，2')
console.log(A instanceof Animal)  // false
console.log(B instanceof Animal)  // true
````

#### 执行上下文（作用域）

> ##### 什么是执行上下文 ？
>
> - 上下文决定了当前变量或者函数可以访问哪些数据，以及它们的行为
> - 每个上下文都会关联到一个变量对象中，上下文中定义的变量和行为都会存入到这个变量对象中。
> - 在浏览器中，它的上下文为 `window` 对象，并且所有通过 `var`定义的全局变量和`函数`都会成为`window` 对象的属性和行为。  `使用  let  和  const  顶级声明不会定义在 window 对象中。
> - 上下文会在所有代码都执行完后销毁。
>
> 
>
> ##### 上下文执行的流程
>
> - 上下文在运行代码时，会创建变量对象的一个 `作用域链` ，这个作用域链决定了各级上下文中的代码在访问变量和函数时执行的顺序。
> - 如果上下文为函数时，那么它最初只有一个 `作用域链`， 就是`  arguments` (全局上下文中没有这个变量)
> - 它执行的顺序为： 它是通过沿作用域逐级搜索标识符完成。 搜索过程中会以最前顶端开始，然后逐级往后，直到找到标识符，搜索完成。
>
> ##### 上下文执行分类
>
> - 全局上下文
> - 函数上下文
> - 块级上下文
>
> ##### 上下文注意
>
> - 函数 或者 块 的局部上下文 不仅可以访问自己作用域的变量，还可以访问全局的作用域变量。
>   - 全局上下文只能访问全局的变量和函数，不能直接访问局部上下文中的任何数据。



